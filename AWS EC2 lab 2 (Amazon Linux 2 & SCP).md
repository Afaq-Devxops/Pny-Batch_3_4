### **Prerequisites**

1. **AWS Account:** Ensure you have an active AWS account.
2. **HTML/CSS Application:** Have your application files ready (e.g., `index.html`, `style.css`).
3. **AWS CLI (Optional):** Install the AWS Command Line Interface for easier setup (optional).

---

### **Step 1: Launch an EC2 Instance**

1. **Log in to the AWS Management Console:**
    
    - Navigate to the **EC2 Dashboard**.
2. **Launch an Instance:**
    
    - Click on **Launch Instance**.
    - **Choose an AMI:** Select an Amazon Machine Image (AMI). Choose **Amazon Linux 2** or **Ubuntu Server**.
    - **Instance Type:** Select an instance type like `t2.micro` (free tier eligible).
    - **Configure Security Group:** Allow the following inbound rules:
        - **HTTP (Port 80):** Allow from 0.0.0.0/0.
        - **SSH (Port 22):** Allow from your IP or 0.0.0.0/0 (not recommended for production).
    - **Key Pair:** Create or select an existing key pair for SSH access.
    - **Launch the Instance.**

---

### **Step 2: Connect to Your EC2 Instance**

1. **SSH into the Instance:**
    
    - Use the `.pem` key file:
        
        ```bash
        ssh -i "your-key-file.pem" ec2-user@<INSTANCE_PUBLIC_IP>
        ```
        
2. **Update the Package Manager:**
    
    - Run the following commands to update the system:
        
        ```bash
        sudo yum update -y   # For Amazon Linux
        sudo apt update -y   # For Ubuntu
        ```
        

---

### **Step 3: Install a Web Server**

1. **Install Apache (Amazon Linux/Ubuntu):**
    
    ```bash
    sudo yum install httpd -y   # For Amazon Linux
    sudo apt install apache2 -y # For Ubuntu
    ```
    
2. **Start and Enable the Web Server:**
    
    ```bash
    sudo systemctl start httpd   # For Amazon Linux
    sudo systemctl enable httpd  # For Amazon Linux
    sudo systemctl start apache2 # For Ubuntu
    sudo systemctl enable apache2 # For Ubuntu
    ```
    
3. **Verify Apache Installation:**
    
    - Access the public IP of your EC2 instance in a browser:
        
        ```
        http://<INSTANCE_PUBLIC_IP>
        ```
        
    - You should see the Apache default page.

---

### **Step 4: Deploy Your HTML/CSS Application**

1. **Transfer Files to the EC2 Instance:**
    
    - Use `scp` to upload files:
        
        ```bash
        scp -i "your-key-file.pem" path/to/your/files/* ec2-user@<INSTANCE_PUBLIC_IP>:/tmp
        ```
        
2. **Move Files to Web Directory:**
    
    - For Amazon Linux:
        
        ```bash
        sudo mv /tmp/* /var/www/html/
        ```
        
    - For Ubuntu:
        
        ```bash
        sudo mv /tmp/* /var/www/html/
        ```
        
3. **Verify Deployment:**
    
    - Access your application in the browser:
        
        ```
        http://<INSTANCE_PUBLIC_IP>
        ```
        

---

### **Step 5: Clean Up and Security**

1. **Remove Unused Files:** Ensure unnecessary files are not in `/var/www/html`.
2. **Secure the Instance:**
    - Restrict SSH access to specific IP addresses.
    - Regularly update and patch the system.

---

### **Testing the Lab**

1. Open a browser and navigate to your EC2 instance's public IP.
2. Verify that your HTML/CSS application is displayed correctly.
