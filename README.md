# Conversão de temperatura

![GitHub repo size](https://img.shields.io/github/repo-size/Caiuzu/conversao-temperatura)
![ViewCount](https://views.whatilearened.today/views/github/Caiuzu/conversao-temperatura.svg)

###### Developed by: https://iniciativakubernetes.com.br/ | [@caiuzu](https://github.com/Caiuzu/)

---

### Objetivo:

> Este projeto tem por intuito introduzir o programador as tecnologias de containerização.

## AULA 1 – CONTAINERS E DOCKER SIMPLIFICADOS

_Nessa aula, você vai conhecer as oportunidades que giram em torno do universo dos containers e Kubernetes e que estão
disponíveis para você. Além disso, você já vai ser capaz de executar uma API no seu próprio container Docker, mesmo que
você nem tenha experiência com a ferramenta. Ao final da aula, tem um desafio para você exercitar o que já aprendeu e
testar os seus conhecimentos._

---

## 1. Comandos:

- #### Realizando o build, criando imagem em diretório local (.):
    ```shell
    docker image build -t caiuzu/conversao-temperatura:v1.0.0 .
    ```

- #### Executando container:
    ```shell
    docker run -d -p 8080:8080 --name conversao-temperatura caiuzu/conversao-temperatura:v1.0.0
    ```

- #### Criando tag latest:
    ```shell
    docker tag caiuzu/conversao-temperatura:v1.0.0 caiuzu/conversao-temperatura:latest
    ```

- #### Parando um container:
    ```shell
    docker stop ${CONTAINER_ID}
    ```

- #### Iniciando um container:
    ```shell
    docker start ${CONTAINER_ID}
    ```

- #### Apagando container:
    ```shell
    docker container rm -f ${CONTAINER_ID}
    ```

- #### Habilitando terminal do container em execução:
    ```shell
    docker container exec -it ${CONTAINER_ID}
    ```

- #### Verificando logs do container: (stdout)
    - Verificação stática
      ```shell
      docker container logs ${CONTAINER_ID}
      ```

    - Verificação continua
      ```shell
      docker container logs -f ${CONTAINER_ID}
      ```

    - Verificação com data e hora
      ```shell
      docker container logs -t ${CONTAINER_ID}
      ```

---

## 2. EndPoint:

- Aplicação:

> http://localhost:8080/celsius/1/fahrenheit

- Swagger:

> http://localhost:8080/api-docs/#/

---

## 3. Opções de comando no Dockerfile:

- **FROM**: Inicializa o build de uma linguagem a partir de uma imagem base
- **RUN**: Executa um comando
- **LABEL**: Adiciona metadados a imagem
- **CMD**: Define o comando e/ou os parâmetros padrões
- **EXPOSE**: Define que o container precisa expor a porta em questão
- **ARG**: Define um argumento para ser usado no processo de construção
- **ENV**: Define variáveis de ambiente
- **ADD**: Copia arquivos ou diretórios e arquivos remotos e adiciona ao sistema de arquivos da imagem
- **COPY**: Copia aquivos ou diretórios e adiciona ao sitema de arquivos da imagem
- **ENTRYPOINT**: Ajuda a configurar um contêiner que pode ser executado como um executável
- **VOLUME**: Define volumes que devem ser definidos
- **WORKDIR**: Define o seu diretório corrente

-----------
