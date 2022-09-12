# Configure Jenkins Instance

Create an AWS EC2 server based on Ubuntu Server 20.04 LTS and name it "Jenkins-Ansible"
![Jenkins Instance](./images/1.png)
Install JDK (since Jenkins is a Java-based application)

```bash
sudo apt update -y
sudo apt install default-jdk-headless -y
```

![Jenkins Instance](./images/2.png)

Install Jenkins

```bash
wget -q -O - https://pkg.jenkins.io/debian-stable/jenkins.io.key | sudo apt-key add -
sudo sh -c 'echo deb https://pkg.jenkins.io/debian-stable binary/ > \
    /etc/apt/sources.list.d/jenkins.list'

sudo apt update
sudo apt install jenkins -y
```

![Jenkins Installation](./images/3.png)

Make sure Jenkins is up and running

```bash
systemctl status jenkins
```

![status jenkins](./images/4.png)

By default Jenkins server uses TCP port 8080 – open it by creating a new Inbound Rule in your EC2 Security Group

![Security Group](./images/5.png)

## Perform initial Jenkins setup.

From your browser access 
*( http:Jenkins-Server-Public-IP-Address-or-Public-DNS-Name:8080 )*

You are prompted to provide a default admin password

![Security Group](./images/6.png)

Enter the following line of code to retrieve it from your server

```sudo
sudo cat /var/lib/jenkins/secrets/initialAdminPassword
```

![Admin password](./images/7.png)

Install the suggested plugins and create a username and password for login authentication, finally click on ```save and finish```

![Admin password](./images/8.png)

In your GitHub account, create a new repository and name it *```ansible-config-mgt```*.

![Admin password](./images/9.png)

Install Ansible

```bash
sudo apt update -y
sudo apt install ansible -y
```

![Install Ansible](./images/10.png)
Check your Ansible version by running 

```bash
ansible --version
```

![Ansible version](./images/11.png)

Configure Jenkins build job to save your repository content every time you change it.

Create a new Freestyle project *```ansible```* in Jenkins and point it to your *```‘ansible-config-mgt’```* repository.
> On the dashboard, click on Create a job, enter the job name *```Ansible```*and click on *```Freestyle project```* and click Ok

![Create new job](./images/12.png)

Copy the url for the *```ansible-config-mgt```* repo
![Create new job](./images/13.png)

Under *```Source Code Management```*, click on *```Git```*, paste the repo url in the ```Repository URL``` field , under *```credentials```* , click *```Add```* - enter your Github login details

![Create new job](./images/14.png)

Click on ```Build Triggers``` and select ```GitHub hook trigger for GITScm polling``` to Configure triggering the job from GitHub webhook
![Create new job](./images/15.png)

Configure ```"Post-build Actions"``` to archive all the files – files resulted from a build are called "artifacts". Click on ```Add post-build action``` and select ```Archive the artifacts```. In the ```Files to archive?``` input ```**``` to archive mulitiple nested artifacts and click on save.
![Create new job](./images/16.png)

Configure ```ansible-config-mgt``` repo to use webhooks.

In the ```ansible-config-mgt``` repo, click on ```Settings > Webhooks > Add Webhooks```. Enter the Payload URL ```( Jenkins ip/github-webhook/)```, Content type ```apllication/json``` and click ```Add webhook```
![Create new job](./images/17.png)

Test setup by making some change in README.MD file in master branch and commit changes.
![Create new job](./images/18.png)

Make sure that builds starts automatically and Jenkins saves the files (build artifacts) in following folder

```bash
ls /var/lib/jenkins/jobs/Ansible/builds/4/archive/
```

![configuration succesful](./images/19.png)

Clone down your ansible-config-mgt repo to your Jenkins-Ansible instance

```bash
git clone https://github.com/oayanda/ansible-config-mgt.git
```

![configuration succesful](./images/21.png)

## BEGIN ANSIBLE DEVELOPMENT

create a new branch that will be used for development

![configuration succesful](./images/22.png)

Create two directories and name them ```playbooks``` and ```inventory``` respectively

![configuration succesful](./images/23.png)