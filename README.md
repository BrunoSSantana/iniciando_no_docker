# INICIANDO NO DOCKER

Repositório criado com o propósito de auxiliar nos estudos com o docker e prover fonte de consulta referencial para futuras utulizações em projetos pessoais o quaisquer outro.

## O que é Docker

*"Docker é um conjunto de produtos de plataforma como serviço que usam virtualização de nível de sistema operacional para entregar software em pacotes chamados contêineres. Os contêineres são isolados uns dos outros e agrupam seus próprios softwares, bibliotecas e arquivos de configuração."* [- Wikipédia](https://pt.wikipedia.org/wiki/Docker_(software))

**Mas o que seria contêineres?**
De forma resumida um contêiner é um tipon de virtualização onde cada serviço está responssável por um contêiner, garantindo que cada ferramenta trabalhe de forma isolada e com o mínimo de recursos. Dexei um vídeo que fala com mais detalhes o uso de contêineres. O uso de contêineres não é só usado no desenvolvimento, mas também nos testes e em produção.

[![IMAGE ALT TEXT HERE](https://img.youtube.com/vi/-pUZBovqRcU/0.jpg)](https://www.youtube.com/watch?v=-pUZBovqRcU)

O que os contêineres podem apresentar de benefício ao desenvolvimento de aplicações:
- Separação dos processos
- Otimização de recursos
- Imutabilidade / Empacotamento da aplicação
- Facilidade no deploy

## Arquitetura do Docker
![](https://wiki.aquasec.com/download/attachments/2854889/Docker_Architecture.png?version=1&modificationDate=1520172700553&api=v2)

**Client**: A interface usada pelo usuário pra interagir com o Docker, de forma mais resumida, seria o linha de comando.

**Docker Host**: Máquina onde os contêineres estão rodando, na qual o docker daemon, o serviço do docker, gerencia as imagens a qual será a base para criar os contêineres.

**Registry:** Servidor remoto de onde o docker daemon irá fazer o download das imagens.

> Aqui não iremos abordar a instalação do docker, mas deixarei um vídeo onde tbm usei como base para construir esse conteúdo.


[![IMAGE ALT TEXT HERE](https://img.youtube.com/vi/RE31GWJGkwA/0.jpg)](https://www.youtube.com/watch?v=RE31GWJGkwA)
