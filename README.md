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
 
<p align="center">
<img src="data/images/public_ip.JPG"  height="200" width="500" />
</p>

* Press Ctrl + X to exit.
* Press Y to confirm the changes
* Press enter to save and exit.

### Starting the Zookeeper Server
* Open a New Terminal
* Connect to Your EC2 Instance via SSH
* Navigate to the Kafka Directory

    cd kafka_2.13-3.7.1

* Start the Zookeeper Server

    bin/zookeeper-server-start.sh config/zookeeper.properties

### Starting the Kafka Server
* Open Another Terminal
* Connect to Your EC2 Instance via SSH
* Set Memory Allocation for Kafka

    export KAFKA_HEAP_OPTS="-Xmx256M -Xms128M"

* Navigate to the Kafka Directory

    cd kafka_2.13-3.7.1

* Start the Kafka Server

    bin/kafka-server-start.sh config/server.properties

### Setting Up Kafka Producer and Consumer
- Create a Topic in Kafka
    * Open a New Terminal
    * Connect to Your EC2 Instance via SSH
    * Navigate to the Kafka Directory

          cd kafka_2.13-3.7.1

    * Create the Topic

          bin/kafka-topics.sh --create --topic your_topic_name --bootstrap-server your-ec2-public-ip:9092 --replication-factor 1 --partitions 1

- Start a Kafka Producer

          bin/kafka-console-producer.sh --topic your_topic_name --bootstrap-server your-ec2-public-ip:9092

- Start a Kafka Consumer
    * Open Another Terminal
    * Connect to Your EC2 Instance via SSH
    * Navigate to the Kafka Directory

          cd kafka_2.13-3.7.1

    * Start the Consumer
  
          bin/kafka-console-consumer.sh --topic your_topic_name --bootstrap-server your-ec2-public-ip:9092


- To verify everything is working correctly, type a message in the producer terminal. You should see the message appear immediately in the consumer terminal.

<p align="center">
<img src="data/images/kafka-working.gif"  height="400" width="600" />
</p>

## 2. Creating an S3 Bucket and IAM User
* Create a User with S3 Access
  - In the AWS Management Console, type "IAM" in the search bar and select it.
  - Navigate to "Users" and click "Add user."
  - Enter a name for the new user.
  - On the next page, select "Attach policies directly."
  - Search for "AmazonS3FullAccess" and check the box to attach this policy.
  - Continue to the next steps and click "Create user."
 
* Generate Access Keys for the User
  - Return to the "Users" section and click on the username you just created.
  - Go to the "Security credentials" tab.
  - Click "Create access key."
  - Select "Command Line Interface (CLI)" and acknowledge the security warning by ticking the confirmation checkbox.
  - Click "Create access key" and download the .csv file containing the access key ID and secret access key. Store this file
  - securely and do not share it publicly.
    
* Configure AWS CLI on Your Local Machine
  - Follow the instructions in the AWS CLI installation guide to install the AWS CLI on your computer.
  - Open a terminal and run the following command:

        aws configure
    
  - Enter the access key ID and secret access key from the .csv file.
  - Set the default region name to match the region where your S3 bucket is located.


    

  

