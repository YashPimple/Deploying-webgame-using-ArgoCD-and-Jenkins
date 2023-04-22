## Deploying a Web-game using Kubernetes, Ingress, ArgoCD and Jenkins

<p align="center">
<img width="623" alt="Workflow" src="https://user-images.githubusercontent.com/97302447/230928505-ea91d995-824b-4fe0-aec6-45a4d54a6a79.png">
</p>

## Prerequisites

- Make sure you have a cluster ready on your terminal
- ArgoCD is installed on the cluster

ArgoCD can be installed using the following commands:

- To create a namespace "argocd", execute the following command. However, this step is optional and you can proceed with the "default" namespace as well:<br>
`kubectl create namespace argocd`

- Run the install ArgoCD script by executing the following command: <br>
`kubectl apply -n argocd -f https://raw.githubusercontent.com/argoproj/argo-cd/stable/manifests/install.yaml`

- Install the CLI using brew to use argocd commands: <br>
`brew install argocd`

- To retrieve the password, execute the following command: <br>
`kubectl -n argocd get secret argocd-initial-admin-secret -o jsonpath="{.data.password}" | base64 -d; echo`

- To access ArgoCD on a browser, forward the port to 8080 by executing the following command: <br>
`kubectl port-forward svc/argocd-server -n argocd 8080:443`

## Deployment through YAML configuration

- create `application.yaml` which will have our argo-cd configuration

```yaml
    apiVersion: argoproj.io/v1alpha1
    kind: Application
    metadata:
      name: webgame-argo-application
      namespace: argocd
    spec:
      project: default

      source:
        repoURL: https://github.com/YashPimple/Project-4
        targetRevision: HEAD
        path: Manifests
      destination: 
        server: https://kubernetes.default.svc
        namespace: web-game

      syncPolicy:
        syncOptions:
        - CreateNamespace=true

        automated:
          selfHeal: true
          prune: true
```          

- `apiVersion: argoproj.io/v1alpha1` and `kind: Application` indicate that this is an ArgoCD Application resource.
- `metadata` section defines the metadata for the ArgoCD Application, including the name and namespace.
- `spec` section describes the specification of the application.
- `project` field specifies the name of the ArgoCD project in which the application will be created.
- `source` field specifies the source code repository from which the application will be deployed, including the repository URL, target revision, and path of the deployment manifests.
- `destination` field specifies the destination Kubernetes cluster and namespace where the application will be deployed.
- `syncPolicy` field specifies how the application will be synchronized, including sync options such as creating a new namespace, and automated policies such as self-healing and pruning.

In summary, this ArgoCD configuration file defines an application named "webgame-argo-application" that will be deployed to the "web-game" namespace in a Kubernetes cluster from the specified GitHub repository using ArgoCD. The file also defines the synchronization policy for the application to ensure that it remains in a desired state.

 Apply the Kubernetes resource manifests `kubectl apply -f application.yaml`
 i am been using Port 8081 to accessed by argocd 
<p align="center">  

![argocd](https://user-images.githubusercontent.com/97302447/233764383-e1ce7620-bd32-47b7-8bdb-1f23841a2045.jpeg)

</p>

Also our web-game application is been Deployed on port 3031 🎉, alongside you can also do this through the UI 🚀
<p align="center">

![web-game](https://user-images.githubusercontent.com/97302447/233764531-848f7776-aca2-4ac1-b0df-404418b2f6b6.jpeg)

</p>


### 🚧 Deployed Napptive project link 

The [Shark-tank](sharktanks.tech/) web game is successfully deployed using the Open Application Model via the NAPPTIVE platform 
[Napptive-demo-link](https://nginx-ingress-cgnggjlt998c97mfuj2g.apps.playground.napptive.dev/)

### Project Demonstration Link : [Youtube](https://youtu.be/Fx_uvF4UT9g)

### 💳 License

This project is licensed under the [MIT License](LICENSE).
