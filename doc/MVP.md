# MVP (Minimum Viable Product)

## Introduce
The Minimum Viable Product (MVP) of the AsciiArtify project is geared towards launching a fundamental application via ArgoCD. This application will monitor changes from the GitHub repository go-demo-app and automatically synchronize them onto a Kubernetes cluster.

## Steps to Set Up MVP

### 1. Establishing a Kubernetes Cluster with k3d
- Utilize k3d to create a local Kubernetes cluster.
- Confirm the cluster's operational status.
```bash
  k3d cluster create demo
  kubectl cluster-info
```
### 2. Installing ArgoCD
- Follow the official ArgoCD documentation to install ArgoCD on the Kubernetes cluster.
- Ensure successful deployment of ArgoCD pods.
```bash
  kubectl create namespace argocd
  kubectl apply -n argocd -f https://raw.githubusercontent.com/argoproj/argo-cd/stable/manifests/install.yaml
```

### 3. Configure Application in ArgoCD
- Download Argo CD CLI
- Add the go-demo-app repository to ArgoCD for continuous delivery.
- Configure automatic synchronization of changes from the repository to the Kubernetes cluster.
More detailed installation instructions can be found via the [CLI installation documentation.](https://argo-cd.readthedocs.io/en/stable/cli_installation/ "CLI installation documentation.").
```bash
  kubectl config set-context --current --namespace=argocd
  argocd login --core
  argocd app create demo --repo https://github.com/den-vasyliev/go-demo-app.git --path helm  --dest-server https://kubernetes.default.svc --dest-namespace demo
```
[![asciicast](https://asciinema.org/a/3tbpjUdcQSUqFas3jS0ljjZPT.svg)](https://asciinema.org/a/3tbpjUdcQSUqFas3jS0ljjZPT)

### 4. Demonstrate Application Deployment
- Deploy the go-demo-app using ArgoCD onto the Kubernetes cluster.
- Monitor the synchronization process and ensure successful deployment of the application.
```bash
  argocd app get demo
  argocd app sync demo
```
[![asciicast](https://asciinema.org/a/5my7cvBh5goJHyhbdZTqMD24x.svg)](https://asciinema.org/a/5my7cvBh5goJHyhbdZTqMD24x)

## Demo Instructions

### Accessing ArgoCD UI
- Use port-forwarding to access the ArgoCD UI:
```bash
  argocd admin initial-password -n argocd
  kubectl port-forward svc/argocd-server -n argocd 8080:443
```
### Accessing ArgoCD UI
- Open a web browser and navigate to [https://localhost:8080](https://localhost:8080) to access the ArgoCD UI.

### Monitoring Application Deployment
- Navigate to the ArgoCD UI and observe the deployment status of the go-demo-app.
- Verify that changes from the GitHub repository are automatically synchronized and deployed onto the Kubernetes cluster.

[![Accessing ArgoCD UI](https://img.youtube.com/vi/qLXZUJJAivY/default.jpg "Accessing ArgoCD UI")](https://youtu.be/qLXZUJJAivY "Accessing ArgoCD UI")

### Demo autosync
- Enable autosync demo for AgroCD and control changes in Git repo.

[![Demo agrocd autosync](https://img.youtube.com/vi/Q37PMmgKtj4/default.jpg "Demo agrocd autosync")](https://youtu.be/Q37PMmgKtj4  "Demo agrocd autosync")

### Test Application
```bash
  kubectl port-forward svc/ambassador -n demo2 8088:80
  curl localhost:8088
  wget -O /tmp/google.png https://www.google.com/images/branding/googlelogo/1x/googlelogo_light_color_272x92dp.png
  curl -F 'image=@/tmp/google.png' localhost:8088/img/
```
[![Test Application](https://img.youtube.com/vi/uHruOaoXOIc/default.jpg "Test Application")](https://youtu.be/uHruOaoXOIc "Test Application")

## Conclusion
The MVP demonstrates the automated deployment of the go-demo-app using ArgoCD on a Kubernetes cluster provisioned with k3d. This setup enables the AsciiArtify team to efficiently manage application deployments and iterate on features based on user feedback.
