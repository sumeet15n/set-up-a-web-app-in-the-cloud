# Set Up a Web App in the Cloud

## Introducing this Project!

In this project, I set up a web app in the cloud.

### Services Used

- IAM
- EC2

### Concepts Learnt

- 

### Architecture Diagram

![Image](https://github.com/sumeet15n/set-up-a-web-app-in-the-cloud/blob/master/Screenshots/SS0.png)

---

## Project Guide

### Step 1: Launch an EC2 instance

First, I signed into the AWS management console as an IAM user with admin access and not as the root user of my AWS account (best practice), navigated to EC2, and set my region as the one closest to my geographical location (ap-south-1) to minimize the latency (another best practice).

I launched a new EC2 instance, with 'nextwork-devops-sumeet' as the name, linux as the AMI, and t2.micro as the instance type. While launching the EC2 instance, I created a key pair with 'nextwork-keypair' as the name, and downloaded the .pem key pair file. Under network settings, I changed 'Allow network traffic from' to 'My IP' instead of 'Anywhere' to ensure that only I could access my EC2 instance.

### Step 2: Change permission settings of .pem file

Next, I changed the permission settings of my .pem key pair file by running the below commands in my terminal on VSCode.

```
icacls "nextwork-keypair.pem" /reset
icacls "nextwork-keypair.pem" /grant:r "USERNAME:R" // replace USERNAME with your username that can be found by running the whoami command in the terminal
icacls "nextwork-keypair.pem" /inheritance:r
```

A screenshot of my permission settings being changed is shown below.

![Image](https://github.com/sumeet15n/set-up-a-web-app-in-the-cloud/blob/master/Screenshots/SS1.png)

### Step 3: SSH into the EC2 instance

I connected to my EC2 instance by running the below command in my terminal on VSCode.

```
ssh -i [PATH TO YOUR .PEM FILE] ec2-user@[YOUR PUBLIC IPV4 DNS] // replace both placeholders accordingly
```

A screenshot of the successful connection is shown below.

![Image](https://github.com/sumeet15n/set-up-a-web-app-in-the-cloud/blob/master/Screenshots/SS2.png)

### Step 4: Install Apache Maven and Amazon Corretto

I installed Apache Maven in my EC2 instance by running the below commands.

```
wget https://archive.apache.org/dist/maven/maven-3/3.5.2/binaries/apache-maven-3.5.2-bin.tar.gz
sudo tar -xzf apache-maven-3.5.2-bin.tar.gz -C /opt
echo "export PATH=/opt/apache-maven-3.5.2/bin:$PATH" >> ~/.bashrc
source ~/.bashrc
```

Next, I installed Amazon Corretto 8 in my EC2 instance by running the below commands.

```
sudo dnf install -y java-1.8.0-amazon-corretto-devel
export JAVA_HOME=/usr/lib/jvm/java-1.8.0-amazon-corretto.x86_64
export PATH=/usr/lib/jvm/java-1.8.0-amazon-corretto.x86_64/jre/bin/:$PATH
```

I also ran the below commands to verify that Maven and Corretto have been successfully installed.

```
mvn -v
java -version
```

### Step 5: Create an application

Now that Maven and Corretto were installed, I generated a Java web app by running the below command.

```
mvn archetype:generate \
   -DgroupId=com.nextwork.app \
   -DartifactId=nextwork-web-project \
   -DarchetypeArtifactId=maven-archetype-webapp \
   -DinteractiveMode=false
```

A screenshot of the build success message is shown below.

![Image](https://github.com/sumeet15n/set-up-a-web-app-in-the-cloud/blob/master/Screenshots/SS3.png)

### Step 6: Connect VSCode with the EC2 instance

I installed the 'Remote - SSH' extension on VSCode. After installing this extension, I clicked on the double arrow icon ('><') at the bottom left corner of VSCode, which is a shortcut to use the 'Remote - SSH' extension, clicked on 'Connect to host', clicked on 'Add new SSH host', and ran the below command.

```
ssh -i [PATH TO YOUR .PEM FILE] ec2-user@[YOUR PUBLIC IPV4 DNS] // replace both placeholders accordingly
```

After the host was added, I clicked on the double arrow icon ('><') again and then clicked on 'Connect to host'. This time, there was an option to connect to added host, and once I clicked on it, a new VSCode window opened, I was successfully connected to my EC2 instance on VSCode. Upon opening the explorer tab, I could now see all the folders and files of my EC2 instance, as shown in the below screenshot. In the index.jsp file, I replaced the {Your Name} placeholder with my name 'Sumeet'.

![Image](https://github.com/sumeet15n/set-up-a-web-app-in-the-cloud/blob/master/Screenshots/SS4.png)

### Step 7: Delete Resources

Finally, I deleted all the created resources after completing this project to avoid any further charges on AWS.

---

## Project Reflection

This project took me approximately 30-45 minutes to complete. The most challenging part was to troubleshoot the '403 Forbidden' error, and the most rewarding part was to see the website being successfully hosted.

Big thanks to NextWork (https://www.nextwork.org/) for this project! I highly recommend this platform to anyone who wants to learn DevOps concepts and complete more projects like this one. Happy learning!