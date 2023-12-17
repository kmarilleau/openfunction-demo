# Deploying and Testing OpenFunction "Hello World" on Minikube

This README provides instructions for deploying a simple "Hello World" function written in Python on Minikube using OpenFunction, and testing it using kubectl port-forward. It also includes steps for configuring OpenFunction to use an insecure private Docker registry.

## Prerequisites

**For a detailed guide on installing these prerequisites, visit [OpenFunction Demo Prerequisites](https://github.com/kmarilleau/openfunction-demo/blob/main/README.md).**

- Minikube installed and running.
- kubectl installed and configured.
- OpenFunction installed on your Minikube cluster.
- Docker installed on your local machine (if building images locally).

## Deploying the Function

1. **Apply the Function Configuration:**

   ```bash
   kubectl apply -f function.yaml
   ```

2. **Check the Function Deployment:**

   ```bash
   kubectl get functions
   ```

## Setting Up Port Forwarding

To test your function locally, set up port forwarding from Minikube to your local machine:

```bash
kubectl port-forward svc/hello-world-function 8080:8080
```

## Testing the Function

With port forwarding set up, you can now test your function by sending a request:

```bash
curl http://localhost:8080
```

You should receive a "Hello World" response from your function.

## Troubleshooting

If you encounter any issues, ensure Minikube is running and all services are in the correct state, the registry IP is correct, and you have the correct credentials for the registry.

## Conclusion

You have successfully deployed and tested a "Hello World" Python function on Minikube using OpenFunction and configured it to use an insecure private Docker registry.
