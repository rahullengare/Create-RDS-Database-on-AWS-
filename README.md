# Create-RDS-Database-on-AWS-
Create a relational database using Amazon RDS and connect it to an EC2 instance for interaction only.
## About Project:

The goal of this project is to demonstrate how to deploy a **managed relational database** in the cloud using **Amazon RDS**, and securely connect to it from an **Amazon Linux EC2 instance**. This setup reflects real-world backend infrastructure used in scalable web applications, enterprise systems, and civic tech platforms.

## Technologies Used:

- **Amazon RDS** → Managed cloud database (MySQL/PostgreSQL)
- **Amazon EC2** → Linux-based virtual server for testing
- **MySQL/PostgreSQL Client** → CLI tool to connect to RDS
- **AWS VPC** → Isolated network environment for EC2 and RDS
- **Security Groups** → Firewall rules to control access
- **SSH** → Secure shell access to EC2
- **AWS Console** → GUI interface for provisioning and monitoring

---

### 🔹 What is a Database?

A **database** is a structured system for storing and retrieving data efficiently. In this project, we use a **relational database**, which organizes data into tables with defined relationships—ideal for structured records like student data, transactions, or user profiles.

### 🔹 What is Amazon RDS?

**Amazon RDS (Relational Database Service)** is a fully managed service that simplifies database setup, operation, and scaling. It automates tasks like backups, patching, and replication, allowing developers to focus on application logic rather than infrastructure.

---

## Step 1: Create a RDS Database

1. Go to AWS Console → Go to Aurora and RDS Services 
2. Click on the Database Tab → Click to Create Database   
    - Choose a database creation method → Standard create
    - Engine options (Engine type)→ **MariaDB** (version - MariaDB 11.4.5)
    - **Templates → Free tier**
    - DB instance identifier → rds-db
    - Credentials Settings
        - Master username → admin
        - Check-in → **Auto generate password**
    - Instance →db.t4g.micro
    - Storage Setting
        - Storage type → General Purpose SSD (gp2)
        - Allocated storage → 20GB (default)
        - Check-in → Enable storage autoscaling
        - Maximum storage threshold → 1000GB (default)
    - Connectivity
    - Virtual private cloud (VPC) → Default
    - DB subnet group → Default
    - Public access → No
    - Existing VPC security groups → launch-wizard-1 (mysql-3306)
    - Availability Zone → Any one (Optional)
    - Database authentication → Password authentication
    - Additional configuration
        - Initial database name → <write db name directed created by rds>
    - Create Database

![Project Screenshot](/images/RDS1.png)
![Project Screenshot](/images/RDS2.png)
![Project Screenshot](/images/RDS3.png)
![Project Screenshot](/images/RDS4.png)
![Project Screenshot](/images/RDS5.png)
![Project Screenshot](/images/RDS6.png)
![Project Screenshot](/images/RDS7.png)
![Project Screenshot](/images/RDS8.png)
![Project Screenshot](/images/RDS9.png)

3. Now RDS Database is Created 
4. copy password & endpoint 

```sql
#password 
82WiUYfP9gbZh4ccTHmc
#endpoint 
rds-db.c8taowgcq72q.us-east-1.rds.amazonaws.com
```

![Project Screenshot](/images/RDS-done.png)
![Project Screenshot](/images/RDS-pass-endpoint.png)

## Step 2: Create a demo instance

1. Go to AWS Console → EC2 services 
2. Click on the Launch Instance 
    - name → demo
    - AMI → Amazon Linux
    - Instance type → t2.micro
    - Key pair → pem-server-key
    - security group → launch-wizard-1
    - Click on launch instance

![Project Screenshot](/images/instance.png)

3. Connect via SSH 

```bash
ssh -i "pem-server-key.pem" ec2-user@ec2-54-224-4-147.compute-1.amazonaws.com
```

![Project Screenshot](/images/connect-bash.png)
![Project Screenshot](/images/connected.png)

4. update system and install MariaDB server

```bash
sudo yum update
sudo yum install -y mariadb105
sudo systemctl start mariadb
sudo systemctl enable mariadb
```

![Project Screenshot](/images/install-start.png)

5. connect the RDS database 

```bash
# enpoint -> Copy from RDS ->>> rds-db.c8taowgcq72q.us-east-1.rds.amazonaws.com 
mysql -h rds-db.c8taowgcq72q.us-east-1.rds.amazonaws.com -u admin -p
# enter password auto gentreted 
82WiUYfP9gbZh4ccTHmc
```

6. Now you inside the RDS 

```sql
show databses;
exit;
```

![Project Screenshot](/images/RDS-created.png)

## Step 4: Delete RDS & instance

1. Go to RDS Console 
2. Select RDS which you want to delete 
3. Click on Action → Delete 
4. uncheck the box - snapshotDetermines & backupsDetermines
5. then Check-in the acknowledge
6. type delete me then Click Delete

![Project Screenshot](/images/rds-delete-1.png)
![Project Screenshot](/images/rds-delete-2.png)

7. Go to EC2 service then Click on instance 
8. Select instance that you want to delete 
9. Click on Instance state → Terminate the instance 

![Project Screenshot](/images/ec2-delete-1.png)
![Project Screenshot](/images/ec2-delete-2.png)
