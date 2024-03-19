# DevOps Project to build Pipelines using GitHub Actions and Self hosted runner on EC2 instance with a Linux AMI
## Summary  
This project is to demonstrate how we can build pipelines using GitHub Actions. An EC2 instance with Linux AMI will act a self hosted runner that will execute the changes whenever a commit is pushed into the main repository.  

This project will involve 5 sections.

Section 1: Set Up GitHub Repository   
Section 2: Launch an EC2 instance with Linux AMI with neccesary Secutiry Groups  
Section 3: Configure a Self-Hosted Runner  
Section 4: Preparing the GitHub Actions Workflow  
Section 5: Test the Output  

## Detailed Procedure
### Section 1: Set Up GitHub Repository

1. Login to your Github repository and click on create new repository. Provide a repository name and choose the Public option. Then click on 'Create repository' button.  
   ![1-GitHub Repository](https://github.com/vmk81/Github-Actions-Repo/assets/157844406/e598f47f-9cd2-4402-ab01-2dc80dbb3145)  
### Section 2: Launch an EC2 instance with Linux AMI with neccesary Secutiry Groups  
2. Navigate to your AWS console and launch an EC2 instance with Linux AMI. Details on how to launch an EC2 instance can be found in this repository.
   
3. Open the Security Groups tab and create a new Security Group by the name MyRunnerGroup. In the Inbound rules, add a new rule for HTTP port 80 and HTTPS port 443 like in the screenshot below. In the Outbound rules, add a new rule for HTTP port 80 and HTTPS port 443. These rules need to be setup for the Self hosted runner to communicate with GitHub Actions.
   ![8-Security Groups for the Runner](https://github.com/vmk81/Github-Actions-Repo/assets/157844406/3fba589d-ff79-4f66-b0bb-678cb3d1e9d8)
   
4. Login into your EC2 instance and run the command 'ssh-keygen -t rsa -b 4096' to generate a rsa key-pair. Open the key file and copy the key. This is to ensure that the runner instance can ssh into the Github repo.    
   ![2-Setup EC2 public key](https://github.com/vmk81/Github-Actions-Repo/assets/157844406/444bed62-606f-4c67-ba7f-88787ab05838)
   
5. Go back to your repository and go to 'Settings' tab(Red Arrow in the screenshot below). Open the 'Actions' section and click on Runners(Green Arrow). Click on 'Add deploy key' and paste the rsa key you copied from the step 4. Click on 'Add key' button.
   ![3-Deploying EC2 Keys](https://github.com/vmk81/Github-Actions-Repo/assets/157844406/8cfc5371-7071-4c28-996c-175316c365a9)
   
### Section 3: Configure a Self-Hosted Runner  



