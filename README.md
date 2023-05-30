# Este projeto demonstra o uso do Quarkus com o Red Hat AMQ Broker

  Veja a seguir nas seções de acordo com o seu foco **DEV** ou **OPS**

## DEV

* Este projeto usa Quarkus, o Supersonic Subatomic Java Framework.
* Se quiser saber mais sobre o Quarkus, visite o site: https://quarkus.io/ .

#### Executando o aplicativo no modo dev
> Você pode executar seu aplicativo no modo dev que permite a codificação ao vivo usando:
```shell script
./mvnw compile quarkus:dev
```
> **_NOTE:_**  O Quarkus agora vem com uma Dev UI, que está disponível no modo dev apenas em http://localhost:8080/q/dev/.

#### Empacotando e executando o aplicativo

> O aplicativo pode ser empacotado usando:
```shell script
./mvnw package
```
* Ele produz o arquivo `quarkus-run.jar` no diretório `target/quarkus-app/`.
* Esteja ciente de que não é um _uber-jar_ pois as dependências são copiadas para o diretório `target/quarkus-app/lib/`.

O aplicativo agora pode ser executado usando `java -jar target/quarkus-app/quarkus-run.jar`.

Se você deseja construir um _uber-jar_, execute o seguinte comando:
```shell script
./mvnw package -Dquarkus.package.type=uber-jar
```

O aplicativo, empacotado como um _uber-jar_, agora pode ser executado usando `java -jar target/*-runner.jar`.

#### Criando um executável nativo

> Você pode criar um executável nativo usando: 
```shell script
./mvnw package -Pnative
```

> Ou, se você não tiver o GraalVM instalado, pode executar o build executável nativo em um contêiner usando:: 
```shell script
./mvnw package -Pnative -Dquarkus.native.container-build=true
```

> Você pode então executar seu executável nativo com: `./target/quarkus-artemis-1.0.0-SNAPSHOT-runner`

> Se você quiser saber mais sobre como criar executáveis nativos, consulte https://quarkus.io/guides/maven-tooling.

#### Criação e implantação

> [Guide: DEPLOYING ON OPENSHIFT](https://quarkus.io/guides/deploying-to-openshift)

```shell script
./mvnw install -Dquarkus.kubernetes.deploy=true
```

## OPS

* Para fazer o deploy usando uma imagem existente, seguir os passos a seguir:

### Crie o App Config/Secret no Cluster

> Aqui você irá criar arquivos apontando para o cluster AMQ que você já possua instalado e configurado.

```shell script
## create a project if you don't have one
$ oc new-project myquarkus

$ oc create secret generic artemis-secret --from-literal="QUARKUS_ARTEMIS_PASSWORD=${BROKER_CREDENTIAL_PASSWORD}"

$ oc create configmap artemis-conf --from-literal=QUARKUS_ARTEMIS_URL="(tcp://${BROKER_SVC_ACCEPTOR_TCP}:61616)" --from-literal="QUARKUS_ARTEMIS_USERNAME=${BROKER_CREDENTIAL_USERNAME}"
```

### Implantar com uma imagem existente
```shell script
$ oc create -f k8s/deploymentConfig.yaml
```

# Referências e Guias

- OpenShift ([guide](https://quarkus.io/guides/deploying-to-openshift)): Generate OpenShift resources from annotations
- Artemis JMS ([guide](https://quarkiverse.github.io/quarkiverse-docs/quarkus-artemis/dev/index.html)): Use JMS APIs to connect to ActiveMQ Artemis via its native protocol
- https://quarkus.io/guides/jms 