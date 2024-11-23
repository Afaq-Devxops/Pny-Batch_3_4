# **Run HTML/CSS Application with Apache on Ubuntu EC2**

---

#### **Step 1: Launch an Ubuntu EC2 Instance**

1. **Log in to the AWS Management Console.**
2. **Launch an Instance:**
    - Select **Ubuntu Server** as the AMI (e.g., Ubuntu 20.04 or newer).
    - Choose an instance type like `t2.micro` (free tier eligible).
    - Configure a **Security Group** to allow:
        - **HTTP (Port 80)** from 0.0.0.0/0.
        - **SSH (Port 22)** from your IP (or 0.0.0.0/0 for testing, but this is insecure).
    - Assign or create a **key pair** for SSH access.

---

#### **Step 2: Connect to the Ubuntu Instance**

1. **SSH into the instance:**
    
    ```bash
    ssh -i "your-key-file.pem" ubuntu@<INSTANCE_PUBLIC_IP>
    ```
    
2. **Update the system:**
    
    ```bash
    sudo apt update
    sudo apt upgrade -y
    ```
    

---

#### **Step 3: Install Apache Web Server**

1. **Install Apache:**
    
    ```bash
    sudo apt install apache2 -y
    ```
    
2. **Start and Enable Apache:**
    
    ```bash
    sudo systemctl start apache2
    sudo systemctl enable apache2
    ```
    
3. **Verify Apache Installation:**
    
    - Open your browser and visit:
        
        ```
        http://<INSTANCE_PUBLIC_IP>
        ```
        
    - You should see the Apache default "It works!" page.

---

#### **Step 4: Deploy Your HTML/CSS Application**

1. **Upload HTML/CSS Files to the Instance:**
    
    - Use `scp` to transfer files:
        

https://github.com/MuzakkirHossainMinhaz/panda-commerce.git

		```bash
        scp -i "your-key-file.pem" path/to/your/files/* ubuntu@<INSTANCE_PUBLIC_IP>:/tmp
        ```
        
2. **Move Files to Apache’s Web Directory:**
    
    - The default web directory for Apache on Ubuntu is `/var/www/html/`. Move your files there:
        
        ```bash
        sudo mv /tmp/* /var/www/html/
        ```
        
3. **Set Proper Permissions:**
    
    - Ensure Apache has the necessary permissions to serve the files:
        
        ```bash
        sudo chmod -R 755 /var/www/html/
        ```
        
4. **Test Your Application:**
    
    - Visit your public IP in the browser:
        
        ```
        http://<INSTANCE_PUBLIC_IP>
        ```
        
    - Your HTML/CSS application should now load.

---

#### **Step 5: Customize Apache Configuration (Optional)**

1. **Enable Directory Indexing (Optional):**
    
    - If your application contains subdirectories and you want directory browsing, enable indexing:
        
        ```bash
        sudo a2enmod autoindex
        sudo systemctl restart apache2
        ```
        
2. **Custom Virtual Host Configuration (Optional):**
    
    - If you plan to host multiple applications or use a custom domain, create a new virtual host file:
        
        ```bash
        sudo nano /etc/apache2/sites-available/your-app.conf
        ```
        
    - Add the following configuration:
        
        ```apache
        <VirtualHost *:80>
            ServerAdmin webmaster@localhost
            DocumentRoot /var/www/html
            ErrorLog ${APACHE_LOG_DIR}/error.log
            CustomLog ${APACHE_LOG_DIR}/access.log combined
        </VirtualHost>
        ```
        
    - Enable the new configuration:
        
        ```bash
        sudo a2ensite your-app.conf
        sudo systemctl reload apache2
        ```
        

---

#### **Step 6: Secure and Maintain the Instance**

1. **Limit SSH Access:**
    
    - Edit the security group to allow SSH only from your IP address.
2. **Apply System Updates Regularly:**
    
    ```bash
    sudo apt update && sudo apt upgrade -y
    ```
    
3. **Monitor Logs for Errors:**
    
    - Access logs: `/var/log/apache2/access.log`
    - Error logs: `/var/log/apache2/error.log`

---

#### **Step 7: Clean Up (Optional)**

- If the lab is for practice, terminate the EC2 instance to avoid unnecessary charges:
    - Go to the EC2 dashboard, select the instance, and click **Terminate**.
