Kubernetes Using Cloud Shell: Deploy a Spring Boot Application
-----------------------------------------------------------------
In this tutorial, you use an Oracle Cloud Infrastructure account to set up a Kubernetes cluster. Then, you deploy a Spring Boot application to your cluster. Key tasks include how to:

- Set up an authentication token.
- Set up a Kubernetes cluster on OCI.
- Build a Spring Boot application and Docker image.
- Push your image to OCIR.
- Deploy your Docker application to your cluster using Cloud Shell.
- Connect to your application from the internet.

Reference URL for documentation : https://docs.oracle.com/en-us/iaas/developer-tutorials/tutorials/spring-on-k8s-cs/01oci-spring-cs-k8s-summary.htm#gather-info



![image](https://user-images.githubusercontent.com/42166489/107624419-d760ef80-6c80-11eb-8c00-c0de932e239b.png)


1. Gather Required Information
  Prepare the information you need from the Oracle Cloud Infrastructure Console.

    1. Check your service limits:
        Regions: minimum 2
          In the top navigation bar, expand <region>. Example: US East (Ashburn) and US West (Phoenix).

        Compute instances: minimum 3
          Click your profile avatar. Select Tenancy. Go to Service Limits and expand Compute.

        Block Volume: minimum 50 GBs
          In the Service Limits section, expand Block Volume.

        Load Balancer: available
          In the Service Limits section, expand Networking.
 
    2. Have a compartment for your cluster
    3. Create an authorization token - From user name select Auth Token and generate Token.
    4. Find your region identifier and region key. Example: us-ashburn-1 and iad for Ashburn.
    5. Collect the following information and copy them into your notepad -
 
        Auth Token: <auth-token> from step 3.
        Region: <region-identifier> from step 4. Example: us-ashburn-1.
        Region Key: <region-key> from step 4. Example: iad.
        Tenancy name: <tenancy-name> from your user avatar.
        Tenancy OCID: <tenancy-ocid> from your user avatar, go to Tenancy:<your-tenancy> and copy OCID.
        Username: <user-name> from your user avatar.
        User OCID: <user-ocid> from your user avatar, go to User Settings and copy OCID.
        Region Key : us-ashburn-1 and iad

2. Generate SSH key using Open SSH or PuttyGen. Save public and private key and keep for future use. 
    Refer for more details : https://www.oracle.com/webfolder/technetwork/tutorials  /obe/cloud/compute-iaas/generating_ssh_key/generate_ssh_key.html
    
3. Create VCN 
   1. In the Start VCN Wizard workflow, select VCN with Internet Connectivity and then click Start VCN Wizard.
   2. In the configuration dialog, fill in the VCN Name for your VCN. 
      Your Compartment is already set to the last compartment you were working in, or if it's your first time, to its default value of <your-tenancy> (root).
   3. In the Configure VCN and Subnets section and Add ingress rules
4. Install your Oracle Linux VM
5. Configure your Oracle Linux VM
  
   1. Connect to your VM with this ssh command
             ssh -i <your-private-key-file> opc@<x.x.x.x> 
  
   2. Enable HTTP connection via port 8080.
   
            sudo firewall-cmd --permanent --add-port=8080/tc 
            sudo firewall-cmd --reload

   3. Install Git
   
      Install Git v2 using the IUS Community Project (https://ius.io/) Spring Boot Docker tutorial. 
      
          wget https://repo.ius.io/7/x86_64/packages/g/git222-core-2.22.4-1.el7.ius.x86_64.rpm  
          
          sudo yum install git224-core-2.24.2-1.el7.ius.x86_64.rpm
          
          git --version               
                    
     
   ![image](https://user-images.githubusercontent.com/42166489/107626717-37a56080-6c84-11eb-92f2-24f5f4ab6426.png)
     
   4. Install Java 
   
      Install OpenJDK 8 using yum.
      
            sudo yum install java-1.8.0-openjdk-devel
            java -version
      Set JAVA_HOME in .bashrc
      
            vi ~/.bashrc
            # set JAVA_HOME
            export JAVA_HOME=/etc/alternatives/java_sdk
            source ~/.bashrc
    
   5. Install Maven 
   
      Install Maven from an Apache mirror. Go to the main Maven site's (https://maven.apache.org/) download page. Get the the URL for the latest version and download with wget.
      
             wget http://apache.mirrors.pair.com/maven/maven-3/3.6.3/binaries/apache-maven-3.6.3-bin.tar.gz   
             
             sudo tar xvfz apache-maven-3.6.3-bin.tar.gz        
                         
             sudo mv apache-maven-3.6.3 /opt/
             
       Add the Maven path /opt/apache-maven-3.6.3/bin to your PATH environment variable and source your .bashrc
             
              vi ~/.bashrc
              export PATH=$PATH:/opt/apache-maven-3.6.3/bin
              source ~/.bashrc
  
       Please note : while executing mvn package, you may face deployment failure. This is because of permission issue.run this command :  sudo chmod –R 777 YourfolderName
     
  ![image](https://user-images.githubusercontent.com/42166489/107626779-4ab83080-6c84-11eb-9a4b-9f3821bec28c.png)

6. Build Your Spring Boot Application
    - Check out the Spring Boot Docker guide with Git:
      git clone http://github.com/spring-guides/gs-spring-boot-docker.git
    - Change into the gs-spring-boot-docker/initial directory.
    - Edit the Application.java file: src/main/java/hello/Application.java.
    - Update the code with the following:   
                        
                              package hello;

                              import org.springframework.boot.SpringApplication;
                              import org.springframework.boot.autoconfigure.SpringBootApplication;
                              import org.springframework.web.bind.annotation.RequestMapping;
                              import org.springframework.web.bind.annotation.RestController;

                                  @SpringBootApplication
                                  @RestController
                                  public class Application {

                                      @RequestMapping
                                      public String home(){
                                          return "<h1>Spring Boot Hello World!</h1>";
                                      }

                                      public static void main(String[] args) {
                                          SpringApplication.run(Application.class, args);
                                      }
                              }
                    
   - Save the file.
   - Use Maven to build the application.
      mvn package
   - Run the application.
   - Test your application from the command line or a browser.

      From a new terminal, connect to your VM with your SSH keys and test with curl:
          curl -X GET http://localhost:8080
          
          ![image](https://user-images.githubusercontent.com/42166489/107626868-6faca380-6c84-11eb-9ea1-9b755df6abfa.png)

               
7. Build and Push your Spring Boot Application

    1. Install Docker
   
      Install the Docker packages for Oracle Linux.
      
          sudo yum install docker-engine docker-cli                        
                    
      Start the docker service and configure it to start at boot time.
        
          sudo systemctl enable --now docker   
          
      Check if the Docker service is running.
      
          sudo docker info   
       
    2. Build your Docker Image
   
      First, make sure you are in the gs-spring-boot-docker/initial directory.
      Create a file named Dockerfile.
      
            FROM openjdk:8-jdk-alpine
            RUN addgroup -S spring && addduser -S spring -G spring
            USER spring:spring
            ARG JAR_FILE=target/*.jar
            COPY ${JAR_FILE} app.jar
            ENTRYPOINT ["java","-jar","/app.jar"]

      Build the Docker image: sudo docker build -t spring-boot-hello .
      
      Run your Docker image: sudo docker run -p 8080:8080 -t spring-boot-hello
      
      Test the application using the command line or a browser.     
      
          To test with curl, in a terminal run: curl -X GET http://localhost:8080
          Connect your browser to the public IP address assigned to your VM: http://<x.x.x.x>:8080.
          
    3. Push your Docker image to OCIR

       Open a terminal window.
       Log Docker into OCIR:
       
            sudo docker login <region-key>.ocir.io
            
       You are prompted for your login name and password.

            Username: <tenancy-name>/<user-name>  for example - bmdrgwy1wsjh/viveklal
            Password: <auth-token>
           
       List your local Docker images.
       
           sudo docker images
           
       Tag your local image so it can be pushed to OCIR:
       
           sudo docker tag <your-local-image> <target-tag>
           
       Check your Docker images to see if the tag has been created.
       
           sudo docker images            

   
   4. View the OCIR Repository you Created
   
   ![image](https://user-images.githubusercontent.com/42166489/107628296-720ffd00-6c86-11eb-8576-3c524ed1ba06.png)
   
   
   ![image](https://user-images.githubusercontent.com/42166489/107628311-79370b00-6c86-11eb-8b47-618996c9077e.png)


8. Create your Kubernetes Cluster

    Set up the Kubernetes cluster. You will use a wizard to set up your first cluster.

      From the OCI main menu select Developer Services then Container Clusters.
      Click Create Cluster.
      Select Quick Create.
      Click Launch Workflow and select VM.Standard.E2.1 shape


  ![image](https://user-images.githubusercontent.com/42166489/107628661-fbbfca80-6c86-11eb-9f8e-f31f4fc19fdb.png)
  
9. Manage your Kubernetes Cluster with Cloud Shell

   From the OCI main menu select Developer Services then Container Clusters.
   Click Access Cluster.
   Select Cloud Shell Access. 
   
   Open your Cloud Shell environment.

   Make your .kube directory if it doesn't exist.
   
        mkdir -p $HOME/.kube

   Create kubeconfig file for your setup. Use the information from Access Your Cluster dialog.
   
        oci ce cluster create-kubeconfig <copy-data-from-dialog>
 
  Test your cluster configuration with the following commands. List clusters:
  
      kubectl get service


  ![image](https://user-images.githubusercontent.com/42166489/107629176-b8b22700-6c87-11eb-819c-dba50e9c6a1a.png)
  
  
10. Deploy your Docker Image to your Cluster with Cloud Shell

    1. Create a Secret to Securely Connect to OCIR
    
       First, create a registry secret for your application. This will be used to authenticate your image when it is deployed to your cluster.
       
       
                kubectl create secret docker-registry ocirsecret --docker-server=<region-key>.ocir.io
                --docker-username='<tenancy-name>/<user-name>' --docker-password='<auth-token>'  --docker-email='<email-address>'                                
                
       Verify the secret was created. Issue the following command.
       
                 kubectl get secret ocirsecret --output=yaml
  
    2. Create a Manifest file and Deploy your Image
    
       With your image in OCIR, you can now deploy your image and app.

       From the home directory, create a directory called spring-hello.
       
                 mkdir spring-hello
                 cd spring-hello
                 
       Create a file called sb-app.yaml.
                 
                 vi sb-app.yaml
       
       
                apiVersion: apps/v1
                kind: Deployment
                metadata:
                  name: sbapp
                spec:
                  selector:
                    matchLabels:
                      app: sbapp
                  replicas: 3
                  template:
                    metadata:
                    labels:
                      app: sbapp
                    spec:
                      containers:
                      - name: sbapp
                        image: <your-image-url>
                        imagePullPolicy: Always
                        ports:
                        - name: sbapp
                        containerPort: 8080
                        protocol: TCP
                      imagePullSecrets:
                      - name: <your-secret-name>
                ---
                apiVersion: v1
                kind: Service
                  metadata:
                  name: sbapp-lb
                    labels:
                      app: sbapp
                spec:
                  type: LoadBalancer
                  ports:
                  - port: 8080
                    selector:
                      app: sbapp

       

   Deploy your application with the following command.
   
         kubectl create -f sb-app.yaml
    
  
  Test your App 
  
  Check for your load balancer to deploy.
  
        kubectl get service
        
   ![image](https://user-images.githubusercontent.com/42166489/109114916-4e5bb500-7764-11eb-910c-5bc9790ad0b5.png)
   
   Test the service on browser
   
   ![image](https://user-images.githubusercontent.com/42166489/109115064-84009e00-7764-11eb-8bed-4d9dbc462308.png)

  
  Clean up your App
  
  After you are done, cleanup and remove the services you created.
  Delete your app and load balancer.
  
      kubectl delete -f sbapp.yaml
      
  To check the service is removed
  
      kubectl get service

  
  ![image](https://user-images.githubusercontent.com/42166489/107629261-db444000-6c87-11eb-8f46-bc7406b01bee.png)  
  
  
  Hence,You have successfully installed and deployed a Spring Boot app to a Kubernetes cluster on Oracle Cloud Infrastructure.
  
