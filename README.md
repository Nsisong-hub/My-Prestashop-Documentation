# PrestaShop Installation on AWS with EC2 Instance Connect
This documentation details my step by step deployment of PrestaShop using AWS Free Tier resources. It includes setting up an EC2 instance for the application server, an RDS instance for the database, and configuring PrestaShop to be publicly accessible.

  ## Architectural Diagram
![WhatsApp Image 2025-01-23 at 23 14 49_d7603c9c](https://github.com/user-attachments/assets/dc3ab58e-35e2-4079-958d-aca07168d7e2)


# Prerequisites
* An AWS Free Tier account.
* Basic knowledge of Linux and AWS services.
* SSH key pair for EC2 instance access (if not using EC2 Instance Connect).
* The PrestaShop zip file ready for upload(if downloaded into local machine)

## Step 1: Launch an EC2 Instance for PrestaShop
**1. Navigate to the AWS Management Console**
* Go to the EC2 service 
<img width="1118" alt="pd1" src="https://github.com/user-attachments/assets/edbd391c-cd09-4bea-a929-a5ed4ae72e30" />

* Click on Launch Instances
<img width="1120" alt="pd2" src="https://github.com/user-attachments/assets/077d35c5-7cd8-4903-8471-43e196575574" />


**2. Name your instance and choose an Amazon Machine Image (AMI)**
* Provide a descriptive name to your instance
* Select Ubuntu Server 22.04 LTS as the AMI
* Ensure it is eligible for the Free Tier.

<img width="1120" alt="pd3" src="https://github.com/user-attachments/assets/d0f10c66-6799-4407-af48-2b1b7cab9eeb" />

**3. Instance Type and key pair**
* Choose t2.micro for the instance type (Free Tier eligible)
* Select an existing key pair or create a new one
<img width="1120" alt="pd4" src="https://github.com/user-attachments/assets/69890f11-647f-4a97-8118-014478e713e4" />

**4. Configure Network settings**
* Keep the Default VPC
* Set availability zone to us-east-1a
* Enable Auto-assign Public IP to ensure public accessibility
* Create security group or select exiting security group
  
 <img width="1120" alt="pd5" src="https://github.com/user-attachments/assets/516ead48-a01a-4a5a-8a1e-711df913e642" />
 
 **5. Configure Security Group**
 * Allow HTTP (80), HTTPS (443), MYSQL/Aurora (3306), and SSH (22) traffic.
   
 <img width="1120" alt="pd6" src="https://github.com/user-attachments/assets/12d0eb28-3309-4d58-914e-f1d47d2a7f05" />

 **6. Launch the Instance**
 * Leave storage configuration at default
 * Review and click Launch.

 <img width="1120" alt="pd7" src="https://github.com/user-attachments/assets/1a85b4c1-9761-4bde-b6b1-6b0428652937" />
 <img width="1120" alt="pd8" src="https://github.com/user-attachments/assets/18a4d4de-13b8-4a06-858f-8a53b3b76850" />
 <img width="1120" alt="pd9" src="https://github.com/user-attachments/assets/aeca5c4f-1956-4af5-a64b-6f6accc4eac7" />

 # Step 2: Launch an RDS Instance for the Database
**1. Navigate to the RDS Service**
* Go to the RDS section of the AWS Console
  
 <img width="1114" alt="pd10" src="https://github.com/user-attachments/assets/fef7d063-4ede-4a2b-82b6-e662391fc7c9" />
 
 * Click on Create Database
 
 <img width="1120" alt="pd11" src="https://github.com/user-attachments/assets/98c0e9c9-8bcf-4595-9c27-5880591e5b9c" />

 **2. Select Database Creation Method**
 * Choose Standard Create.

 **3. Database Engine**
 * Select MySQL.
   
   <img width="1115" alt="pd12" src="https://github.com/user-attachments/assets/a55fcc89-d20f-433d-9a29-a993acc01aa0" />

 **4. Database Engine**
 * Select MYSQL
 * Choose version 8.0.32 (Free Tier eligible)

**5. Templates** 
 * Select Free Tier
 <img width="1120" alt="pd13" src="https://github.com/user-attachments/assets/bc440b53-6490-4bc0-b358-fdc4576467a8" />

**6. Login Settings**
* Give your database a descriptive name, mine, i used prestashopdb
* Configure Master username, mine, i used admin
* Configure Master password
   
<img width="1115" alt="pd14" src="https://github.com/user-attachments/assets/d47b24be-a167-48eb-8fe6-e54335910f8b" />

**7. Instance Specifications**
* Enable precious generation classes
* Select db.t3.micro (Free Tier eligible)
  
<img width="1120" alt="pd15" src="https://github.com/user-attachments/assets/3a946261-4c1b-4e1f-85d5-daa500ef356f" />

**8. Connectivity**
* Select the same default VPC you used on ec2 instance
* Set DB subnet group to Default
* Disable public accessibility for the database.

<img width="1106" alt="pd16" src="https://github.com/user-attachments/assets/184665c5-28c4-4d13-9f1e-47ef3a7a7050" />

**9. VPC Security group**
* Choose exiting and select exactly the same security group configured on your ec2 instance
* I used `launch-wizards-1`
* Choose the same availability zone as configured on your ec2 instance
* I `used us-east-1a`
<img width="1120" alt="pd17" src="https://github.com/user-attachments/assets/1c4c4eed-9d99-482b-9b82-69d2eeaa3a3e" />

**10. Additional Configuration**
* Ensure the database port is set to port 3306
  
<img width="1120" alt="pd18" src="https://github.com/user-attachments/assets/a993babd-03e7-4b60-b36d-9e4a8cbab3cf" />

* Configure initial database name
  
 <img width="1120" alt="pd19" src="https://github.com/user-attachments/assets/1e7f01bf-9674-49fc-9fb9-a818a60d24ed" />

 **11. Launch the RDS Instance**
* Leave the rest of the settings on default
* Review and click Create Database

<img width="1120" alt="pd20" src="https://github.com/user-attachments/assets/9b4e24c0-7632-48fd-ac20-a874265d1881" />
<img width="1120" alt="pd21" src="https://github.com/user-attachments/assets/e313c937-d325-4d57-bc76-f9fb90007890" />
<img width="1120" alt="pd22" src="https://github.com/user-attachments/assets/739a1d9e-d507-454b-9aee-6bb7dd4a9780" />


# Step 3: Set Up the EC2 Instance
**1. Connect to the Instance**
* On the search bar, type and click on EC2
  
   <img width="1120" alt="pd23" src="https://github.com/user-attachments/assets/b02182f1-d33d-400b-9d15-77213b769b11" />

* Navigate downward and click on connect
 <img width="1120" alt="pd24" src="https://github.com/user-attachments/assets/adf20608-2600-4b99-b04b-f3d519b7e53b" />

* Use EC2 Instance Connect to SSH into the instance
* Select EC2 instance connect and then navigate down and click connect
  
<img width="1108" alt="pd25" src="https://github.com/user-attachments/assets/0ad64b36-0dad-4706-8063-fa12543b9cfa" />
<img width="1114" alt="pd26" src="https://github.com/user-attachments/assets/96c7f012-d4d4-4188-8252-f594ed9e6231" />


**2. Update and Upgrade Ubuntu**
   
* Before installing PrestaShop, ensure your Ubuntu system is up-to-date. Run the following commands to update the package index and upgrade the system:
  ```bash
  sudo apt update && sudo apt upgrade -y

  ```
<img width="1121" alt="pd27" src="https://github.com/user-attachments/assets/2c3d316a-be51-4606-9aa9-44df8c587b9e" />

**3. Install Apache Web Server**
   
* Next, install the Apache web server using the following command:
  
```bash
sudo apt install apache2
```
<img width="1116" alt="pd28" src="https://github.com/user-attachments/assets/907e6739-c11d-49b9-b7c2-97f11cc6ef2a" />
This command installs the Apache web server and its dependencies



**4. Configure Apache to Allow .htaccess Files**
* By default, Apache does not allow `.htaccess` files to override its configuration settings
* To enable this feature, open the default Apache configuration file using a text editor:
```bash
 sudo vi /etc/apache2/sites-available/000-default.conf
```
* Add the following code to the configuration file
  
```bash
<Directory /var/www/html>
 AllowOverride All
</Directory>
```  
<img width="1120" alt="pd30" src="https://github.com/user-attachments/assets/916aa42e-e8d4-4a72-b8a5-2410525b5808" />

* This configuration change allows `.htaccess` files to override Apache settings
* Save and close the file
* Next, enable the `mod_rewrite` Apache module, which is required for PrestaShop:

```bash
sudo systemctl restart apache2
```

**5. Install PHP7.4**
* PrestaShop is built using PHP, so install it on your Ubuntu system but before that...
* Note that prestashop recommends using at most PHP v7.4 or lower version for the latest version of their software
* Add the PHP 7.4 PPA repository to your Ubuntu system using the following command:
```bash
sudo add-apt-repository ppa:ondrej/php
```
<img width="1121" alt="pd32" src="https://github.com/user-attachments/assets/da1afca5-fe81-4a8c-9a83-3bfc182916aa" />

* Update your repositories by running the following command:
```bash
sudo apt update
```

<img width="1103" alt="pd33" src="https://github.com/user-attachments/assets/7d5ae06f-e338-49cd-8c88-8e3600b1a3f6" />


* Now install PHP 7.4 and the required extensions for Prestashop using the following command:
```bash
sudo apt install php7.4 php7.4-cli php7.4-common php7.4-mysql php7.4-xml php7.4-curl php7.4-zip php7.4-mbstring php7.4-intl php7.4-gd php7.4-json php7.4-opcache php7.4-soap php7.4-bcmath

```
<img width="1124" alt="pd34" src="https://github.com/user-attachments/assets/b73af38c-f1ac-4acd-b392-1d2ec7cf4caa" />

* Next, enable the Apache rewrite module and restart the Apache service to apply the changes:
```bash
sudo apt2enmod php7.4
sudo apt2enmorewrite
sudo systemctl enable apache2
sudo systemctl start apache2

```
<img width="1109" alt="pd35" src="https://github.com/user-attachments/assets/db8fddc7-195b-4087-9fea-204926c1cffe" />

**6. Download and Install PrestaShop**
* I downloaded Prestashop directly into my local machine
* To upload it onto my remote EC2 instance, i used the command:
```bash
scp -i /path/to/privatekey.pem /path/to/localfile username@remote_host:/path/to/destination/
```
* -i /path/to/privatekey.pem: Specifies the private key for authentication
* /path/to/localfile: Path to the file on your local machine
* username@remote_host: SSH username and IP address (or domain) of the remote server
* /path/to/destination/: Path on the remote server where the file will be copied

<img width="868" alt="pd36" src="https://github.com/user-attachments/assets/c18a501d-333b-4cf3-b8a3-13c9d5d0e44f" />

* ls -l into the remote EC2 instance home directory, the uploaded prestashop zip file is there

<img width="1112" alt="pd37" src="https://github.com/user-attachments/assets/3cbdbc7d-6355-4de9-be49-ebf7e0a10e56" />

* To unzip the file, first install unzip with the command:
```bash
sudo apt install unzip

```
<img width="1117" alt="pd38" src="https://github.com/user-attachments/assets/4c8e95b4-ebb1-4584-8d08-ead20464c71c" />

* Extract the prestashop to the default Apache document root directory and set proper permissions with these commands:

```bash
sudo unzip prestashop_edition_basic_version_8.2.0.zip -d /var/www/html
sudo chown -R www-data:www-data /var/www/html
sudo chmod -R 755 /var/www/html
```
<img width="1122" alt="pd39" src="https://github.com/user-attachments/assets/d951feba-3037-4ba5-8cfb-71c9a3fc08d5" />

**7. Remove the default Apache page and finalize:**
* Change the current directory to the default Apache document root directory `/var/www/html`
* Remove the default Apache welcome page to allow PrestaShop to load as the main site
* Restart the Apache server to apply all recent changes
* And then delete the /install directory, which is a necessary security measure after PrestaShop setup to prevent unauthorized reinstallation.
```bash
cd /var/www/html  
sudo rm index.html  
sudo systemctl restart apache2  
sudo rm -rf /var/www/html/install  
```
<img width="1120" alt="pd40" src="https://github.com/user-attachments/assets/ea4da778-c3a1-457f-8430-cbae0f104aba" />

# Step 4: Access PrestaShop via the Public URL
* Ensure your security group allows HTTP traffic on port 80.
* Once the above steps are complete, you can access your PrestaShop site by visiting:
```bash
http://<Your_EC2_Public_IP>
```
<img width="1120" alt="pd41" src="https://github.com/user-attachments/assets/2b46bf56-b406-472c-85c2-fb3420209ccc" />

<img width="1120" alt="pd42" src="https://github.com/user-attachments/assets/adceb779-9295-4b68-895f-a3c11fb85a81" />
Choose your language and then click on Next. You should see the following page:

<img width="1120" alt="pd43" src="https://github.com/user-attachments/assets/65fdd369-3fad-420f-8846-0634ec910aeb" />
Accept the license and click on the Next. You should see the following page:

<img width="1120" alt="pd44" src="https://github.com/user-attachments/assets/ff93af44-595f-412c-80a6-6d083085e289" />

<img width="1120" alt="pd45" src="https://github.com/user-attachments/assets/a8ef10e8-5e3b-419f-ab47-c428036df839" />
Provide your site information and click on the Next. You should see the following page:
<img width="1120" alt="pd46" src="https://github.com/user-attachments/assets/6da6a02c-fa14-4bf9-8f9b-dd4969c64a73" />

<img width="1118" alt="pd47" src="https://github.com/user-attachments/assets/87211fd8-b453-4bc3-acba-c191f7fb20ce" />
Provide your database information and click on the Next. You should see the following page:
<img width="1120" alt="pd48" src="https://github.com/user-attachments/assets/df8b5d94-04fe-4785-a5a6-8404a1e2e3bd" />
Click on the “Manage your store“. You will be redirected to the following page:
<img width="1120" alt="pd49" src="https://github.com/user-attachments/assets/8d3635da-86cb-446a-b67b-78c059d9e74f" />

# Conclusion
This solution ensures that the database is not hosted on the same server as the application and here is how; the MySQL database is hosted on Amazon RDS, a separate managed service provided by AWS. This guarantees the database is on a different server, physically and logically separate from the EC2 instance running the PrestaShop application. Secondly the connection between PrestaShop (running on the EC2 instance) and the MySQL database (hosted on RDS) is configured via the RDS endpoint as no database is installed or hosted on the EC2 instance itself.

# Recommendation
However, this whole setup is more of a traditonal (manual installation), you can simplify the application Deployment by running the PrestaShop application via Docker on the EC2 instance as the PrestaShop and its dependencies are pre-packaged in the Docker image, thereby reducing manual setup.
























