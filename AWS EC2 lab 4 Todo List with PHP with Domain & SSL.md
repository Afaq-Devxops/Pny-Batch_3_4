# **Deploy To-Do List PHP Application with Domain and SSL/TLS**

---

## **Objective**

This guide will walk you through:

1. Hosting the PHP To-Do List application on an AWS EC2 instance.
2. Connecting a custom domain to your application.
3. Enabling SSL/TLS with Let’s Encrypt.

---

## **Step 1: Launch an Ubuntu EC2 Instance**

1. **Log in to the AWS Console** and navigate to the EC2 Dashboard.
2. **Launch an Instance:**
    - Choose **Ubuntu Server 20.04 LTS** AMI.
    - Select **t2.micro** as the instance type (Free Tier Eligible).
    - Configure the security group:
        - Allow **HTTP (Port 80)**.
        - Allow **HTTPS (Port 443)**.
        - Allow **SSH (Port 22)** (Restrict to your IP for security).
    - Assign or create a **key pair** for SSH access.
3. **Launch the Instance** and note its public IP address.

---

## **Step 2: Connect to the EC2 Instance**

1. Open your terminal and SSH into the instance:
    
    ```bash
    ssh -i "your-key.pem" ubuntu@<INSTANCE_PUBLIC_IP>
    ```
    
2. Update the system:
    
    ```bash
    sudo apt update -y
    sudo apt upgrade -y
    ```
    

---

## **Step 3: Install Apache, PHP, and MySQL**

1. Install Apache:
    
    ```bash
    sudo apt install apache2 -y
    sudo systemctl start apache2
    sudo systemctl enable apache2
    ```
    
2. Install PHP and required modules:
    
    ```bash
    sudo apt install php libapache2-mod-php php-mysql -y
    ```
    
3. Install MySQL:
    
    ```bash
    sudo apt install mysql-server -y
    ```
    
4. Secure the MySQL installation:
    
    ```bash
    sudo mysql_secure_installation
    ```
    
    - Set a root password and follow the prompts.

---

## **Step 4: Set Up the Database**

1. Log into MySQL:
    
    ```bash
    sudo mysql -u root -p
    ```
    
2. Create a database and table:
    
    ```sql
    CREATE DATABASE todo;
    USE todo;
    EXIT;
    ```
    
3. Clone the GitHub repository and import the database:
    
    ```bash
    sudo apt install git -y
    git clone https://github.com/ali-azgar-rakib/Todo-list-with-php.git
    cd Todo-list-with-php
    sudo mysql -u root -p todo < todo.sql
    ```
    

---

## **Step 5: Deploy the To-Do Application**

1. Move application files to Apache's web directory:
    
    ```bash
    sudo mv * /var/www/html/
    ```
    
2. Set proper permissions:
    
    ```bash
    sudo chmod -R 755 /var/www/html/
    ```
    
3. Restart Apache:
    
    ```bash
    sudo systemctl restart apache2
    ```
    
4. Test the application:
    
    - Open your browser and visit:
        
        ```
        http://<INSTANCE_PUBLIC_IP>
        ```
        

---

## **Step 6: Attach a Custom Domain**

1. **Point Your Domain to the EC2 Instance:**
    
    - Log into your domain registrar or Route 53.
    - Add an **A Record**:
        - **Name:** `@` or your subdomain (e.g., `www`).
        - **Value:** EC2 instance public IP.
    - Save the changes.
2. **Verify the DNS Configuration:**
    
    - Use the `dig` command to confirm:
        
        ```bash
        dig yourdomain.com
        ```
        
    - Ensure it resolves to your EC2 instance's IP.

---

## **Step 7: Install Certbot for SSL/TLS**

1. Install Certbot:
    
    ```bash
    sudo apt install certbot python3-certbot-apache -y
    ```
    
2. Obtain an SSL certificate:
    
    ```bash
    sudo certbot --apache -d yourdomain.com -d www.yourdomain.com
    ```
    
    - Follow the prompts and confirm the setup.
3. Verify SSL is working:
    
    - Visit:
        
        ```
        https://yourdomain.com
        https://www.yourdomain.com
        ```
        
4. Test certificate renewal:
    
    ```bash
    sudo certbot renew --dry-run
    ```
    

---

## **Step 8: Redirect HTTP to HTTPS**

1. Open the Apache configuration file:
    
    ```bash
    sudo nano /etc/apache2/sites-available/000-default.conf
    ```
    
2. Add a redirect rule:
    
    ```apache
    <VirtualHost *:80>
        ServerName yourdomain.com
        ServerAlias www.yourdomain.com
        Redirect permanent / https://yourdomain.com/
    </VirtualHost>
    ```
    
3. Restart Apache:
    
    ```bash
    sudo systemctl restart apache2
    ```
    

---

## **Step 9: Verify the Setup**

1. **Open your browser** and visit your domain:
    
    - `https://yourdomain.com`
    - Verify the secure lock icon in the address bar.
    - Ensure the To-Do application loads properly.
2. **Test Application Features:**
    
    - Add, edit, delete, and view tasks.

---

## **Step 10: Secure and Maintain**

1. Restrict SSH access in the EC2 security group to your IP.
2. Monitor logs for any issues:
    - Apache access logs: `/var/log/apache2/access.log`
    - Apache error logs: `/var/log/apache2/error.log`
3. Ensure certificates renew automatically via Certbot.
