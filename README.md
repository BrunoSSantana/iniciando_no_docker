# INICIANDO NO DOCKER

Repositório criado com o propósito de auxiliar nos estudos com o docker e prover fonte de consulta referencial para futuras utulizações em projetos pessoais o quaisquer outro.

## Contêineres

Antes de começarmos entender o que é o Docker, precisamos entender o princípio fundamental no qual ele é criado, que é justamente o container.

**Mas o que seria contêineres?**
De forma resumida um contêiner é uma forma de fazer isolamento da parte mais lógica no sentido de usuários, processos network, e outros e dos recursos da máquina, cpu, memória ram, i/o (input - output do disco), garantindo que cada contêiner trabalhe de forma isolada e com o mínimo de recursos. Dexei um vídeo que fala com mais detalhes o uso de contêineres. O uso de contêineres não é só usado no desenvolvimento, mas também nos testes e principalmente em produção para realização do deploy.

[![IMAGE ALT TEXT HERE](https://img.youtube.com/vi/-pUZBovqRcU/0.jpg)](https://www.youtube.com/watch?v=-pUZBovqRcU)

**Quais problemas os contêineres resolvem?**

Ao colocarmos uma aplicação rodando em ambiente produção nos deparamos com os questionamentos:
- Em qual sistema operacional e qual versão a aplicação vai rodar
- Qual versão do interpretador da linguagem
- As versões das bibliotecas

*Dependency Hell*

**O que os contêineres podem apresentar de benefício ao desenvolvimento de aplicações:**
- Separação dos processos
- Otimização de recursos
- Imutabilidade / Empacotamento da aplicação
- Facilidade no deploy

Entre as ferramentas que utilizam a tecnologia de containers, o Docker é a padrão no mercado por n motivos e por tanto iremos trabalhar com ela.

**O que é o Docker?**

Ferramenta de empacotamento de uma aplicação e suas dependências em um container virtual que pode ser executado em um servidor Linux, ou seja, o Docker serve para nos auxilar a administrar containers e os recursos da nossa máquina.

- Ambiente de execução auto-contido
- Kernel compartilhado com Host
- Isolamento dos demais contêineres
- Baixo overhead e tempo de boot

**Mas o Docker é uma MV (máquina virtual)?**

O Dokcer possui menos camadas como podemos ver na imagem abaixo, e diferente da virtualização onde para cada máquina existe um SO e um kernel nos containers e por consequência, no docker não existe isso.

<img height=400 src="https://docker-unleashed.readthedocs.io/_images/virt_docker.png"></img>

[Referência](https://www.youtube.com/watch?v=hCMcQfGb4cA&t)

## Arquitetura do Docker

**Client**: A interface usada pelo usuário pra interagir com o Docker, de forma mais resumida, seria o linha de comando.

**Docker Host**: Máquina onde os contêineres estão rodando, na qual o docker daemon, o serviço do docker, gerencia as imagens a qual será a base para criar os contêineres.

**Registry:** Servidor remoto de onde o docker daemon irá fazer o download das imagens.

> Aqui não iremos abordar a instalação do docker, mas deixarei um vídeo onde tbm usei como base para construir esse conteúdo.

[![IMAGE ALT TEXT HERE](https://img.youtube.com/vi/RE31GWJGkwA/0.jpg)](https://www.youtube.com/watch?v=RE31GWJGkwA)

## Mãos a obra!

Vamos começar por um exemplo bem simples. Com o Docker já instalado, temos uma certa imagem e precisamos executar um container, que comando usar?

```bash
docker run --name nome-do-container  imagem:tag
```

**Mas o que é uma IMAGEM?**

Uma imagem é um modelo/template/"ISO"/"VDI" apenas de leitura (READ ONLY), que é utilizado para subir um container.
Mas nem toda imagem que utilizamos é um "sistema operacional". O Docker permite que construamos nossas próprias imagens e a utilizemos como base para containers, utilizando um arquivo chamado Dockerfile, que a partir dele damos as instruções para ser montado uma imagem que será a base para executarmos nossos containers.

Uma imagem de container é formado por várias camadas formada por cada instrução contida no seu `Dockerfile`. Apenas a última camada é passiva de alteração (READ AND WRITE) e existe apenas em  tempo de execução. O ideal é que um Dockerfile não possua instruções que possam interferir em camadas anteriores, pois, cada camada é isolada uma da outra e essas camadas são READ ONLY.

Docckerfile (exemplo):

```
FROM imagem[:tag] # Imagem que estamos nos baseando

RUN comando # basicamente o que escrevemos em um script bash

WORKDIR /app # diretório raiz para os comandos seguintes

COPY . /app # Copia arquivos para dentro do container

VOLUME /app # Volumes expostos para fora do container

EXPOSE 3000 # Portas liberadas para fora do container

CMD ["comando", "parametros", ...] # Comando que deve ser execuitando assim que um container sobe
```

Após a criação do Dockerfile, vamos construir a nossa imagem com o comando:

```bash
docker -t nome-da-imagem-criada:tag . # este ponto no final representa onde está o arquivo Dockerfile que iremos criar a imagem
```
E para subirmos o container:

```bash
docker -d --name nome-do-container imagem-criada:tag
```

## Containers e seus arquivos

Arquivos vivem e morrem" no contexto dos contêineres, ou seja, caso for gerado alguma informação a partir do uso de determinado container, será perdido depois de matarmos o serviço, para que consigamos salvar tais arquivos, podemos utilizar *mount points*, algo semelhante a uma pasta compartilhada entre o container e a sua máquina, agora ela não será apagada ao matarmos o container.

Vamos entender melhor em um exemplo com o postgresql.

Para isso utilizamos a flag '-v' para conectar os "volumes" e -p para conectar as portas:

```sh
docker run --name postgres \
  -v /mnt/pgdata:/var/lib/postgresql/data/pgdata \
  -e LANG=en_US.utf8 \
  -e PGDATA=/var/lib/postgresql/data/pgdata \
  -e POSTGRES_PASSWORD=postgres \
  -p 5432:5432 -d postgres:9.3.6
```
**IMPORTANTE:**
- -v diretorio-na-sua-maquina:diretorio-no-container**
- "/var/lib/postgresql/data/pgdata" diretório no qual o postgresql armazena seus dados na máquina.
- -p porta-da-sua-maquina:porta-dentro-do-container

## Comandos iniciais úteis
**Subir um container em modo interatico com um terminal**
```bash
docker container run -ti nome-da-imagem
```
**Subir uma container em background**
```bash
docker container run -d nome-da-imagem
```
**Listar containers**
```bash
## em execução
docker container ls
## todos os containers
docker container ls -a
```
**Para container em execução**
```bash
docker container stop identificador-do-container
```
**Iniciar execução do container**
```bash
docker container start identificador-do-container
```
**Reiniciar execução do container**
```bash
docker container restart identificador-do-container
```
**Pausar execução do container**
```bash
docker container pause identificador-do-container
```
**Despausar execução do container**
```bash
docker container unpause identificador-do-container
```
**Apagar container**
```bash
docker container rm identificador-do-container
```
**Apagar container em execução**
```bash
docker container rm -f identificador-do-container
```
**Executar comandos bash via modo interativo:**
```bash
docker container exec -ti identificador-do-container bash
```
**Mostra processos executados no container:**
```bash
docker container exec -ti nome-do-container bash -c 'top -b -n l'
##  mas que pode ser usado também o comando 'ocker stats'
```
**Entrar no container em execução**
```bash
docker container attach identificador-do-containern
```
**Inspecionar container**
```bash
docker container inspect identificador-do-container
```
**Verificar recursos utlizados no container**
```bash
docker container stats identificador-do-container
```
**Verificar processos em execução no container**
```bash
docker container top identificador-do-container
```
**Limitar recurso de memória ao executar um container**
```bash
docker container run -d -m (quantidade de memória em megas)M imagem-docker
```
**Limitar recurso de cpu ao executar um container**
```bash
docker container run -d --cpus (quantidade de núcleos cpu) imagem-docker
## Exemplo: docker container run -d --cpus 0.5 imagem-docker
```

## Atalhos
COMANDO | FUNÇÃO
--------|-------
ctrl + D | sair do bash
ctrl + P + Q | sair do container sem matar o bash

## Likando contêineres

Links servem para que contêineres se comuniquem de fora segura. Para que isso funcione, é importante observar o seguintes pontos:
- Subir contêineres bom a flag `--name` (não é necessário, mas facilita)
- Subir o container que quer se conectar com os outros, com a flag `--link`
- Subir os contêineres na ordem

Exemplo:
```bash
docker run -d --name db training/postgres
docker run -d -p 5000:5000 --name web \ 
--link db training/webapp python app.py
```

Para facilitar a linkagem dos contêineres, é usado o Docker-compose, o orquestrador nativo do docker

## Comandos para limpar a bagunça

> Apagando contêineres que já morreram
```sh
docker rm -v $(docker ps -a -q -f status=exited)
```

> Apagando imagens soltas
```bash
docker rmi $(docker images -f dangling=true -q)
```

> Limpando volumes esquecidos
```bash
docker run -v /var/run/docker.sock:/var/run/docker.sock -v /var/lib/docker:/var/lib/docker -rm martin/docker-cleanup-volumes
```

## Entendendo o Dockerfile

O Docker pode construir imagens automaticamente lendo as instruções de um arquivo `Dockerfile`. O `Dockerfile` é um documento de texto que contém todos os comandos para montagem de uma imagem. Usando o comando `docker build`, os usuários podem iniciar um build que executará as várias instruções contidas no arquivo `Dockerfile`. Para mais detalhes acesse a Docuimentação oficial [aqui](https://docs.docker.com/engine/reference/builder/).

Durante a criação do nosso `Dockerfile` temos que ter em mente que ao criar e niomear nosso arquivo o `Docker` nos dá a liberdade de escoher qualquer nome, toda via, se o arquivo for diferente do nome padrão (`Dockerfile`), teremos que passar o caminho do arquivo no momento do build, como por exemplo: ` docker build -f /path/to/a/Dockerfile .`.

Agora vamos abordar um pouco da sintaxe do Dockerfile que irá orientar a criação dos nossos containers.

### FROM

A instrução `FROM` define a base da imagem que iremos criar, como por exemplo, em projetos nodejs é referenciado `node:tag` e em projetos java seria `openjdk`. A imagem que será baixada por padrão será a do DokcerHub.

Sintaxe padrão:

```dockerfile
FROM [--platform=<platform>] <image>[:<tag>] [AS <name>]
```

### RUN

Com o comando `RUN`, podemos definir quais serão os comandos executados na etapa de criação de camadas da imagem e cada camada gerada por ele poderá ser reutilizada na criação de outras imagens. O comando é executado em um shell, que por padrão é `/ bin / sh -c` no Linux ou `cmd /S /C` no Windows.

Sintaxe Padrão

```
RUN <comando> 
```

ou

```
RUN ["comandos", "em", "array"]
```

### CMD

O comando `CMD` funciona semelhante ao comando `RUN`, com algumas diferenças:

- Executado uma vez por arquivo, caso o arquivo possuir mais de um, a prioridade será do último
- As instruções serão executadas apenas na criação do container que fizer uso da imagem
- Caso passássemos algo como `docker run -it <id da imagem> /bin/bash`, ele sobrescreveria o comando `CMD`  

O ENTRYPOINT funciona bem semenlhante, uma das principais diferenças é que seus parâmetros não são sobrescritos igual ao CMD.
  
### VOLUME

A instrução `VOLUME` cria um ponto de montagem com o nome especificado a qual será compartilhada entre o container e o host. Todo arquivo criado dentro dessa pasta será acessível a partir da máquina host no caminho `/var/lib/docker/volumes`.

#### ADD

O papel do ADD é fazer a cópia de um arquivo, diretório ou até mesmo fazer o download de uma URL de nossa máquina host e colocar dentro da imagem. ADD também tem alguns efeitos interessantes, como: caso o arquivo que esteja sendo passado seja um arquivo de extensão tar, ele fará a descompressão automaticamente.

Sintaxe básica:

```dockerfile
ADD <src>... <dest>
ADD ["<src>",... "<dest>"]
```

### COPY

O `COPY` é semelhante ao `ADD` no que se refere a sintaxe porém e só possui a funcionalidade de copiar arquivos entre o host e o container.

Sintaxe básica:

```dockerfile
COPY <src>... <dest>
COPY ["<src>",... "<dest>"]
```

### WORKDIR

Essa instrução tem o propósito de definir o nosso ambiente de trabalho. Com ela, definimos onde as instruções CMD, RUN, ENTRYPOINT, ADD e COPY executarão suas tarefas, além de definir o diretório padrão que será aberto ao executarmos o container.

Sintaxe básica:

```dockerfile
WORKDIR <path>
```

### EXPOSE

Há uma certa dúvida quanto ao uso dessa instrução. Muitas pessoas pensam que o EXPOSE serve para definir em qual porta nossa aplicação rodará dentro do container, mas na verdade o propósito é servir apenas para documentação.

Essa instrução não publica a porta efetivamente, já que o propósito dela é fazer uma comunicação entre quem escreveu o Dockerfile e quem rodará o container.

*Obs.: material retirado de [Desvendando o DockerFile - Alura](https://www.alura.com.br/artigos/desvendando-o-dockerfile)
Mais detalhes em [Dockerfile reference](https://docs.docker.com/engine/reference/builder/)*
  






