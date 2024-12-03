HTML/CSS web page on an **AWS EC2 instance** using **Nginx** with **SSL/TLS** support.

---

## Step 1: **Launch an EC2 Instance**

1. Log in to your AWS Management Console.
2. Navigate to **EC2 Dashboard** > Launch an Instance.
3. Choose an Amazon Linux 2 AMI or Ubuntu as the base image.
4. Select an instance type (e.g., `t2.micro` for free tier).
5. Configure security group:
    - Allow HTTP (port 80), HTTPS (port 443), and SSH (port 22) access.
6. Launch the instance and download the private key `.pem` file.

---

## Step 2: **Connect to the EC2 Instance**

1. Open your terminal.
    
2. Run the following command to connect to your EC2 instance:
    
    ```bash
    ssh -i your-key.pem ec2-user@<EC2-Public-IP>
    ```
    

---

## Step 3: **Install Nginx**

1. Update the system:
    
    ```bash
    sudo yum update -y  # For Amazon Linux
    sudo apt update && sudo apt upgrade -y  # For Ubuntu
    ```
    
2. Install Nginx:
    
    ```bash
    sudo yum install nginx -y  # For Amazon Linux
    sudo apt install nginx -y  # For Ubuntu
    ```
    
3. Start and enable Nginx:
    
    ```bash
    sudo systemctl start nginx
    sudo systemctl enable nginx
    ```
    
4. Verify Nginx installation by visiting `http://<EC2-Public-IP>` in your browser.
    

---

## Step 4: **Deploy the HTML/CSS App**

1. Create a directory to host your website:
    
    ```bash
    sudo mkdir -p /var/www/html
    ```
    
2. Set permissions:
    
    ```bash
    sudo chown -R $USER:$USER /var/www/html
    sudo chmod -R 755 /var/www
    ```
    
3. Create a simple HTML app:
    
    ```bash
    cat <<EOF > /var/www/html/index.html
    <!DOCTYPE html>
    <html lang="en">
    <head>
        <meta charset="UTF-8">
        <meta name="viewport" content="width=device-width, initial-scale=1.0">
        <title>Welcome to Nginx</title>
        <style>
            body { font-family: Arial, sans-serif; text-align: center; margin-top: 50px; }
            h1 { color: #4CAF50; }
            a { text-decoration: none; color: blue; }
        </style>
    </head>
    <body>
        <h1>Welcome to Your Nginx Server</h1>
        <p>This is a simple HTML page deployed on an EC2 instance.</p>
        <a href="https://www.example.com">Public Link Example</a>
    </body>
    </html>
EOF
```
    
4. Test by visiting `http://<EC2-Public-IP>`.
    

---

## Step 5: **Configure Nginx**

1. Edit the Nginx configuration file:
    
    ```bash
    sudo nano /etc/nginx/conf.d/default.conf
    ```
    
2. Add the following content:
    
    ```nginx
    server {
        listen 80;
        server_name <EC2-Public-IP>;
    
        root /var/www/html;
        index index.html;
    
        location / {
            try_files $uri $uri/ =404;
        }
    }
    ```
    
3. Restart Nginx to apply changes:
    
    ```bash
    sudo systemctl restart nginx
    ```
    

---

## Step 6: **Set Up SSL/TLS Using Let's Encrypt**

1. Install **Certbot**:
    
    ```bash
    sudo yum install certbot python3-certbot-nginx -y  # Amazon Linux
    sudo apt install certbot python3-certbot-nginx -y  # Ubuntu
    ```
    
2. Run Certbot to obtain an SSL certificate:
    
    ```bash
    sudo certbot --nginx -d <your-domain-name>
    ```
    
    Follow the prompts to verify ownership of the domain. Certbot will automatically update your Nginx configuration for HTTPS.
    
3. Test the SSL configuration:
    
    ```bash
    sudo nginx -t
    sudo systemctl reload nginx
    ```
    
4. Schedule certificate renewal:
    
    ```bash
    sudo crontab -e
    ```
    
    Add the following line:
    
    ```bash
    0 0 * * * /usr/bin/certbot renew --quiet
    ```
    

---

## Step 7: **Access Your Deployed App**

- Visit `https://<your-domain-name>` or `http://<EC2-Public-IP>` to see your deployed app.
- Replace `<your-domain-name>` with the domain you used during Certbot setup.

---

This setup ensures your web page is secure with HTTPS and publicly accessible. Let me know if you need help with the domain configuration!