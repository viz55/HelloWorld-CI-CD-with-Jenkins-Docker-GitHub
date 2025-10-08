# HelloWorld CI/CD with Jenkins, Docker & GitHub

This project demonstrates running a simple Hello World Java program using Jenkins by pulling the code directly from GitHub. It also illustrates using Docker containers and multi-agent setups for CI/CD.

## AWS EC2 Instance

- Go to AWS Console ---> EC2 Instances
- Launch instances(Ubuntu)
- Instances(running)


### Install Jenkins.

Pre-Requisites:
 - Java (JDK)

### Run the below commands to install Java and Jenkins

Install Java: -

```
sudo apt update
sudo apt install openjdk-17-jre
```

To Verify Java has been Installed: -

```
java -version
```

To install Jenkins: -

```
curl -fsSL https://pkg.jenkins.io/debian/jenkins.io-2023.key | sudo tee \
  /usr/share/keyrings/jenkins-keyring.asc > /dev/null
echo deb [signed-by=/usr/share/keyrings/jenkins-keyring.asc] \
  https://pkg.jenkins.io/debian binary/ | sudo tee \
  /etc/apt/sources.list.d/jenkins.list > /dev/null
sudo apt-get update
sudo apt-get install jenkins
```

#### Note: By default, Jenkins will not be accessible to the external world due to the inbound traffic restriction by AWS. Open the port 8080 in the inbound traffic rules as shown below:

- EC2 > Instances > Click on <Instance-ID>
- In the bottom tabs -> Click on Security
- Security groups
- Add inbound traffic rules as shown in the image (you can just allow TCP 8080 as well, in my case, I allowed `All traffic`).

<img width="1358" height="448" alt="Screenshot (33)" src="https://github.com/user-attachments/assets/0b75016a-105c-4a6f-ac98-5bd57370275b" />

### Login to Jenkins using the below URL:

http://<ec2-instance-public-ip-address>:8080    [You can get the ec2-instance-public-ip-address from your AWS EC2 console page]

login to Jenkins, 
      - Run the command to copy the Jenkins Admin Password - `sudo cat /var/lib/jenkins/secrets/initialAdminPassword`
      - Enter the Administrator password

<img width="1291" height="596" alt="215959008-3ebca431-1f14-4d81-9f12-6bb232bfbee3" src="https://github.com/user-attachments/assets/da06e4e4-52a8-4362-9a7b-13678300f603" />


### Click on Install suggested plugins

Wait for the Jenkins to Install suggested plugins.

<img width="1291" height="596" alt="215959294-047eadef-7e64-4795-bd3b-b1efb0375988" src="https://github.com/user-attachments/assets/b043eef7-4cbe-49e4-b408-fb3ac6bbbf4d" />

##### Create First Admin User or Skip the step [It is recommended to create admin user if this Jenkins instance is to be used for future purposes.]

Jenkins is Successfully installed! 

<img width="990" height="616" alt="215961440-3f13f82b-61a2-4117-88bc-0da265a67fa7" src="https://github.com/user-attachments/assets/7cb50970-081a-4d36-9449-0d7e3887d291" />

## Install the Docker Pipeline plugin in Jenkins:

   - Log in to Jenkins.
   - Go to Manage Jenkins > Manage Plugins.
   - In the Available tab, search for "Docker Pipeline".
   - Select the plugin and click the Install button.
   - Restart Jenkins after the plugin is installed.

<img width="1392" height="556" alt="215973898-7c366525-15db-4876-bd71-49522ecb267d" src="https://github.com/user-attachments/assets/12d39b25-cb4c-47f4-9a06-537fb1471f2d" />

Wait for the Jenkins to be restarted.

## Docker Slave Configuration

Run the below command to Install Docker

```
sudo apt update
sudo apt install docker.io
```
 
#### Grant Jenkins user and Ubuntu user permission to docker deamon.

```
sudo su -
usermod -aG docker jenkins
usermod -aG docker ubuntu
systemctl restart docker
```

After the above steps, it is recommended to restart Jenkins.

```
http://<ec2-instance-public-ip>:8080/restart
```

#### The docker agent configuration is now successful.

#### Note: Ensure your repository contains `HelloWorld.java` and a `Jenkinsfile` with the code before doing the rest of the steps.

## Run the Pipeline in Jenkins

  -Open Jenkins → New Item → Pipeline

  -Name it something like "java-docker-pipeline".

  -Under Pipeline → Definition, select `Pipeline script from SCM`.

  -Choose Git and provide:

       ->Repository URL: https://github.com/<your-username>/<your-repo>.git
       ->Branch: main (or your branch name)

## Run the Pipeline.

   Click  `Build Now`.

#### Jenkins will pull the code from GitHub, compile, and run it.

Output should be: -

<img width="1267" height="595" alt="Screenshot (32)" src="https://github.com/user-attachments/assets/4c80802c-dd5d-4223-b7ba-ff54f6878528" />
