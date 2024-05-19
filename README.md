# Robusta Task



## Getting started

- This repo has the deployment files for deploying a simple to-do application that consists of 3 components (frontend, backend and mongo databse)

- to deploy this application push a new commit to the repo or use run pipeline from build > pipeline section on gitlab and a new pipleine will be triggered, create namespace on kubernetes cluster named "robusta" and after making sure that the full stack is deployed on the cluster use this commmand : kubcetl port-forward svc/web -n robusta 3000:3000 to make sure that the application runs locally on port 3000 locally on your machine ,, after that open a new browser and go for this URL: http://localhost:3000/ 

- the pipeline consits of three stages : first one for building and pushing the frontend imag to DockerHub and the second one for building and pushing the backend image and the final stage is the deployment one where the helm charts for frontend and backend application gets deployed on the kubernetes cluster and also the deployment file for the mongo database gets deployd as well

- In this pipeline, a new self-hosted runner is deployed on local minikube cluster using Helm chart with the right permsission to deploy respurces in "robusta" namepace

- Assumptions: I assumed that deployment will happen at local minikube cluster so I went with the approach to install gitlab hosted runner on my cluster to be able to deploy helm charts otherwise the job fo deployment will not be able to reach my private cluster

- In case the pipeline is being tested on another cluster , a kubeconfig varibale should be defined and injected inside the deploy stage so that the deploy job will be able to deploy the application to target clsuter specified in the KubeConfig provided

- A DockerHub username and password are defined for that project as masked variables 

- The testing process for this application included testing the functionality of the app locally first to make sure that the frontend and backend service are up and running since the application is not developed by me 

- Afterwards, creating Dockerfiles to build the app images and push them to personal DockerHub Repository and after that creating the helm charts and deploy them to my local cluster

- probelms encountered was that the github self-hosted runner was not picking the pipeline deployment stage and was solved by updating the runner deployment configuration with specific tag and using that tag inside the stage definition 

- Another problem faced was that the image used for deployment stage  with helm already installed was giving an error becasue the entrypoint for these images was the helm command with no options passed to it and  was solved by updating the image feinition inide that stage with empty entrypoint :  entrypoint: [""]

- Another problem encountered was that seld-hosted runner didnot have required permission to patch and apply ertain resources and was solved by modifying the Cluster-role attached to its sservice acccount and adding the needed actions on certain resources like deployments and services



