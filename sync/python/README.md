# Deploying and Testing OpenFunction "Hello World" on Minikube

This README provides instructions for deploying a simple "Hello World" function written in Python on Minikube using OpenFunction, and testing it using kubectl port-forward. It also includes steps for configuring OpenFunction to use an insecure private Docker registry.

## Prerequisites

- Minikube installed and running.
- kubectl installed and configured.
- OpenFunction installed on your Minikube cluster.
- Docker installed on your local machine (if building images locally).

## Step 1: Starting Minikube

Ensure Minikube is running:

```bash
minikube start
```

## Step 2: Configuring Insecure Registry Access

OpenFunction requires the IP address of the registry when using an insecure private image repository. Obtain the IP address of the Minikube registry:

```bash
minikube addons enable registry
REGISTRY_IP=$(kubectl get svc registry -n kube-system -o jsonpath='{.spec.clusterIP}')
```

Next, configure your OpenFunction to use the Minikube registry. When defining the image to be used by OpenFunction, specify the IP address:

```yaml
apiVersion: openfunction.io/v1beta1
kind: Function
metadata:
  name: hello-world-function
spec:
  image: "$REGISTRY_IP:5000/hello-world-function:latest"
  build:
    builder: "openfunction/builder-go:v1"
    params:
      registry: "$REGISTRY_IP:5000"
      registryCredential: "push-secret"
```

Note: Replace `$REGISTRY_IP` with the actual IP address of your registry.

## Step 3: Creating a Secret for the Registry

If your registry requires authentication, create a secret with your Docker credentials:

```bash
kubectl create secret docker-registry push-secret \
  --docker-server=http://$REGISTRY_IP:5000 \
  --docker-username=<your_registry_user> \
  --docker-password=<your_registry_password>
```

Replace `<your_registry_user>` and `<your_registry_password>` with your actual registry username and password.

## Step 4: Deploying the Function

Apply the Function Configuration:

```bash
kubectl apply -f function.yaml
```

Check the Function Deployment:

```bash
kubectl get functions
```

## Step 5: Setting Up Port Forwarding

To test your function locally, set up port forwarding from Minikube to your local machine:

```bash
kubectl port-forward svc/hello-world-function 8080:8080
```

## Step 6: Testing the Function

With port forwarding set up, you can now test your function by sending a request:

```bash
curl http://localhost:8080
```

You should receive a "Hello World" response from your function.

## Troubleshooting

If you encounter any issues, ensure Minikube is running and all services are in the correct state, the registry IP is correct, and you have the correct credentials for the registry.

## Conclusion

You have successfully deployed and tested a "Hello World" Python function on Minikube using OpenFunction and configured it to use an insecure private Docker registry.
