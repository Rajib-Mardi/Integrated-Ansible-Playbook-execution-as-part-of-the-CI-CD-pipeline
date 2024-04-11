 ### Project: 
   * Ansible Integration in Jenkins 
 ### Technologiesused: 
   * Ansible, Jenkins, DigitalOcean, AWS, Boto3, Docker, Java, Maven, Linux, Git

### Project Description:

### Create and configure a dedicated server for Jenkins 

   * create a DigitalOcean droplet ssh into the server ,  Install docker and run jenkins as docker container.

### Create and configure a dedicated server for Ansible Control Node
  * create a DigitalOcean droplet
  * ssh into the server
    * Configure Droplet as Ansible Control Node(Install Ansible).
       *  ``` apt install ansible``` command
    * Install pip3
       * ```apt install python3-pip``` command 
    * Install boto3 and botocore
      
       * ``` apt install python3-boto3```
       * ```apt install python3-botocore```
    *  Configure the aws credentials in the ansible-server.
     * create  .aws credentials  in the home directory as root user.
     *  ```mkdir .aws```
     *   Create the credentials file and use a text editor vim to edit it. Copy and paste the AWS credentials you have locally in your home directory:
    
    
### Create two  EC2 Instances (for Ansible Managed Nodes) manually and download the pem file.

* Manually launch the two Amazon Linux instances (managed servers) in AWS EC2 Dashboad and create the new key pairs for ansible-jenkins since ansible-server connects to the instances required to configure the ssh key

* written   the Ansible Playbook that installs  the  Docker and Docker Compose in the Ec2 server with ansible.cfg file , and inventory file



### Add ssh key file credentials in Jenkins for Ansible Control Node server(droplet server) and Ansible Managed Node servers(ec2 instance)

* need the ssh key to connect to the Ansible server (droplet) from Jenkins; 


*   In Jenkins use the ssh username with a private key (ansible server control node) before adding the private key, convert the ssh private key command to "ssh-keygen -p -f..sh/id_rsa -m pem -p "" -N""and ""  " then add these private keys in Jenkins. 

* need the ssh key to connect to the Ansible manged server  (ec2 instance) from Jenkins; 
*   Private key for EC2 instance: For ansible playbook execution, the.ssh-key pem file must be present on the ansible server, so we must first make this pem file available within Jenkins so that Jenkins can copy it to the ansible server. In Jenkins server , create the credentials  with ssh username with private key 


  * Install the ```ssh agent``` Plugin and  ```ssh pipeline steps plugin``` in jenkins.
      
### Configure Jenkins to execute the Ansible Playbook on remote Ansible Control Node server as part of the CI/CD pipeline

* ```Pipeline Agent```: The pipeline is set to execute on any available agent.

* ```Environment```: It defines an environment variable ```ANSIBLE_SERVER``` with the value "142.93.4.168," which represents the IP address of the Ansible control node.

* ```Stages```:

1. Copy Files to Ansible Server:

   * This stage is responsible for copying necessary files to the Ansible control node.
   * It uses the ```scp``` command to transfer files from the Jenkins agent to the Ansible control node, including Ansible playbooks and an SSH key.
   * The SSH key is obtained from Jenkins credentials (credentialsId: 'ec2-server-key').
     
2. Execute Ansible Playbook:

   * This stage triggers the execution of an Ansible playbook on the Ansible control node.
   * It defines a ```remote``` object that specifies the remote server (```ANSIBLE_SERVER```) to connect to and execute commands.
   * The SSH key for authentication is obtained from Jenkins credentials (credentialsId: 'ansible-server-key').
   * It executes the ```prepare-ansible-server.sh``` script and the ```ansible-playbook my-playbook.yaml``` command on the Ansible control node.
  
 ### execute the pipeline

   * execute the playbook remotely on that Control Node that will configure the 2 EC2 Managed Nodes.

<img src="https://github.com/Rajib-Mardi/Complete-CI-CD-Pipeline-with-EKS-and-AWS-ECR/assets/96679708/465fa75f-33d2-461a-9339-4d85426ecf65" width="700">


<img src="https://github.com/Rajib-Mardi/Complete-CI-CD-Pipeline-with-EKS-and-AWS-ECR/assets/96679708/df3d07e3-d9af-477d-848a-c60569a10e23" width="700">
