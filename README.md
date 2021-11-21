# CI-CD-WeightTracker-k8S ![image](https://user-images.githubusercontent.com/89352211/142737603-a00b2530-e159-4d80-9636-b23cc0cb1ec1.png)
## Requirements:
* Install Docker on each of the virtual machines by these commands: </br>
sudo apt-get update && apt-get upgrade -y </br>
curl -fsSL https://get.docker.com/ -o get-docker.sh </br>
bash get-docker.sh </br>
sudo apt-get update && apt-get upgrade -y </br>
* Node.js 12.x or higher
* Configure an agent: [agent-pool](https://www.youtube.com/watch?v=psa8xfJ0-zI&ab_channel=Raaviblog) 
* PostgreSQL (can be installed locally using Docker)
* Free Okta developer account for account registration, login


## Steps:
* Create 2 resource groups for 2 environments- staging and production which contains: 2 AKS - Kubernetes Cluster in Microsoft’s Azure Kubernetes Service for the project infrastructure..
* Create a virtual machine and install on it docker and agent by azure devops.
* Create a container registy for storing files.
* Install the Nginx ingress controller by with the commands: 
choco install kubernetes-helm
helm repo add bitnami https://charts.bitnami.com/bitnami
helm install my-release bitnami/nginx-ingress-controller
* Deploy the NodeWeightTracker application on AKS meeting the following requirements:
- The NodeWeightTracker application must be accessible from the internet
- The NodeWeightTracker application must be exposed to the internet on port 80
- The NodeWeightTracker must have at least 3 instances to ensure high availability
- Use configmaps/secrets to store your application configurations
- You must expose your application using the ingress controller
* CI \ CD process using "Azure DevOps Organizations"
* The CI process contained: " Build and push an image to container registry "
* The CD process contained for each enviroment ( Staging and Production )
* Create imagePullSecret With secretArguments:

'--from-literal=COOKIE_ENCRYPT_PWD=$(COOKIE_ENCRYPT_PWD) --from-literal=HOST=$(HOST) --from-literal=PORT=$(PORT) --from-literal=NODE_ENV=$(NODE_ENV) --from-literal=HOST_URL=$(HOST_URL) --from-literal=OKTA_CLIENT_ID=$(OKTA_CLIENT_ID) --from-literal=OKTA_CLIENT_SECRET=$(OKTA_CLIENT_SECRET) --from-literal=OKTA_ORG_URL=$(OKTA_ORG_URL)  --from-literal=PGHOST=$(PGHOST) --from-literal=PGUSERNAME=$(PGUSERNAME) --from-literal=PGDATABASE=$(PGDATABASE) --from-literal=PGPASSWORD=$(PGPASSWORD)  --from-literal=PGPORT=$(PGPORT)'

* Deploy to Kubernetes cluster:
                $(Pipeline.Workspace)/manifests/deployment.yml
                $(Pipeline.Workspace)/manifests/service.yml
                $(Pipeline.Workspace)/manifests/ingress.yml
                
* Kubectl Apply to deployment.yml
* Run the pipeline and check that your applications are running :)

The full process:
![image](https://user-images.githubusercontent.com/71599740/142738639-068572f2-c29b-4b4f-92d3-e6c316064c8d.png)

![image](https://user-images.githubusercontent.com/47865329/142753458-0083b2b4-7bb7-4ebc-8df1-1173d5a2311b.png)
