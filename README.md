Project 1: Implementations.
 Jenkins CI/CD pipeline with GitHub webhook integration for Deploying Docker application on EC2 instances using the declarative pipeline.
Follow the below steps please:
1.     First of all, go to AWS portal, and create a new instance. As,
·       Name: jenkins-server
·       AMI: ubuntu.
·       Instance type: t2.micro (free tier).
·       Key pair login: Create > docker.pem.
·       Allow HTTP.
·       Allow HTTPS.
(Download the .pem file.)
Click on Launch Instance.
No alt text provided for this image

2.     Now, connect to the EC2 instance that you have created. Copy the SSH from server:
No alt text provided for this image
 
3.     Go to the download folder, where the .pem file is placed and open the terminal in the same location, and paste the SSH.
 
4.     In the machine, run the command
“ssh-keygen”
This will generate public and private keys in the machine.
Id_rsa – Private Key.
Id_rsa.pub – Public Key.
No alt text provided for this image

5.     Now we will install Jenkins on the machine, by following this link  
https://www.jenkins.io/doc/book/installing/linux/
This will automatically install java with Jenkins.
 
6.     Install Docker as well to the machine by running,
“Sudo apt-install docker.io”
 
7.     Now check if it got installed by running “jenkins --version” and “docker --version”
No alt text provided for this image

8.     Now, we will allow ports 8080 and 8001 for this machine from a security group. We can find the security group in the VM description. Now, here we need to allow “Inbound Rule” as below:
No alt text provided for this image
 
9.     Now, Copy the Public Ip of the machine and paste it to the browser to access the Jenkins portal. As,
“54.193.49.139:8080”
No alt text provided for this image
 
10.  We need an Administrator Password to unlock this. For that, go to the provided highlighted path in the upper screenshot.
“cat /var/lib/Jenkins/secrets/initialAdminPassword”
No alt text provided for this image
Paste this password in the “Administrator Password” Column.

11.  Now Click on, “Install Suggested Plugins”
No alt text provided for this image
 
12.  This will now install the suggested plugins. As
No alt text provided for this image
  
13.  Now, Jenkins will ask us to create the First Admin User.
No alt text provided for this image
 
14.  Add the fields accordingly.
No alt text provided for this image
 
15.  The Jenkins homepage will look like this,
No alt text provided for this image

16.  Now, we will create a CI/CD pipeline, which will fetch the code from GitHub. 

17.  From Jenkins Dashboard, Click on “New Item”.
No alt text provided for this image
 
18.  Now, Add name as
Name: todo-app
Project: Freestyle project
Click “Ok”.
No alt text provided for this image

19.  Here, we need to fill up the description.
No alt text provided for this image

20.  In Source Code Management, select Git and Add Repository URL and Credentials.
(If there is not any added credential, we need to add)
No alt text provided for this image

21.  In Build Step, select Execute Shell and write following command to build docker image and from Docker image we will create a container.
No alt text provided for this image
 
22.  Now, Click on build Now. And the build will be started, in the build history.
No alt text provided for this image
 
23.  In Output Console,
No alt text provided for this image
 
24.  After getting success, In the browser, search for
<public_ip_of_ec2:8001>
No alt text provided for this image
Now, our goal is,
·       Whenever the developer commits their code in GitHub, after every commit, it should reflect in the live web app.
·       For that, we will use “GitScm polling”.
·       Every time, a developer made a commit, a trigger will run automatically, which will rebuild the image and run a container on your behalf as a part of automation that will run the pipeline automatically.

25.  Now, configure the project again, and add
Build Trigger: GitHub hook trigger for GitScm polling.
Description: GitHub webhook integration
No alt text provided for this image

26.  We need to install the “Git Integration” plugin from Manage Jenkins, by following the path,
(Manage Jenkins > Manage Plugins > Git Integration).
No alt text provided for this image
 
27.  Now, Goto GitHub > Settings > SSH and GPG Keys > New SSH Key.
Add details as
Title: chetanrakhra@gmail.com
Key type: <public_key>
No alt text provided for this image
 
28.  To get the Public key, open the “id_rsa.pub” file and copy the content. (Public Key)
No alt text provided for this image

29.  Now, we need to go to GitHub and create a new SSH and GPG Key.
GitHub > Repo “react-django-demo-app” > Settings > Webhooks.
30.  Add the following details,
Payload URL: http://<public_ip_of_ec2>:8080/github-webhook/
Content Type: application/json
Which event would you like to trigger this webhook?
o  Just the push event.
Active: True
Click on “Add Webhook”...
No alt text provided for this image
 
31.  Now, Save the configured project.
32.  Do some changes in the code and push to GitHub, this will automatically run a pipeline, and the new code will be Live.
