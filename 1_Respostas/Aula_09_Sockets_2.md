1. Crie dois programas em C para criar um cliente e um servidor. Faça com que o cliente envie os valores 1, 2, 3, 4, 5, 6, 7, 8, 9 e 10 para o servidor, com intervalos de 1 segundo entre cada envio. Depois de o cliente enviar o número 10, ele aguarda 1 segundo e termina a execução. O servidor escreve na tela cada valor recebido, e quando ele receber o valor 10, ele termina a execução.
```C
///////////////////////////////////////////////////////////////////////////////////
//                    CODIGO CLIENTE
///////////////////////////////////////////////////////////////////////////////////
#include <stdio.h>
#include <stdlib.h>
#include <string.h>
#include <sys/socket.h>
#include <sys/un.h>
#include <unistd.h>

int main (int argc, char* const argv[])
{
	char *socket_name;
	char *mensagem;

	int socket_id;
    
	struct sockaddr name;
	
    int length;

	socket_name = argv[1];// O ARGUMENTO COM O NOME


        
	fprintf(stderr, "Abrindo o socket local... ");
	socket_id = socket(PF_LOCAL, SOCK_STREAM,0);   //DESCRITOR DO SOCKET
	fprintf(stderr, "Feito!\n");

	fprintf(stderr, "Conectando o socket ao servidor no endereco local \"%s\"... ", socket_name);
    
//     DEFININDO ESTRUTURA sockaddr
	name.sa_family = AF_LOCAL;
	strcpy(name.sa_data, socket_name);
	
    
    connect(socket_id, &name, sizeof(name));    //ASSOCIANDO O DESCRITOR A ESTRUTURA, QUE TEM O ENDERECO
    
	fprintf(stderr, "Feito!\n");

	fprintf(stderr, "Mandando mensagem ao servidor... ");

//    length = strlen(mensagem) + 1;//TAMANHO DA mensagem
	//MENSAGEM SERÁ OS NUMEROS
    for(int n = 1; n <= 10; n++)
    {
 /*       length = sizeof(n) + 1;//TAMANHO DA mensagem
        write(socket_id, &length, sizeof(length)); //ESCREVENDO O TAMANHO DA MENSAGEM
        */
        write(socket_id, &n, sizeof(int));    //ESCRENDO A MENSAGEM
        sleep(1);
    }
    fprintf(stderr, "Feito!\n");

	fprintf(stderr, "Fechando o socket local... ");
	close(socket_id);
	fprintf(stderr, "Feito!\n");
	return 0;
}


///////////////////////////////////////////////////////////////////////////////////
//                    CODIGO SERVIDOR
///////////////////////////////////////////////////////////////////////////////////

// Servidor Local
#include <stdio.h>
#include <stdlib.h>
#include <unistd.h>
#include <string.h>
#include <sys/socket.h>
#include <sys/un.h>
#include <signal.h>


char socket_name[50];
int socket_id;
void sigint_handler(int signum);
void print_client_message(int client_socket);
void end_server(void);

int main (int argc, char* const argv[])
{
	struct sockaddr socket_struct;

		strcpy(socket_name, argv[1]);	//COPIANDO O NOME DA ENTRADA DE ARGUMENTO DO TERMINAL


	fprintf(stderr, "Definindo o tratamento de SIGINT... ");
	signal(SIGINT, sigint_handler);	//ASSOCIANDO A FUNÇÃO PARA SER CHAMADA COM O "CTRL+C"
	fprintf(stderr, "Feito!\n");
	
	fprintf(stderr, "Abrindo o socket local... ");
	socket_id = socket(PF_LOCAL, SOCK_STREAM, 0);	//RETORNA O DESCRITO É COMO ABRIR UM ARQUIVO
	fprintf(stderr, "Feito!\n");

	fprintf(stderr, "Ligando o socket ao endereco local \"%s\"... ", socket_name);
//     DEFININDO A ESTRUTURA ""SOCKADDR""
	socket_struct.sa_family = AF_LOCAL;		//FAMILY
	strcpy(socket_struct.sa_data, socket_name);	//NAME


		/*Quando	criado,	um	socket	não	tem	um	endereço	local	e	nem	um	
		endereço	remoto.	Um	servidor	usa	o	procedimento	bind	para	
		prover	um	número	de	porta	de	protocolo	em	que	o	servidor	
		esperará	por	contato.*/
		
		// """SOCKET_STRUCT""" É DO TIPO """SOCKET_ADDR""" |estrutura que especifica	o endereço local a ser 			atribuído	ao	socket	(no	formato	sockaddr, sockaddr_in)	
		// NESSA ESTRTURA TEMOS O """SA_FAMILY""" E O """NAME"""

	bind(socket_id, &socket_struct, sizeof(socket_struct));

	fprintf(stderr, "Feito!\n");


	fprintf(stderr, "Tornando o socket passivo (para virar um servidor)... ");

		/*À	medida	que	chegam	requisições	de	clientes,	elas	são	inseridas	na	fila;	
		quando	o	servidor	pede	para	recuperar	uma	requisição	recebida	do	socket,	o	
		sistema	retorna	a	próxima	requisição	da	fila.	*/
		//O NUMERO 10 DEFINE =>>> queuesize:	comprimento	para	a	fila	de	requisição	do	socket	
	
	listen(socket_id, 10);

	fprintf(stderr, "Feito!\n");

	while(1)
	{
		struct sockaddr cliente;
		int socket_id_cliente;

		socklen_t cliente_len;

		fprintf(stderr, "Aguardando a conexao de um cliente... ");

		/*newsock = accept(socket_id, caddress, caddresslen)
		• socket_id:	descritor	de	um	socket	que	após	o	bind
		• cadderss:	endereço	de	uma	estrutura	do	tipo	sockaddr
		• caddresslen:	ponteiro	para	um	inteiro	*/

		socket_id_cliente = accept(socket_id, &cliente, &cliente_len);
		fprintf(stderr, "Feito!\n");

		fprintf(stderr, "Obtendo a informacao transmitida pelo cliente...");
	
		print_client_message(socket_id_cliente);
		fprintf(stderr, "Feito!\n");

		fprintf(stderr, "Fechando a conexao com o cliente... ");
		close(socket_id_cliente);
		fprintf(stderr, "Feito!\n");
	}
	return 0;
}


/////////////////////////////////////////////////////////////////////////////////////////////						////
								//ENCERRAR PELO TERMINAL
/////////////////////////////////////////////////////////////////////////////////////////////

void sigint_handler(int signum)
{
	fprintf(stderr, "\nRecebido o sinal CTRL+C... vamos desligar o servidor!\n");
	end_server();
}

/////////////////////////////////////////////////////////////////////////////////////////////						////
								//MOSTRAR MENSAGEM DO CLIENTE
/////////////////////////////////////////////////////////////////////////////////////////////

void print_client_message(int client_socket)
{
	int length;
	int entrada;
	fprintf(stderr, "\nMensagem enviada pelo cliente tem ");

//	for(int n = 0; n < lenght; n++)
	do
	{
//	read(client_socket, &length, sizeof (length));
//	fprintf(stderr, "%d bytes.", length);
//	text = (int*) malloc (length);
	read(client_socket, &entrada, sizeof(int));
	fprintf(stderr,"\n\n   Mensagem = %d\n\n", entrada);
	} while(entrada != 10);
	if (entrada == 10)
	{
		//free (entrada);
		fprintf(stderr, "Cliente pediu para o servidor fechar.\n");
		end_server();
	}
//	free (entrada);
}

/////////////////////////////////////////////////////////////////////////////////////////////						////
								//END OF SERVIDOR
/////////////////////////////////////////////////////////////////////////////////////////////
void end_server(void)
{
	fprintf(stderr, "Apagando \"%s\" do sistema... ", socket_name);
	unlink(socket_name);	//APAGA O NOME "LINK"
	fprintf(stderr, "Feito!\n");
	fprintf(stderr, "Fechando o socket local... ");
	close(socket_id);	//FECHA O SOCKET
	fprintf(stderr, "Feito!\n");
	exit(0);
}
```
```BASH
/////////////////////////////////////////////////////////////////////////////////////////////						////
								//CLIENTE
/////////////////////////////////////////////////////////////////////////////////////////////
marcosadriano@marcosadriano:~/Área de trabalho/Aula_09$ ./main.exe /tmp/socket1 "as"
Abrindo o socket local... Feito!
Conectando o socket ao servidor no endereco local "/tmp/socket1"... Feito!
Mandando mensagem ao servidor... Feito!
Fechando o socket local... Feito!

/////////////////////////////////////////////////////////////////////////////////////////////						////
								//SERVIDOR
/////////////////////////////////////////////////////////////////////////////////////////////
marcosadriano@marcosadriano:~/Área de trabalho/09_Sockets$ ./mainn.exe /tmp/socket1
Definindo o tratamento de SIGINT... Feito!
Abrindo o socket local... Feito!
Ligando o socket ao endereco local "/tmp/socket1"... Feito!
Tornando o socket passivo (para virar um servidor)... Feito!
Aguardando a conexao de um cliente... Feito!
Obtendo a informacao transmitida pelo cliente...
Mensagem enviada pelo cliente tem 

   Mensagem = 1



   Mensagem = 2



   Mensagem = 3



   Mensagem = 4



   Mensagem = 5



   Mensagem = 6



   Mensagem = 7



   Mensagem = 8



   Mensagem = 9



   Mensagem = 10

Cliente pediu para o servidor fechar.
Apagando "/tmp/socket1" do sistema... Feito!
Fechando o socket local... Feito!
```

2. Crie dois programas em C para criar um cliente e um servidor. Execute a seguinte conversa entre cliente e servidor:

```
CLIENTE: Pai, qual é a verdadeira essência da sabedoria?
SERVIDOR: Não façais nada violento, praticai somente aquilo que é justo e equilibrado.
CLIENTE: Mas até uma criança de três anos sabe disso!
SERVIDOR: Sim, mas é uma coisa difícil de ser praticada até mesmo por um velho como eu...
```

Neste exercício, o cliente deve escrever no terminal as mensagens enviadas e recebidas.