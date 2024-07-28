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
    Set the type to "All traffic."
    Set the source to "Anywhere-IPv4" to allow connections from any IP address.
* Save the rules.

### SSH into your EC2 from your local machine.

      ssh -i "stock-market-kafka-key-pair.pem" ec2-user@ec2-54-84-85-196.compute-1.amazonaws.com



  

