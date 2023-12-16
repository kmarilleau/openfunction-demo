# Deploying and Testing OpenFunction "Hello World" on Minikube

This README provides instructions for deploying a simple "Hello World" function written in Python on Minikube using OpenFunction, and testing it using kubectl port-forward.

## Prerequisites

- Minikube installed and running
- kubectl installed and configured
- OpenFunction installed on your Minikube cluster

## Step 1: Starting Minikube

Ensure Minikube is running:

```bash
minikube start
```

## Step 2: Deploying the Function

1. **Apply the Function Configuration:**
   Deploy your function by applying the `function.yaml` file to your Minikube cluster.

   ```bash
   kubectl apply -f function.yaml
   ```

2. **Check the Function Deployment:**
   Verify that your function has been deployed successfully.

   ```bash
   kubectl get functions
   ```

## Step 3: Setting Up Port Forwarding

To test your function locally, you need to set up port forwarding from Minikube to your local machine.

1. **Find the Pod Name:**
   Get the name of the pod running your function.

   ```bash
   kubectl get pods
   ```

2. **Port Forwarding:**
   Forward a local port to the port your function is running on in the pod. Assume your function is running on port 8080.

   ```bash
   kubectl port-forward pod/<pod-name> 8080:8080
   ```

   Replace `<pod-name>` with the name of your pod.

## Step 4: Testing the Function

With port forwarding set up, you can now test your function:

1. **Send a Request to the Function:**
   Use a tool like `curl` to send a request to your function.

   ```bash
   curl http://localhost:8080
   ```

2. **Check the Response:**
   You should receive a "Hello World" response from your function.

## Troubleshooting

If you encounter any issues, check the following:

- Ensure Minikube is running and all services are in the correct state.
- Verify that the pod name used in the port forwarding command is correct.
- Check the logs of the function pod for any errors:

  ```bash
  kubectl logs <pod-name>
  ```

## Conclusion

You have successfully deployed and tested a "Hello World" Python function on Minikube using OpenFunction. This setup provides a basic example of how to work with serverless functions in a local Kubernetes environment.
