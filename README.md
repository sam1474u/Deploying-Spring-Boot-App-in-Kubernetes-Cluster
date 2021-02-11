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

a) Check your service limits:
  Regions: minimum 2
   In the top navigation bar, expand <region>. Example: US East (Ashburn) and US West (Phoenix).

  Compute instances: minimum 3
   Click your profile avatar. Select Tenancy. Go to Service Limits and expand Compute.

  Block Volume: minimum 50 GBs
   In the Service Limits section, expand Block Volume.

  Load Balancer: available
   In the Service Limits section, expand Networking.
 
 b) Have a compartment for your cluster
 c) Create an authorization token
 d) Collect the following information and copy them into your notepad -
 

    Auth Token: <auth-token> from step 3.
    Region: <region-identifier> from step 4. Example: us-ashburn-1.
    Region Key: <region-key> from step 4. Example: iad.
    Tenancy name: <tenancy-name> from your user avatar.
    Tenancy OCID: <tenancy-ocid> from your user avatar, go to Tenancy:<your-tenancy> and copy OCID.
    Username: <user-name> from your user avatar.
    User OCID: <user-ocid> from your user avatar, go to User Settings and copy OCID.
    Region Key : us-ashburn-1 and iad

2. Generate SSH key using Open SSH or PuttyGen. Save public and private key and keep for future use
3. Create VCN
4. Install your Oracle Linux VM
5. Configure your Oracle Linux VM
  a) Install Git
     wget https://repo.ius.io/7/x86_64/packages/g/git222-core-2.22.4-1.el7.ius.x86_64.rpm  
    ![image](https://user-images.githubusercontent.com/42166489/107626717-37a56080-6c84-11eb-92f2-24f5f4ab6426.png)
     
   b) Install Java and Maven
   
     - Please note, while executing mvn package, you may face deployment failure. This is because of permission issue.run this command :  sudo chmod –R 777 YourfolderName
     ![image](https://user-images.githubusercontent.com/42166489/107626779-4ab83080-6c84-11eb-9a4b-9f3821bec28c.png)

6. Build Your Spring Boot Application
   a) Check out the Spring Boot Docker guide with Git:
      git clone http://github.com/spring-guides/gs-spring-boot-docker.git
   b)  Change into the gs-spring-boot-docker/initial directory.
   c)  Edit the Application.java file: src/main/java/hello/Application.java.
   d)  Update the code with the following:   
                        
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
                    
   e) Save the file.
   f) Use Maven to build the application.
      mvn package
   g) Run the application.
   h) Test your application from the command line or a browser.

      From a new terminal, connect to your VM with your SSH keys and test with curl:
          curl -X GET http://localhost:8080
          
          ![image](https://user-images.githubusercontent.com/42166489/107626868-6faca380-6c84-11eb-9ea1-9b755df6abfa.png)

               
7. Build and Push your Spring Boot Application

   a) Install Docker
   
   b) Build your Docker Image
   
   c) Push your Docker image to OCIR
   
   d) View the OCIR Repository you Created
   
   ![image](https://user-images.githubusercontent.com/42166489/107628296-720ffd00-6c86-11eb-8576-3c524ed1ba06.png)
   
   
   ![image](https://user-images.githubusercontent.com/42166489/107628311-79370b00-6c86-11eb-8b47-618996c9077e.png)


8. Create your Kubernetes Cluster

  ![image](https://user-images.githubusercontent.com/42166489/107628661-fbbfca80-6c86-11eb-9f8e-f31f4fc19fdb.png)
  
9. Manage your Kubernetes Cluster with Cloud Shell

  ![image](https://user-images.githubusercontent.com/42166489/107629176-b8b22700-6c87-11eb-819c-dba50e9c6a1a.png)
  
  
10. Deploy your Docker Image to your Cluster with Cloud Shell

  a) Create a Secret to Securely Connect to OCIR
  
  b) Create a Manifest file and Deploy your Image
  
  c) Test your App
  
  d) Clean up your App
  
  ![image](https://user-images.githubusercontent.com/42166489/107629219-c9fb3380-6c87-11eb-8303-446f656ed13e.png)
  
  ![image](https://user-images.githubusercontent.com/42166489/107629261-db444000-6c87-11eb-8f46-bc7406b01bee.png)
  
  
  
  Hence,You have successfully installed and deployed a Spring Boot app to a Kubernetes cluster on Oracle Cloud Infrastructure.
  
  
  
  
  

