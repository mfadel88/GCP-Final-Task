
# GCP project Final Task
This project to deploy a python application on "GKE" kubernetes cluster from the image I have built from the application dockerfile which i have been created on Google cloud provider using terraform.

# project Chart
![Final-project-iti (1)](https://user-images.githubusercontent.com/93100689/202595149-e63cef27-c3bd-4f5f-bf88-a948fef5d157.png)

# Tools
- ### Terraform
   - Infrastructure as a code to execute the app Infrastructure on the GCP or any cloud provider. 
- ### GCP
   - The Google cloud provider which we will use to deploy our project.
- ### K8s
   - Is an open-source system for automating deployment and management of our containerized applications.
- ### GitHub
   - This is the source of our code that al the developers push their code on it.
- ### GCR Artifact Registry
   -The service or recource which we use to push and pull the app and redis images through it.

## Terraform script structure section by section
### 1- cloudProvider
   - To define the cloud provider which we will run the script on it and you should to run the below command to sync with cloud platform.
    -  #terraform init  
![1](https://user-images.githubusercontent.com/93100689/202312687-98152351-c2e7-49ce-8a05-ab5a2dbb936b.png)

### 2- Main-VPC
   - That contain all of our Infrastructure.
   ![2](https://user-images.githubusercontent.com/93100689/202312999-da74cf17-9184-40b2-83d1-ad0052230f53.png)

### 3- Subnets
   - #### private-mangment-subnet.
     contain the cluster and worker nodes.
   - #### restricted-subnet.
     Contain the private-VM that we will use to connecy on the cluster.

 ![3](https://user-images.githubusercontent.com/93100689/202313522-a3c4a311-e522-45da-affc-e64e6add738f.png)

### 4- Service account
   - This service account will be with Role "container.admin" which we will give it to our VM and gke cluster  to Access our GKE Cluster.
![30](https://user-images.githubusercontent.com/93100689/202586151-d653f862-10b2-4570-aa30-3272d095433f.png)

### 5- Router
   - For nat
![5](https://user-images.githubusercontent.com/93100689/202314300-71e89b45-8912-4a66-ad4e-0b5965a9642c.png)

### 6- NAT
   - To allow the cluster and private-vM reach out to internet from it's subnets.
![6](https://user-images.githubusercontent.com/93100689/202314584-95a1e338-8b14-4613-8554-44b519bd51bc.png)
### 7- Private-VM
   - This is a Bastion VM in order to connect on the cluster and execute whatever we need through it.
![7](https://user-images.githubusercontent.com/93100689/202314877-91eae36f-424b-4d36-b85b-3a0b139de4ef.png)

### 8- Firewall
   - This Firewall to allow ssh connection from bastion VM to the cluster.
![8](https://user-images.githubusercontent.com/93100689/202315086-ced581e1-ae27-4be1-bddf-4ae715123706.png)

### 9- private-k8s-cluster
![9](https://user-images.githubusercontent.com/93100689/202315410-9e38db34-0a04-4a3e-86f3-2d257e837bfa.png)

![10](https://user-images.githubusercontent.com/93100689/202315417-57552855-f6c9-411c-b45c-860e2f8c739f.png)

### 10- Node pool
![31](https://user-images.githubusercontent.com/93100689/202586401-b15f62e1-1735-4020-9830-cc9a75263ba3.png)
![12](https://user-images.githubusercontent.com/93100689/202315836-b4f39ec3-8305-4e59-aa7d-75a648c39f76.png)

### Test and Execute
   - To test the file code syntax run the below command
    -  #terraform plan
   - To execute the file code run the below command
    -  #terraform apply
   then you have to get that apply complete successfully as below
   ![13](https://user-images.githubusercontent.com/93100689/202318830-03ef0161-8f3e-4013-920f-401d82d7bf37.png)
   
#### Note: Then we will find that all our infra has been deployed on GCP.

## Then we will access and configur cluster from the private-vm to install jenkins component
   - SSH connect on private-vm then we will install kubectl by the following command
    - sudo apt install kubectl
   - then we will authenticate my account to be able to access the project recources
    - gcloud auth login
then it will give you URL in order to navigate then you will copy the pass code and past it as shown below
![14](https://user-images.githubusercontent.com/93100689/202320979-1569a6ea-7dcb-4c03-93ec-796c5b0e1275.png)

   - now We need to access the K8s cluster so We will go to the GCP console then navigate to private-k8s-cluster> click connect
   ![15](https://user-images.githubusercontent.com/93100689/202322113-63a91e99-4327-4c1d-98d1-f13ef1a4bd9f.png)

   - Take the command that will appear to you and copy and run it on private-vm
    - gcloud container clusters get-credentials private-k8s-cluster --zone us-central1-a --project final-proj-fadel
![16](https://user-images.githubusercontent.com/93100689/202322125-75d43661-d413-44d7-bed8-95b6babd4215.png)

   - Note: if you faced an issue warning running this command, try to run it first.
    - sudo apt-get install google-cloud-sdk-gke-gcloud-auth-plugin
![18](https://user-images.githubusercontent.com/93100689/202324056-a9ff8d7e-1ba9-4f85-a381-1776ff08b45e.png)

### Note: now we are able to connect and run kubectl from private vm successfully.

______________________________


## We will navigate to the next stage “python app deployment”
   - First, we will pull Redis image from docker hub and give it the gcr tage in order to be able to push it on GCR then we will push it on GCR artifact registry by the below commands.
    - 	docker pull redis
    -	docker image list
    -	docker tag 3358aea34e8c gcr.io/final-proj-fadel/redis
    -	docker push gcr.io/final-proj-fadel/redis
![32](https://user-images.githubusercontent.com/93100689/202590693-b3eb42ff-6a06-412a-b0ed-1ec7f137490c.png)

   - Then we will create Redis Deployment and service file.
![33](https://user-images.githubusercontent.com/93100689/202590824-ab36daae-74ca-4f37-83a5-7ff683c3e02c.png)

-	We will create the python app docker file as per the requirements which provided through the python app directory after that we will build an app image from this dockerfile
![35](https://user-images.githubusercontent.com/93100689/202590875-89ec21d4-27ba-4ed4-a58c-190eeca639f3.png)

   - After that we will git clone the project on the private-VM bastion to build the dockerfile which we have been created by the below commands.
    - mkdir pythonApp
    - cd pythonApp
    - git clone https://github.com/mfadel88/GCP-Final-Task.git
    
    
![36](https://user-images.githubusercontent.com/93100689/202590876-4931bd24-4141-4a34-a8ec-72cd2c3c2002.png)

   - Then we will build the docker file with gcr tag and push it on artifact registry by the below commands.
    - docker build . -t gcr.io/final-proj-fadel/python-app
    - docker image list
    - docker push gcr.io/final-proj-fadel/python-app
![37](https://user-images.githubusercontent.com/93100689/202590879-061b958a-d0b8-427c-b829-790d1f055e45.png)
![38](https://user-images.githubusercontent.com/93100689/202590880-db80e1f4-57f2-4f7d-8972-15deb489087f.png)

- Then we will create the python app deployment and service file to deploy it 
![39](https://user-images.githubusercontent.com/93100689/202590882-0b50694a-625e-4c90-ad64-419d839a05fa.png)

   - We will create the namespace to deploy our resources in it
    - kubectl create ns final-task
![40](https://user-images.githubusercontent.com/93100689/202590884-48cc52ae-23d6-4da0-ad3d-4fffd6ff43d3.png)

-Then we will deploy the python and redis deployment file.
![41](https://user-images.githubusercontent.com/93100689/202590887-117f5af4-f693-4ff3-b931-cbf8f562eb31.png)

![42](https://user-images.githubusercontent.com/93100689/202590888-9d3b3069-30dd-41d9-8c24-b5910f2036e3.png)

![43](https://user-images.githubusercontent.com/93100689/202590890-0688af89-fea6-49e1-bcf2-c07e488e74fd.png)

### Now we will check the service ip for python app to browse the url app.
![44](https://user-images.githubusercontent.com/93100689/202592404-478f2728-df64-4a79-bb91-1039c5fb3d0b.png)
- we will take the python ip and port then hit it 
![45](https://user-images.githubusercontent.com/93100689/202592460-34cde901-45d6-4a5b-bd59-e7cee2cb367d.png)

## Now everything is running ok as expected

## To destroy all of the Infrastructure
      - terraform destroy
