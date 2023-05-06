# kubernetes-gitops-lab

Lab Steps:

Set up a Git repository: Create a new Git repository or use an existing one to store your Kubernetes manifests and application configurations.

Install Flux: Flux is a popular GitOps tool that synchronizes your Kubernetes cluster with your Git repository. Install Flux on your cluster using the following command:

```
helm repo add fluxcd https://charts.fluxcd.io
helm upgrade -i flux fluxcd/flux --namespace fluxcd \
--set git.url=<your-git-repo-url> \
--set git.path=kubernetes \
--set git.pollInterval=1m \
--set syncGarbageCollection.enabled=true \
--set manifestGeneration=true \
--set registry.pollInterval=1m \
--set sync.state=secret
```

This command installs Flux in the fluxcd namespace and configures it to sync your Git repository's kubernetes-gitops-lab directory with your Kubernetes cluster. It also enables garbage collection, which ensures that any resources that are no longer defined in your Git repository are deleted from your cluster.

Configure Flux: To allow Flux to access your Git repository, you need to configure a deploy key or a personal access token (PAT). In this lab, we'll use a deploy key. Generate a deploy key using the following command:

```
ssh-keygen -t rsa -b 4096 -C "flux" -N "" -f ./flux
```
This command generates a new SSH key pair named flux in the current directory. The -C option sets the comment field of the public key to "flux".

Add the public key to your Git repository as a deploy key.

### trigger or Create a Kubernetes deployment
edit then manifest files  in the kubernetes directory of your Git repository.

Deploy the application: Commit and push the changes to your Git repository to trigger a deployment to your Kubernetes cluster. You should see the web-interior deployment being created 

Update the application: Make a change to the nginx deployment by increasing the number of replicas from 1 to 2 in the replicas field of the manifest. Commit and push the changes to your Git repository to trigger a deployment update. You should see the new nginx pod being created and the old pod being terminated.

This is a practical hands-on lab  to use gitops with kubernetesas the single source of truth. 
