
# Ptereodactyl
Pterodactyl® is a free, open-source game server management panel built with PHP, React, and Go. Designed with security in mind, Pterodactyl runs all game servers in isolated Docker containers while exposing a beautiful and intuitive UI to end users.

For the installation of this application, you require:
- **SQL database**
- **Redis instance**
- **Secrets Operator**


# Documentation
- [Pterodactyl Documentation](https://pterodactyl.io/project/introduction.html)
- []()



# Pipelines in Github actions
To support the automation of the docker container creation, it has been implemented Github Actions on this repo that allows to test, build and push the image to GCR.

Processs goes as explained next:
- Developer creates code in a new branch
- Push of the code and merge to develop branch, triggers actions, lint and ci Pipelines.
- The code is approved, and is time to create a tag to mark this point of the functional code.
- Tag Created and pushed to repo, triggers a release pipeline and a docker pipeline.
- **Release pipeline** creates a release branch on the repo and publish a new release of the code.
- **Docker Pipeline** builds a new docker image and pushes it to a Google container registry.

## CI/CD Diagram

![CI/CD Diagram](https://github.com/EdEngineering/Images/blob/main/Final%20Project%20Diagrams-Pterodactyl%20CICD.drawio.png?raw=true)


# Pterodactyl in Kubernetes

In this repo, in Kubernetes folder are the Kubernetes manifests to deploy the application in a Kubernetes environment. 
We have to folders, panel and redis; Panel is for the application itself, redis is a database that the app needs, but in my case i did not use the reids deployment of this folder.

- **Panel folder**, contains the Kubernetes manifests to deploy the application.
  - **Deployment manifest**, deployment-panel.yaml in this file its defined the containers for the appllication to work.
    - ***Pterodactyl*** container, uses a docker image built by this application pipeline and uploaded to GCR.
    - ***cloud-sql-proxy*** sidecar container, uses a docker image provided by google to create a proxy on localhost to connect to the external database that the application uses.
  - **Claim manifest**, claim persistent volumes to save data of the application.
  - **Service Manifest**, to expose the application on the cluster.
  - **Ingress Manifest**, Creates a nginx ingress object to allow users to connect throug a domain name to the panel app.

## Application Diagram

The next diagram shows how the application works, the deployment creates the pod with two containers as mentioned before, Pterodactyl container connects to the Database through the proxy sql client created in the sidecar container, The Redis database is created in another namespace and its accesible via the default ports exposed to the cluster via a service object.
Then the Pterodactyl app its exposed via service and allows connections and visibility from outside through a nginx ingress object, connecting to the load balancer.

![Application Diagram](https://github.com/EdEngineering/Images/blob/main/Final%20Project%20Diagrams-Pterodactyl%20App.drawio.png?raw=true)



## Installation

For the Installation guide you can follow the instruction in the [repository link here](https://github.com/applaudo-gcp-tp/evasquez_finalproject_argocd).

## Author

- [@EdEngineering](https://github.com/EdEngineering)

