# kubernetes-meetup-13062019

This project is intended to demonstrate Kubernetest Hello World deployment with Azure Kubernetes Service. The final result of this excersize is:

- Kubernetest Cluster with one Workier Node in Azure;
- Application Gateway with Public IP address for HTTP calls;
- Hello World NodeJS express API application;
- `kubectl` and `az` commands to deploy API to Kubernetes;
- Access API from outside world;

> NOTE! This demo is not including SSL protection of the API, Kubernetes Logs collection in Azure Log Service.
>
> In order to do SSL we would need DNS name and SSL certificate for it. In order to collect logs for analysis we need Azure Log Service linked to Azure (I will show production cluster example for both).

# Creating express application

In order to create express application I used the simpliest way:

Initialize Node JS application with 

```
npm ini
```
Create `"srart"` sctipt and set `"node index.js"` as executing command. Create `"index.js"` file in the project root. See `"index.js"` context in the file and notice that it has only one root path that returns `"Hello World!" text.

If you cloned the repository, run it with the commands:

```
npm install
npm start
```

# Dockerizing API

In order to run API in Kubernetes we need to create a docker container image. See `Dockerfile` in the root of the project. Also check `.gockerignore` file.

## Dockerfile

Docker file is a simple as is, take a look at it, it only builds `npm` packages and copies everything that was built into working directory.

Also it runs `"npm run start"` command on start.

Notice, it exposes ports `3000 80 443` as this port API is running at. In the Kubernetes deployment file we set running port to `80` in the environment variable.

> `index.js` file contains the `PORT` environment variable check, if given it uses it.

Reporisoty was created in public dockerhub and pushed under the name

```
sergeysedelnikovstoraenso/kubernetes-demo-api:1.0.0
```

## Kubectl deployment

See `deployment.yaml` file for deployment details.