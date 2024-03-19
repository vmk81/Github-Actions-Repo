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
2. Navigate to your AWS console and launch an EC2 instance with Linux AMI. Details on how to launch an EC2 instance can be found in the repository https://github.com/vmk81/Launch-an-EC2-Instance.   
   
3. Open the Security Groups tab and create a new Security Group by the name MyRunnerGroup. In the Inbound rules, add a new rule for HTTP port 80 and HTTPS port 443 like in the screenshot below. In the Outbound rules, add a new rule for HTTP port 80 and HTTPS port 443. These rules need to be setup for the Self hosted runner to communicate with GitHub Actions.
   ![8-Security Groups for the Runner](https://github.com/vmk81/Github-Actions-Repo/assets/157844406/3fba589d-ff79-4f66-b0bb-678cb3d1e9d8)
   
4. Login into your EC2 instance and run the command 'ssh-keygen -t rsa -b 4096' to generate a rsa key-pair. Open the key file and copy the key. This is to ensure that the runner instance can ssh into the Github repo.    
   ![2-Setup EC2 public key](https://github.com/vmk81/Github-Actions-Repo/assets/157844406/444bed62-606f-4c67-ba7f-88787ab05838)
   
5. Go back to your repository and go to 'Settings' tab(Red Arrow in the screenshot below). Scroll down to Security section and click on 'Deploy keys'(Green Arrow). Click on 'Add deploy key' and paste the rsa key you copied from the step 4. Click on 'Add key' button.   
   ![3-Deploying EC2 Keys](https://github.com/vmk81/Github-Actions-Repo/assets/157844406/c7f7eaca-7925-4c97-9647-396abb7bbbee)

6. Go back to your EC2 instanc ssh prompt and type the command 'ssh -T git@github.com' and you should see an output like from the screenshot below.
   ![4-Git Clone](https://github.com/vmk81/Github-Actions-Repo/assets/157844406/c0525810-07c1-4df5-a4ca-e48beb659c72)

   
### Section 3: Configure a Self-Hosted Runner  

7. Go back to Actions section and click on Runners(Red Arrow in the screenshot below). Choose the Linux option at the top and copy the commands one after the other and execute them on the EC2 instance   
   ![5-Runners Copy](https://github.com/vmk81/Github-Actions-Repo/assets/157844406/3430603b-c7ad-4275-90dd-a8fe281c0824)
   
9. Finally execute the run.sh script to launch the Self-Hosted runner. You should recieve 'Listening for Jobs' message like the screenshot below.
   ![6-Installing GitHub Runner](https://github.com/vmk81/Github-Actions-Repo/assets/157844406/173cb694-9632-433a-9f9d-90a7a7335ff3)

10. Go back to your repository and under the Runners tab you should be able to see your self hosted runner in idle state like the screenshot below . This means the runner is ready to execute jobs.
   ![7-Checking for Runners](https://github.com/vmk81/Github-Actions-Repo/assets/157844406/5bfe026c-d0fc-47bc-99a3-0a6e5a1895ff)


### Section 4: Preparing the GitHub Actions Workflow  
 
11. Go back to your repository main page and click on the 'Add file' button(Red Arrow in the screenshot below) . And then create a directory called .github and create another directory called workflows inside it (Green Arrow). All the pipleline yaml files will be stored in this directory called .github/workflows.
    ![10-Creating directory](https://github.com/vmk81/Github-Actions-Repo/assets/157844406/1d76876f-b1c6-41d1-ae40-f4f474185899)

12. Inside the .github/workflows directory, create a file called main.yml. This file will be the pipeline that will tell the runner what all to be executed. In this example yml, we will be executing a simple python script to add 2 numbers. It will run the commands specified inside the yml file.
     ![10-Creating the Pipeline yml file](https://github.com/vmk81/Github-Actions-Repo/assets/157844406/16a45b8e-8007-4549-b063-68e74e3d980d)

13. The trigger for this pipeline to execute is the push command (Red Arrow in the screenshot above). Any commit pushed into the main repository will be trigger. And self-hosted (Green Arrow) indicates that the jobs will run on a self-hosted runner.  

### Section 5: Test the Output  

14. Open any file inside the repository and commit a change. And then go back to your Runner EC2 instance and you should be able to see the 'Job build completed with result: Succeeded' message like the screenshot below.
   ![11-Test Result for Runner](https://github.com/vmk81/Github-Actions-Repo/assets/157844406/a9bfadc3-5827-419c-96bd-291f89030e54)
 
    



