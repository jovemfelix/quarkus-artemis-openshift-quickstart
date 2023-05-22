# quarkus-artemis

This project uses Quarkus, the Supersonic Subatomic Java Framework.

If you want to learn more about Quarkus, please visit its website: https://quarkus.io/ .

## Running the application in dev mode

You can run your application in dev mode that enables live coding using:
```shell script
./mvnw compile quarkus:dev
```

> **_NOTE:_**  Quarkus now ships with a Dev UI, which is available in dev mode only at http://localhost:8080/q/dev/.

## Packaging and running the application

The application can be packaged using:
```shell script
./mvnw package
```
It produces the `quarkus-run.jar` file in the `target/quarkus-app/` directory.
Be aware that it’s not an _über-jar_ as the dependencies are copied into the `target/quarkus-app/lib/` directory.

The application is now runnable using `java -jar target/quarkus-app/quarkus-run.jar`.

If you want to build an _über-jar_, execute the following command:
```shell script
./mvnw package -Dquarkus.package.type=uber-jar
```

The application, packaged as an _uber-jar_, is now runnable using `java -jar target/*-runner.jar`.

## Creating a native executable

You can create a native executable using: 
```shell script
./mvnw package -Pnative
```

Or, if you don't have GraalVM installed, you can run the native executable build in a container using: 
```shell script
./mvnw package -Pnative -Dquarkus.native.container-build=true
```

You can then execute your native executable with: `./target/quarkus-artemis-1.0.0-SNAPSHOT-runner`

If you want to learn more about building native executables, please consult https://quarkus.io/guides/maven-tooling.


## Create the App Config/Secret at the Cluster

```shell script
# create a project if you don't have one
$ oc new-project myquarkus

$ oc create secret generic artemis-secret --from-literal="QUARKUS_ARTEMIS_PASSWORD=${BROKER_CREDENTIAL_PASSWORD}"

$ oc create configmap artemis-conf --from-literal=QUARKUS_ARTEMIS_URL="(tcp://${BROKER_SVC_ACCEPTOR_TCP}:61616)" --from-literal="QUARKUS_ARTEMIS_USERNAME=${BROKER_CREDENTIAL_USERNAME}"
```

### Build and Deployment

> [Guide: DEPLOYING ON OPENSHIFT](https://quarkus.io/guides/deploying-to-openshift)

```shell script
./mvnw install -Dquarkus.kubernetes.deploy=true
```

### Or Just Deploy with an existing image
```shell script
$ oc create -f k8s/deploymentConfig.yaml
```

## Related Guides

- OpenShift ([guide](https://quarkus.io/guides/deploying-to-openshift)): Generate OpenShift resources from annotations
- Artemis JMS ([guide](https://quarkiverse.github.io/quarkiverse-docs/quarkus-artemis/dev/index.html)): Use JMS APIs to connect to ActiveMQ Artemis via its native protocol
- https://quarkus.io/guides/jms 