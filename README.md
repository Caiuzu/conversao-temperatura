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

## 4. Kubernetes:

- ### Configurando Kubernetes em ambiente local:

[//]: # (TODO: Formatar corretamente a documentação referente a kubernetes)

<details>
<summary>Configurando Kubernetes em ambiente local:</summary>

1. instalar o https://k3d.io/v5.4.1/

- wget -q -O - https://raw.githubusercontent.com/k3d-io/k3d/main/install.sh | bash

2. instalar o kuubectl

- https://kubernetes.io/docs/tasks/tools/
    - curl -LO "https://dl.k8s.io/release/$(curl -L -s https://dl.k8s.io/release/stable.txt)/bin/linux/amd64/kubectl"
    - curl -LO "https://dl.k8s.io/$(curl -L -s https://dl.k8s.io/release/stable.txt)/bin/linux/amd64/kubectl.sha256"
    - echo "$(cat kubectl.sha256)  kubectl" | sha256sum --check
    - sudo install -o root -g root -m 0755 kubectl /usr/local/bin/kubectl
    - kubectl version --client
- ou https://kubernetes.io/docs/tasks/tools/install-kubectl-linux/#install-using-native-package-management

3. Criatemos os clusuter com k3d

- verrificaremos se nosso serviço de docker está inicializado
    - docker ps
- caso não esteja, iniciar com o comando abaixo para WSL:
    - sudo /etc/init.d/docker start
- comando: k3d cluster create
    - será criado com 1 container representando uma máquina, já configurando o kuubectl para comuunicação com o clusuter

- listaremos nossos nodes de kuubernetes
    - kubectl get nodes
        - Obs: notaremos que em ROLE, o control plane será executado no container, porém em ambientes de produção o
          mesmo não ocorre e nem deve ocorrer, apenas em ambientes de testes, para consumir menos recursos

- listaremos os container docker:
    - docker ps
        - Notamos que haverá o k3d-proxy (que faz a conexão entre nodes), mesmo que sejam criados uum node, pois ele é
          criado por padrão

- Criaremos um novo node com um nome e sem load balance (--no-lb)
    - k3d cluster create meucluster --no-lb

- listaremos todos cluester
    - k3d cluster list

- deletando um cluster:
    - k3d cluster delete nome_cluster

- criando um cluster com X servers (control plane) e X agents (nodes), algo mais proximo de um ambiente de prod, mais
  detalhado
    - k3d cluster create meucluster --servers 3 --agents 3

---------

Cada pod é como uma perna do balance, um node Agora iremos preparar nossos deploys e configgurar apartir de um manifesto
-> arquivo.yml

executar nosso deploy apartir do manifesto:

- kubectl apply -f deployment.yml

listar pods:

- kubectl get pods
- serão listados os pods exemplo: nome_do_pod-32ij2

exibir todas informações sobre um pod

- kubectl describe pod nome_do_pod-32ij2

listar replicaset:

- kubectl get replicaset

deletar replicaset:

- kubectl delete -f replicaset.yml

port-foward para acessar a aplicação através de um pod especifico na porta definida desejada:

- kubectl port-forward pod/nome_do_pod-32ij2 8080:80
- basta acessar localhost:8080

deletar um pod (ele será recriado automaticamente porém atualizado se estiver com replicaset):

- kubectl delete pod nome_do_pod-32ij2

histórico de deploys:

- kubectl rollout history deployment meu-deployment

parra realizar um rollback de versões de deploy, basta executar o comando:

- kubectl rollout undo deployment meu-deployment

----

Para não termos mais quue utilizar o port-foward, tendo um balanceamento e um ponto unico, utilizaremos os services para
expor a app:

Services existentes:

- ClusterIP: uutilizado internamente, é gerado uum ip porém o kubernetes resolve atrravés do nome (nunca utilize o
  IP!!!)
- NodePort: pode ser acessado externamente, ele utiliza uma porta no range padrão 30000-32767, sendo acessado de
  qualquer maquina de meu cluster kubernetes, utilizaremos qualquer ip de nosso cluster kubernetes
- É muito utilizado em cenários on-premisses e local
- LoadBalance:
- É muito utilizado em 'as service', cenários de nuvem

executar nosso deploy apartir do manifesto:

- kubectl apply -f deployment.yml

------------

recriando cluster com port bind

- criando um cluster com X servers (control plane) e X agents (nodes), e com port bind
    - k3d cluster create meucluster --servers 3 --agents 3 -p "8080:30000@loadbalancer"

</details>
