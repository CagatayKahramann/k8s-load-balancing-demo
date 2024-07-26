# k8s-load-balancing-demo

## Overview

This project contains Kubernetes manifests for deploying and managing Nginx instances. It includes configurations for Deployments, Services, Ingresses, and ConfigMaps.

## Description

Demonstrates Kubernetes load balancing with Nginx Ingress. This project shows how traffic is distributed across multiple pods with the same label, using Kubernetes manifests for Nginx deployment, service, and Ingress  to achieve balanced traffic distribution and smart routing. Once you set up the project correctly, refreshing the URL will result in traffic being balanced between `nginx-test1` and `nginx-test2` pods, with the service redirecting you to each pod alternately.

## Requirements

To get started with this project, you'll need the following:

1. **Kubernetes Cluster**: A running Kubernetes cluster where you can deploy the manifests. You can set up a local cluster using tools like [Minikube](https://minikube.sigs.k8s.io/docs/) or [Kind](https://kind.sigs.k8s.io/docs/user/quick-start/), or use a managed Kubernetes service like GKE, EKS, or AKS. This project uses minikube as an example.

2. **Kubectl**: The command-line tool for interacting with Kubernetes. You can install it by following the instructions [here](https://kubernetes.io/docs/tasks/tools/install-kubectl/).

3. **Ingress Controller**: An Ingress Controller is required to manage and route external traffic into your Kubernetes cluster. Popular options include [Nginx Ingress Controller](https://docs.nginx.com/nginx-ingress-controller/) or [Traefik](https://doc.traefik.io/traefik/providers/kubernetes-ingress/). You can find installation instructions in their respective documentation. This project uses an Nginx Ingress Controller as default.

4. **DNS Configuration**: For local testing with `k8s-load-balancing-demo.local` as the URL, you need to configure your `/etc/hosts` file or use a local DNS service to map `k8s-load-balancing-demo.local` to the IP address of your Ingress controller.

## Using Minikube

If you're using Minikube for local development, you can enable the Ingress addon to simplify setup:

1. **Start Minikube**:
   ```bash
   minikube start
   ```

2. **Enable Ingress Addon**:
   To deploy the Nginx Ingress Controller within your Minikube cluster, run:
   ```bash
   minikube addons enable ingress
   ```
4. **Get the Minikube IP**:
   To get the Minikube IP address, which will be used in the `/etc/hosts` file, run:
   ```bash
   minikube ip
   ```

## Directory Structure

- **deployments/**: Kubernetes Deployment manifests.
- **services/**: Kubernetes Service manifests.
- **ingresses/**: Kubernetes Ingress manifests.
- **configmaps/**: Kubernetes ConfigMap manifests.

## Setup

**Important Note**: During the setup process, the order in which you apply the manifests is important. First, apply the ConfigMap to ensure that the configurations are available. Next, deploy the Nginx instances, then expose the service, and finally configure the Ingress. This ensures that all necessary resources are created and properly configured in the correct sequence. Apply each manifest one by one to ensure proper setup.
    
1. **Clone the Repository**:
   ```bash
   git clone https://github.com/your-username/k8s-load-balancing-demo.git
   cd k8s-load-balancing-demo
   ```

2. **Start Minikube and Enable Ingress (Optional)**:
   If you decided to use Minikube, make sure it is started and the Ingress addon is enabled. If you have not done this already, run:
   ```bash
   minikube start
   minikube addons enable ingress
   ```

3. **Apply the ConfigMap**:
   Apply the ConfigMap first to ensure all configurations are available before deploying other resources.
   ```bash
   kubectl apply -f configmaps/
   ```

4. **Deploy Nginx Instances**:
   Deploy the Nginx instances to ensure the application pods are up and running. Make sure to apply the manifest files one-by-one, otherwise the necessary configurations might not be properly initialized, leading to potential deployment issues.
   ```bash
   kubectl apply -f deployments/nginx-test1-deployment.yaml
   kubectl apply -f deployments/nginx-test2-deployment.yaml
   ```

5. **Expose Nginx Service**:
   Expose the Nginx service to make the application accessible within the cluster.
   ```bash
   kubectl apply -f services/nginx-test-service.yaml
   ```

6. **Configure Ingress**:
   Configure the Ingress to manage external access to the services.
   ```bash
   kubectl apply -f ingresses/nginx-test-ingress.yaml
   ```
   The Ingress configuration provided uses the hostname `k8s-load-balancing-demo.local`. If you need to use a different hostname, you can edit the Ingress manifest file that is inside the `ingresses/` directory and modify the host field in the `/etc/hosts` to specify a different hostname according to your needs.

7. **Get the Minikube IP**:

   To get the Minikube IP address, which will be used in the `/etc/hosts` file, run:
   ```bash
   minikube ip
   ```

8. **Update Your /etc/hosts File**:

   After retrieving the Minikube IP address and the service URL, update your `/etc/hosts` file to map your service’s hostname to the Minikube IP. Open `/etc/hosts` with a text editor and add a line like this:
   ```bash
   <minikube-ip>   k8s-load-balancing-demo.local
   ```
   Replace `<minikube-ip>` with the IP address obtained from the minikube ip command. If you edited the default Ingress manifest, change the URL address to the one specified in your Ingress configuration to ensure it    matches the hostname used in your `/etc/hosts` file.


9. **Access the Service Locally**:
   Now you can access your service using the hostname you specified in the ingress manifest file and `/etc/hosts` file, for the default configuration the URL is `http://k8s-load-balancing-demo.local`. Once set up correctly, refreshing the address will load balance the traffic and redirect you to `test1` and `test2` alternately.

## Troubleshooting

- Ensure you have the necessary permissions and context set up in your Kubernetes cluster.
- Verify that the Ingress controller is running and properly configured.
- Double-check your `/etc/hosts` file for correct IP mapping.
- Use kubectl describe and kubectl logs commands to debug any issues with the deployed resources.

## License
This project is licensed under the MIT License. See the [LICENSE](LICENSE) file for details.

