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