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

2. Generate SSH key using Open SSH or PuttyGen. Save public and private key and keep for future use.


