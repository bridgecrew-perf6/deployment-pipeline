# Tekton CI/CD

## Installing Node.js
`https://nodejs.org`

### Verify
`node -e "console.log('Hello world')"`

## Installing VS Code and extensions
 `https://code.visualstudio.com/`

### Extensions
Kubernetes by Microsoft
YAML by Red Hat
Tekton Pipelines by Red Hat

## Installing Docker
`https://docker.com/get-started`

### Verify
`docker version`

## Installing minikube
`https://minikube.sigs.k8s.io/docs/start/`

## Starting minikube
`minikube start`

## Enabling the ingress add-on
`minikube addons enable ingress`

## Installing kubectl
`https://kubernetes.io/docs/tasks/tools/install-kubectl/`

### Verify
`kubectl version` or `kubectl get all`

## Tekton tooling
### Installing Tekton CLI
`https://tekton.dev/docs/getting-started/#set-up-the-cli`

### Installing Tekton on the Kubernetes cluster
`kubectl apply --filename https://storage.googleapis.com/tekton-releases/pipeline/latest/release.yaml`

### Installing the necessary CRDs for Tekton Triggers on the Kubernetes cluster
`kubectl apply -f https://storage.googleapis.com/tekton-releases/triggers/latest/release.yaml`

`kubectl apply -f https://storage.googleapis.com/tekton-releases/triggers/latest/interceptors.yaml`

### Verify
`tkn version`

### Installing Tekton Dashboard
`kubectl apply --filename https://github.com/tektoncd/dashboard/releases/latest/download/tekton-dashboard-release.yaml`

`kubectl --namespace tekton-pipelines port-forward svc/tekton-dashboard 9097:9097`

Go to `http://localhost:9097`

## Creating a service account
`kubectl apply -f https://raw.githubusercontent.com/tektoncd/triggers/main/examples/rbac.yaml`

## Creating a cluster role binding
`kubectl create clusterrolebinding serviceaccounts-cluster-admin --clusterrole=cluster-admin --group=system:serviceaccounts`


## Clone the source code
`git clone https://github.com/genekuo/tekton-book-app.git`

## Test the source code
`cd tekton-book-app`

`npm install`

`npm start`

`curl localhost:3000`

`curl localhost:3000/add/12/10`

`curl localhost:3000/substract/10/2`

`Ctrl+C`

`npm run lint`

`npm run test`

## Creating the container
`docker build -t <YOUR_USERNAME>/tekton-lab-app .`

`docker login docker.io`

`docker push <YOUR_USERNAME>/tekton-lab-app`

## Deploying the application
`kubectl apply -f deploy.yaml`

### Verify the deployment
`kubectl get deploy`

`curl $(minikube ip)`

## Updating the application
Change the response to the "/" route in server.js to return a different response.

`git commit -am "Change a server response to one"`

`git push origin main`

`npm run test`

`npm run lint`

`docker build –t <YOUR_USERNAME>/tekton-lab-app .`

`docker push <YOUR_USERNAME>/tekton-lab-app`

## Rollout the deployment
`kubectl rollout restart deployment/tekton-deployment`

### Verify the updates
`curl $(minikube ip)`

## Installing tools from the task catalog
`https://hub.tekton.dev`

### git-clone
`kubectl apply -f https://raw.githubusercontent.com/tektoncd/catalog/main/task/git-clone/0.5/git-clone.yaml`

### npm
`kubectl apply -f https://raw.githubusercontent.com/tektoncd/catalog/main/task/npm/0.1/npm.yaml`

### kubernetes-actions
`kubectl apply -f https://raw.githubusercontent.com/tektoncd/catalog/main/task/kubernetes-actions/0.2/kubernetes-actions.yaml`

## Creating build-push task
`cd deployment-pipeline`

`kubectl apply -f task.yaml

## Creating the pipeline
`kubectl apply -f pipeline.yaml`

## Creating the event-binding, commit-tt and listener
### Creating a secret key, which will be shared between your trigger and GitHub
`export TEKTON_SECRET=$(head -c 24 /dev/random | base64)`

`kubectl create secret generic git-secret --from-literal=secretToken=$TEKTON_SECRET`

`echo $TEKTON_SECRET`

Edit <YOUR_USERNAME>  and <YOUR_PASSWORD> in the `trigger.yaml`
`kubectl apply -f trigger.yaml`

## Creating a public route to your local Kubernetes cluster
### Installing Ngrok
`https://ngrok.com/download`

### Creating a public route
`kubectl port-forward svc/el-listener 8080`

`ngrok http 8080`

## Adding a new webhook to your repository (tekton-lab-app) in the GitHub

Payload URL: This is your public ngrok URL
Content Type: This should be changed to application/json
Secret: This is the secret you've stored in the $TEKTON_SECRET environment variables

## Verifying CI/CD pipeline
### Make a change to source code in tekton-lab-app
Change the response to the "/" route in server.js to return a different response.
`npm run test`

`npm run lint`

`git commit -am "Change a server response to two"`

`git push origin main`

`tkn pipelineruns ls`

`curl $(minikube ip)`

## Clean up
`minikube stop`

`minikube delete`
