# Scalable 3-Tier Web Application on AWS 🚀

This project demonstrates how to design and deploy a **highly available and scalable 3-tier architecture** on AWS. The setup follows industry best practices using VPC, EC2, Application Load Balancer, and RDS to ensure security, high availability, and fault tolerance.

Based on implementation steps from the project document: 

---

## 📊 Architecture Overview

A 3-tier architecture divides the application into three logical layers:

| Layer              | AWS Service                     | Purpose              |
| ------------------ | ------------------------------- | -------------------- |
| Presentation Layer | Application Load Balancer + EC2 | Handles user traffic |
| Application Layer  | EC2 Instances (Private Subnet)  | Business logic       |
| Database Layer     | Amazon RDS                      | Data storage         |

---

## 🏗️ Architecture Components

### Networking

* Custom VPC
* Public Subnets
* Private Subnets
* Internet Gateway
* NAT Gateway
* Route Tables
* Security Groups

### Compute

* Bastion Host (Jump Server)
* EC2 Instances (Amazon Linux)
* Apache Web Server
* PHP 8.2

### Database

* Amazon RDS
* phpMyAdmin configuration

### Load Balancing

* Application Load Balancer
* Target Groups
* Health Checks

---

## 📂 Project Workflow

### Step 1 – VPC Setup

Create a custom VPC and subnets across multiple Availability Zones for high availability.

Example CIDR configuration:

| Resource             | CIDR        |
| -------------------- | ----------- |
| VPC                  | 20.0.0.0/20 |
| Public Subnet 1      | 20.0.1.0/24 |
| Public Subnet 2      | 20.0.2.0/24 |
| Private App Subnet 1 | 20.0.3.0/24 |
| Private App Subnet 2 | 20.0.4.0/24 |
| Private DB Subnet 1  | 20.0.5.0/24 |
| Private DB Subnet 2  | 20.0.6.0/24 |

Configure:

* Internet Gateway for public subnets
* NAT Gateway for private subnets
* Route tables

---

### Step 2 – EC2 Setup

Launch instances:

| Instance    | Purpose            |
| ----------- | ------------------ |
| jump-server | Bastion Host       |
| app-server1 | Application server |
| app-server2 | Application server |

Security groups:

* Allow SSH (22)
* Allow HTTP (80)
* Allow internal communication between instances

---

### Step 3 – Server Configuration

SSH into private servers through bastion host and install dependencies.

Update system:

```bash
sudo yum update -y
```

Install PHP:

```bash
sudo dnf install php8.2
sudo yum install php8.2-mysqlnd
```

Install Apache:

```bash
sudo yum install -y httpd
sudo systemctl start httpd
sudo systemctl enable httpd
```

Configure permissions:

```bash
sudo usermod -a -G apache ec2-user
sudo chown -R ec2-user:apache /var/www
sudo chmod 2775 /var/www
```

Install additional PHP modules:

```bash
sudo yum install php-mbstring php-xml -y
sudo yum install php-fpm
```

Restart services:

```bash
sudo systemctl restart httpd
sudo systemctl restart php-fpm
```

---

### Step 4 – Install phpMyAdmin

```bash
cd /var/www/html

wget https://www.phpmyadmin.net/downloads/phpMyAdmin-latest-all-languages.tar.gz

mkdir phpMyAdmin

tar -xvzf phpMyAdmin-latest-all-languages.tar.gz -C phpMyAdmin --strip-components 1

rm phpMyAdmin-latest-all-languages.tar.gz
```

Create test pages:

Server 1:

```bash
echo "PHP server 1" > /var/www/html/index.html
```

Server 2:

```bash
echo "PHP server 2" > /var/www/html/index.html
```

---

### Step 5 – Configure Load Balancer

Create:

* Target group
* Application Load Balancer
* Listener on port 80

Register EC2 instances to target group.

Test using Load Balancer DNS.

Refreshing browser should alternate between:

```
PHP server 1
PHP server 2
```

---

### Step 6 – Configure Database

Connect phpMyAdmin to RDS:

```bash
cd /var/www/html/phpMyAdmin

mv config.sample.inc.php config.inc.php

nano config.inc.php
```

Update database endpoint:

```php
$cfg['Servers'][$i]['host'] = 'mydb-project.c65k6wsy6xkg.us-east-1.rds.amazonaws.com';
```

Login using database credentials.

---

## 🖼️ Screenshots

Add screenshots from project:

```
Images/
├── vpc.png
├── ec2.png
├── alb.png
├── phpmyadmin.png
```

---

## 🧠 Skills Demonstrated

* AWS VPC Design
* High Availability Architecture
* Load Balancing
* Bastion Host Configuration
* Linux Administration
* Apache Web Server
* PHP Installation
* RDS Connectivity
* Secure Networking

---

## 🚀 How to Run

1. Create VPC and subnets
2. Launch EC2 instances
3. Configure Apache and PHP
4. Install phpMyAdmin
5. Configure Load Balancer
6. Connect RDS database
7. Test using Load Balancer DNS

---

## 📈 Future Improvements

* Infrastructure as Code (Terraform)
* Auto Scaling Group
* HTTPS using ACM
* CI/CD pipeline integration
* Docker containerization

---

## 👤 Author

Mohammed Abdul Sohail

GitHub:
https://github.com/SOHAIL62786

---

## 📜 License

For educational and portfolio use.
