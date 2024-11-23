# **Deploy the Todo List with PHP on EC2**

### **Step 1: Launch an Ubuntu EC2 Instance**

1. **Log in to the AWS Console** and navigate to the EC2 Dashboard.
    
2. **Launch an Instance:**
    
    - **AMI:** Select **Ubuntu Server 20.04 LTS**.
    - **Instance Type:** `t2.micro` (Free Tier Eligible).
    - **Key Pair:** Create or use an existing key pair.
    - **Security Group:**
        - Allow **HTTP (Port 80)** from 0.0.0.0/0.
        - Allow **SSH (Port 22)** from your IP.
3. **Launch the Instance.**
    

---

### **Step 2: Connect to the EC2 Instance**

1. **SSH into the instance:**
    
    ```bash
    ssh -i "your-key-file.pem" ubuntu@<INSTANCE_PUBLIC_IP>
    ```
    
2. **Update the system:**
    
    ```bash
    sudo apt update -y
    sudo apt upgrade -y
    ```
    

---

### **Step 3: Install Apache, PHP, and MySQL**

1. **Install Apache:**
    
    ```bash
    sudo apt install apache2 -y
    ```
    
2. **Install PHP and Required Modules:**
    
    ```bash
    sudo apt install php libapache2-mod-php php-mysql -y
    ```
    
3. **Install MySQL:**
    
    ```bash
    sudo apt install mysql-server -y
    ```
    
4. **Secure MySQL Installation:**
    
    ```bash
    sudo mysql_secure_installation
    ```
    
    - Set a root password and follow the prompts to secure MySQL.

---

### **Step 4: Set Up the Database**

1. **Log into MySQL:**
    
    ```bash
    sudo mysql -u root -p
    ```
    
2. **Create a Database:**
    
    ```sql
    CREATE DATABASE todo;
    EXIT;
    ```
    
3. **Import the `todo.sql` File:**
    
    - Clone the GitHub repository:
        
        ```bash
        sudo apt install git -y
        git clone https://github.com/ali-azgar-rakib/Todo-list-with-php.git
        ```
        
    - Navigate to the repository directory:
        
        ```bash
        cd Todo-list-with-php
        ```
        
    - Import the `todo.sql` file into the `todo` database:
        
        ```bash
        sudo mysql -u root -p todo < todo.sql
        ```
        

---

### **Step 5: Deploy the Application**

1. **Move the Application Files:**
    
    - Move all files from the cloned repository to the Apache web directory:
        
        ```bash
        sudo mv * /var/www/html/
        ```
        
2. **Set Permissions:**
    
    - Ensure Apache has access to the files:
        
        ```bash
        sudo chmod -R 755 /var/www/html/
        ```
        
3. **Restart Apache:**
    
    ```bash
    sudo systemctl restart apache2
    ```
    

---

### **Step 6: Test the Application**

1. Open your browser and navigate to:
    
    ```
    http://<INSTANCE_PUBLIC_IP>
    ```
    
2. **What You Should See:**
    
    - A working To-Do List application with options to:
        - Add tasks.
        - View tasks.
        - Edit tasks.
        - Delete tasks.

---

### **Step 7: Secure and Clean Up**

1. **Restrict SSH Access:**
    
    - Update your EC2 Security Group to allow SSH only from your IP address.
2. **Stop or Terminate the Instance (if not needed).**
    
	