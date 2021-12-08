# INICIANDO NO DOCKER

Repositório criado com o propósito de auxiliar nos estudos com o docker e prover fonte de consulta referencial para futuras utulizações em projetos pessoais o quaisquer outro.

## O que é Docker

*"Docker é um conjunto de produtos de plataforma como serviço que usam virtualização de nível de sistema operacional para entregar software em pacotes chamados contêineres. Os contêineres são isolados uns dos outros e agrupam seus próprios softwares, bibliotecas e arquivos de configuração."* [- Wikipédia](https://pt.wikipedia.org/wiki/Docker_(software))

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

de forma resumida, o Dokcer possui menos camadas como podemos ver na imagem abaixo, mas podemos tratar como um tipo de virtualização.

[- Ref](https://www.youtube.com/watch?v=hCMcQfGb4cA&t)

![arquitetura docker x vm](https://docker-unleashed.readthedocs.io/_images/virt_docker.png)

## Arquitetura do Docker
![](https://wiki.aquasec.com/download/attachments/2854889/Docker_Architecture.png?version=1&modificationDate=1520172700553&api=v2)

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

