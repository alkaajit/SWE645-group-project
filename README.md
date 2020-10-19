# SWE645-group-project

Group Members : 

Ambily Sureshbabu Rema, G01174300, http://swe645-ambily.s3-website-us-east-1.amazonaws.com

Alka Ajit, G01220250, http://swe645-aajit.s3-website-us-east-1.amazonaws.com/

Krishnapriya Mattathil Letha, G01181598, http://krishnaawslearning.s3-website-us-east-1.amazonaws.com/

Pranathi Krishna Sangani, G01213280, http://pksanganibucket.s3-website-us-east-1.amazonaws.com

======================================================================================

Our application can be accessed at :
http://ec2-18-206-14-103.compute-1.amazonaws.com:31748/SWE645_HW_2/

======================================================================================

Detailed documentation can be found at: https://bit.ly/347UPGF

1. Setting up Git repository :
  1) Create a new repository in Github
  2) Push the code that you have created in eclipse(or any other IDE) into the new repository.
         
  Our code has been added to Github and you can access it in the following link: https://github.com/AmbilySureshbabu/SWE645-group-project.git

2. Creating a Docker Image and setting up Docker Hub: 
  1) Install docker on your machine. We installed it on Ubuntu and followed the steps given here https://docs.docker.com/engine/install/ubuntu/
  2) Create an account in dockerhub https://hub.docker.com/ and create your repository
     Repository Url: https://hub.docker.com/r/swe645group/swe645_hw2
  3) Create a Docker file with a tomcat base image and bundle the war file.
  4) Use the following commands in the command line to create an image and run the container.. We have named the tag the image as myapp:
      docker build -t myapp .
      docker run -it --rm -p 8888:8080 myapp 
  5) You must be able to find the image at http://localhost:8888.
  
3. Setting up the rancher cluster:

  We will be launching two EC2 instances. 

 On the first EC2 instance we will have Rancher UI/Server installed along with Jenkins. On the other EC2 instance we will run the rancher-agent. These two EC2 instances will then be added as a Rancher Cluster.

  1) Login into AWS educate account
  2) Select EC2 from AWS services
  3) Launch EC2 Instance-1. Install docker, Rancher UI/Server, Jenkins on this instance
  4) Launch EC2 Instance-2. Install docker and create a rancher cluster using the two EC2 instances

4. Setting up CI/CD Pipeline
    Login to Jenkins using http://ec2-3-237-11-24.compute-1.amazonaws.com:8080/
      a. Add additional plugins- Docker pipeline, Kubernetes, Rancher
      b. Click Manage Jenkins -> Global Tool Configuration
      c. Create a LocalAnt ( This will be referenced in JenkinsFile)
      d. Click Manage Jenkins -> Manage Nodes and Clouds -> Configure Cloud ->Add a new cloud ->Kubernetes
          Name: Rancher
          Click Kubernetes Cloud details
          Here we need to use the Kubernetes URL and Credentials from Rancher UI, follow the below steps to get the details
            1) Login to Rancher UI https://ec2-3-237-11-24.compute-1.amazonaws.com/
            2) Select the cluster swe645-test ->Kubeconfig File
            3) In Kubeconfig File, configuration for the server will show the Kubernetes URL
            4) For Credentials: Select API & Keys 
                Click Add Key
                Description- ec2-jenkins
                Select A month , scope as the cluster you have created and Click Create
                Access Key will be the username and Secret Key will be the password that needs to be used in Jenkins.
                Click Test Connection , It should give Connected to Kubernetes and then Save.
        e. Next we will setup all credentials for Jenkins
            1) Click Manage Jenkins ->Credentials
            2) Add credentials for GitHub
            3) Add credentials for DockerHub ( use id as swe645group as that is what is referenced in JenkinsFile)


5. Setting up the CI/CD pipeline:
   a. In Jenkins, click on new items.
      1) Select Pipeline
      2) Choose pipeline script from scm
          Choose SCM as Git (as we have our project pushed into Git repository)
          Provide the repository URL as your github url
          The script path will be the JenkinsFile

      3) Run the Pipeline by clicking on "Build now:
         This will checkout code, run the Ant Build, create the Docker Image and deploy the docker image to the Rancher Cluster using the swe-645-1.yaml
      4) Once the application is deployed you will be able to access the application using the EC2-Instance2 http://Public DNS:nodePort/ContextRoot
         In our case it is : http://ec2-18-206-14-103.compute-1.amazonaws.com:31748/SWE645_HW_2/

