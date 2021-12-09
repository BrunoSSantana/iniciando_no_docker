# INICIANDO NO DOCKER

Repositório criado com o propósito de auxiliar nos estudos com o docker e prover fonte de consulta referencial para futuras utulizações em projetos pessoais o quaisquer outro.

## Contêineres

Antes de começarmos entender o que é o Docker, precisamos entender o princípio fundamental no qual ele é criado, que é justamente o container.

**Mas o que seria contêineres?**
De forma resumida um contêiner é um tipon de virtualização onde cada serviço está responssável por um contêiner, garantindo que cada ferramenta trabalhe de forma isolada e com o mínimo de recursos. Dexei um vídeo que fala com mais detalhes o uso de contêineres. O uso de contêineres não é só usado no desenvolvimento, mas também nos testes e em produção.

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

Entre as ferramentas que utilizam a tecnologia de contêineres, o Docker é a padrão no mercado por n motivos e por tanto iremos trabalhar com ela.

**O que é o Docker?**

Ferramenta de empacotamento de uma aplicação e suas dependências em um container virtual que pode ser executado em um servidor Linux.
- Ambiente de execução auto-contido
- Kernel compartilhado com Host
- Isolamento dos demais contêineres
- Baixo overhead e tempo de boot

**Mas o Docker é uma MV (máquina virtual)?**

De forma resumida, o Dokcer possui menos camadas como podemos ver na imagem abaixo, mas podemos tratar como um tipo de virtualização.

![arquitetura docker x vm](https://docker-unleashed.readthedocs.io/_images/virt_docker.png)

[- Ref](https://www.youtube.com/watch?v=hCMcQfGb4cA&t)

## Arquitetura do Docker

**Client**: A interface usada pelo usuário pra interagir com o Docker, de forma mais resumida, seria o linha de comando.

**Docker Host**: Máquina onde os contêineres estão rodando, na qual o docker daemon, o serviço do docker, gerencia as imagens a qual será a base para criar os contêineres.

**Registry:** Servidor remoto de onde o docker daemon irá fazer o download das imagens.

![](https://wiki.aquasec.com/download/attachments/2854889/Docker_Architecture.png?version=1&modificationDate=1520172700553&api=v2)

> Aqui não iremos abordar a instalação do docker, mas deixarei um vídeo onde tbm usei como base para construir esse conteúdo.

[![IMAGE ALT TEXT HERE](https://img.youtube.com/vi/RE31GWJGkwA/0.jpg)](https://www.youtube.com/watch?v=RE31GWJGkwA)

## Mãos a obra!

Vamos começar por um exemplo bem simples. Com o Docker já instalado, temos uma certa imagem e precisamos executar um container, que comando usar?

```bash
docker run --name nome-do-container  imagem:tag
```

**Mas o que é uma IMAGEM?**

Uma imagem é um modelo/template/"ISO"/"VDI" apenas de leitura, que é utilizado para subir um container.
Mas nem toda imagem que utilizamos "é um sistema operacional". O Docker permite que construamos nossas próprias imagens e a utilizemos como base para containeres, utilizando um arquivo chamadi Dockerfile, que a partir dele damos as instruções para ser montado uma imagem que será a base para executarmos nossos containeres.

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

## Contêineres e seus arquivos

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

```bash
docker exec -ti nome-do-container bash
```

Com esse comando, o docker abre um novo bash dentro do container.

```bash
docker exec -ti nome-do-container bash -c 'top -b -n l'
```

Comando mostra os processos sendo executados no container mas que pode ser usado também o comando `docker stats`.

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



