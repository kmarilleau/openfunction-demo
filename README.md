# Installing OpenFunction on Minikube with Helm

This document provides instructions on how to install OpenFunction on Minikube using Helm and configure it to use Docker Hub with a personal access token for the registry secret.

## Prerequisites

- Minikube installed and running.
- Helm installed on your local machine.
- Docker Hub account with a generated personal access token.

## Installing OpenFunction

1. Start Minikube if it's not already running:

   ```bash
   minikube start
   ```

2. Add the OpenFunction Helm repository:

   ```bash
   helm repo add openfunction https://openfunction.github.io/charts/
   helm repo update
   ```

3. Install OpenFunction using Helm:

   ```bash
   helm install openfunction openfunction/openfunction --create-namespace
   ````

## Configuring Docker Registry Secret with Personal Access Token

1. Create a personal access token (PAT) from your Docker Hub account. Refer to Docker Hub documentation on how to create a PAT.

2. Create a Kubernetes secret in the same namespace where OpenFunction is installed, replacing `<docker-username>` with your Docker Hub username and `<pat>` with the personal access token:

   **Bash:**
   ```bash
   kubectl create secret docker-registry dockerhub-credentials \
     --docker-server=https://index.docker.io/v2/ \
     --docker-username=<docker-username> \
     --docker-password=<pat> \
     --namespace=openfunction
   ```

   **PowerShell:**
   ```powershell
   kubectl create secret docker-registry dockerhub-credentials `
     --docker-server=https://index.docker.io/v2/ `
     --docker-username=<docker-username> `
     --docker-password=<pat> `
     --namespace=openfunction
   ```

## Using the Docker Registry Secret in OpenFunction

1. When creating your OpenFunction function definition, reference the Docker Hub registry and the created secret:

   ```yaml
   apiVersion: openfunction.io/v1alpha2
   kind: Function
   metadata:
     name: sample-function
     namespace: openfunction
   spec:
     image: "<docker-username>/sample-function:latest"
     build:
       builder: "openfunction/builder-go:v1"
       params:
         registry: "index.docker.io"
         registryCredential: "dockerhub-credentials"
   ```

2. Apply the function configuration to deploy it:

   ```bash
   kubectl apply -f your-function.yaml
   ```

## Setting Up a Kafka Cluster

To demonstrate OpenFunction with event-driven functions, you can create a Kafka cluster in the default namespace.

1. **Create Kafka Cluster and Topic:**
   Run the following command to create a Kafka server named `kafka-server-demo` and a Kafka topic named `kafka-topic-demo`:

   ```bash
   kubectl apply -f https://raw.githubusercontent.com/kmarilleau/openfunction-demo/main/setup/kafka.yaml
   ```

2. **Check Kafka and Zookeeper Pod Status:**
   Wait for Kafka and Zookeeper to run and start:

   ```bash
   kubectl get po
   ```

3. **View Kafka Cluster Metadata:**
   Use `kafkacat` to view the metadata for the Kafka cluster:

   ```bash
   kafkacat -L -b kafka-server-demo-kafka-brokers:9092
   ```

## Verifying the Deployment

1. Check the status of the function to ensure it's been deployed and the image is pushed to Docker Hub:

   ```bash
   kubectl get functions -n openfunction
   ```

2. Once the function is deployed, you can invoke it based on the event source or trigger you have defined.

## Conclusion

You have now set up OpenFunction on Minikube using Helm and configured it to use a Docker registry secret with a personal access token. Your OpenFunction functions can now be built and pushed to Docker Hub securely.
