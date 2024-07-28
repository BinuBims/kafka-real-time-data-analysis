<h2>This project is about real time data analysis using Kafka and Athena</h2>

### The architecture
<p align="center">
<img src="data/images/architecture.jpeg"  height="400" width="600" />
</p>

### AWS services and Tools
* EC2
* S3
* Crawler
* Data Catalog
* Athena
* Kafka

### Steps taken to implement this architecture
1. Deploy Apache Kafka on an Amazon EC2 Instance
2. Create an S3 Bucket
3. Use Jupyter Notebook to stimulate live stock data
4. Configure a Crawler and a Data Catalog
5. Utilize Athena to Run SQL Queries

## 1. Deploy Apache Kafka on an Amazon EC2 Instance
### Launch an EC2 Instance on AWS
* In the AWS Management Console, search for "EC2" in the search bar and select it.
* Click on "Launch Instance."
* Choose a name for your EC2 instance.
* Select "Amazon Linux" as your Amazon Machine Image (AMI).
* Under "Key pair," create a new key pair and download it for SSH access.

### Launch an EC2 Instance on AWS
* Navigate to "Instances" in the EC2 dashboard.
* Select the instance you launched, then go to the "Security" tab.
* Click on the associated security group to modify its settings.
* Edit the inbound rules by adding a new rule:
  - Set the type to "All traffic."
  - Set the source to "Anywhere-IPv4" to allow connections from any IP address.
* Save the rules.

### SSH into your EC2 from your local machine.
* In the AWS Management Console, search for "EC2" in the search bar and select it.
* Go to "Instances" and click on the instance ID of your EC2 instance.
* Click on "Connect" and choose the "SSH client" tab.
* In the "SSH client" section, you will find the command to connect to your instance. Copy this command to your clipboard.

      ssh -i "stock-market-kafka-key-pair.pem" ec2-user@ec2-54-84-85-196.compute-1.amazonaws.com

* Open a terminal on your local machine.
* Navigate to the directory where your key pair file (.pem) is saved using the cd command.
* Paste and run the copied SSH command in the terminal to connect to your EC2 instance.

### Installing Kafka and Zookeeper
* After connecting to your EC2 instance via SSH, follow these steps to install Kafka:
    Download Kafka
  
        wget https://dlcdn.apache.org/kafka/3.7.1/kafka_2.13-3.7.1.tgz
  
    Extract the Kafka Archive
  
        tar -xzf kafka_2.13-3.7.1.tgz
  
    Kafka requires Java to run. Install it using the following command:
  
        sudo yum install java-22-amazon-corretto-devel

### Configuring Kafka to Use the Public IP Address
* To configure Kafka to use the public IP address instead of the private IP, follow these steps:
    Navigate to Kafka Configuration Directory
            cd kafka_2.13-3.7.1
    Edit the Server Properties File
            sudo nano config/server.properties
* In the file, find the line with advertised.listeners.
* Replace your.host.name with the public IP address of your EC2 instance.
* 
<p align="center">
<img src="data/images/public_ip.JPG"  height="200" width="500" />
</p>
* Press Ctrl + X to exit.
* Press Y to confirm the changes
* Press enter to save and exit.


  

