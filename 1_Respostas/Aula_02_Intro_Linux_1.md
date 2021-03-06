1. Por que o Linux recebeu esse nome?


O nome vem direto do seu criador, Linus Torvalds. O nome é uma junção dos nomes Linus + UNIX.


2. O que são daemons?


Daemon é um software que fica executando sem o controle direto do usuário.


3. O que é o shell?


O Shell (concha) é um programa que permite ao usuário iteragir com o sistema operacional através de comandos digitados do teclado.


4. Por que é importante evitar executar o terminal como super-usuário?


Super-usuário ou Root (raiz) refere-se a um tipo de usuário com privilégios absolutos. Com o Root é possível ter controle total sobre o sistema (executar comandos absolutos e ter acesso a processos), o que pode ser muito interessante e também muito perigoso. Portanto o mais recomendado não é logar como Root, e sim utilizar comandos como **su** e **sudo**. O primeiro loga como Root e outr usuário em um terminal, podendo depois fazer logoff com o comando **exit**, o segundo apenas executar um comando em um terminal com privilégios de super usuário.   


5. Qual botão do teclado completa o que o usuário escreve no terminal, de acordo com o contexto?

- **TAB**: TAB autocompleta a linha de um texto. 

6. Quais botões do teclado apresentam instruções escritas anteriormente?

- **Seta para cima/Seta para baixo**.

7. Apresente os respectivos comandos no terminal para:

 (a) Obter mais informações sobre um comando.
```
	man #nome_do_comando
```
 (b) Apresentar uma lista com os arquivos dentro de uma pasta.

``` 
	$ ls
```

 (c) Apresentar o caminho completo da pasta.

```
	$ pwd
```

 (d) Trocar de pasta.

- Ir para o diretório home do usuário logado:

```
	$ cd
```
- Ir para o diretório acima do atual: 

```
	$ cd ..
```
- Voltar para o diretório que estávamos antes do atual:

```
	$ cd -
```

- Mudar para o diretório raiz do sistema:

```
	$ cd /
```
- Ir para o diretório home do usuário logado:

```
	$ cd ~
```
- Estando dentro de um diretório, deseja-se entrar em outro pertecente ao diretório atual:

```
	$ cd #nome_da_pasta
```
- Entrando em um diretório que não pertence ao diretório atual:

```
	$ cd #caminho_para_diretorio_que_se_deseja_entrar
```

 (e) Criar uma pasta.

``` 
	$ mkdir #nome_da_pasta
```

 (f) Apagar arquivos definitivamente.

- Arquivo se encontra no diretório atual:

```
	$ rm #nome_do_arquivo
```

ou 

```
	$ rm arquivo1 arquivo2 arquivo3
```

- Arquivo se encontra em outra pasta:

```
	$ rm #caminho_para_arquivo
```
- Apagando arquivos com a mesma extensão:

```
	$ rm *.txt
```

 (g) Apagar pastas definitivamente.

- Apagar pasta inteira recursivamente: 

```
	$ rm -rf nomedapasta/
```

- Apagar arquivos e pastas do diretório atual:

```
	$ rm -rf *
```

 (h) Copiar arquivos.

- Copiar arquivo passwd do diretório /etc para o diretório /home/leonardo

```
	$ cp /etc/passwd /home/leonardo/
```
- Copiar o arquivo passwd do diretório /etc/ para o diretório /home/leonardo renomeando a cópia como usuarios.txt

```
	$ cp /etc/passwd /home/leonardo/usuarios.txt
```

- Copiar todos os arquivos cujo nome se inicia com a letra l no diretório /lib/ para o diretório atual:

```
	$ cp /lib/l*.
```

 (i) Copiar pastas.

- Copiar o diretório /home/leonardo/Downloads e todo o seu conteúdo recursivamente para o diretório /home/leonardo/Backup:

```
	$ cp -r /home/leonardo/Downloads /home/leonardo/Backup
```


 (j) Mover arquivos.

- Mover o arquivo passwd do diretório atual para o subdiretório Documentos:

```
	$ mv passwd ./Documentos/
```

 (k) Mover pastas.

- Mover todo o conteúdo dentro da pasta (arquivos e pastas) de teste1 para teste2. 

```
	$ mv teste1/* /home/leonardo/Documentos
```

 (l) Renomear pastas.

```
	$ mv pasta1 pasta2
```

 (m) Apresentar o conteúdo de um arquivo.

```
	$ cat #nome_do_arquivo
```
 (n) Apresentar o tipo de um arquivo.

```
	$ file arquivo
```

 (o) Limpar a tela do terminal.

```
	$ clear
```
 (p) Encontrar ocorrências de palavras-chave em um arquivo-texto.

```
	$ cat arquivo.txt | grep "String"
```
 (q) Ordenar informações em um arquivo-texto.

```
	$ sort [opcoes][arquivo] 
```


 (r) Substituir ocorrências de palavras-chave em um arquivo-texto.

- O comando sed:

```
	$ sed 's/palavra_p_substituir/palavra_nova/'
```
 (s) Conferir se dois arquivos são iguais.

```
	$ diff arquivo1.txt arquivo2.txt
```

 (t) Escrever algo na tela

```
	$ echo 'O que você quiser escrever.'
```
