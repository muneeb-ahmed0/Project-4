
# Deploy a basic HTML/CSS Website on AWS EC2 Using Nginx


To deploy a basic HTML/CSS website on AWS EC2 using Nginx, follow the steps below:

### Prerequisites:

- An AWS account.
- Basic knowledge of EC2, SSH, Nginx, and HTML/CSS.
- EC2 instance running a Linux distribution (e.g., Amazon Linux, Ubuntu).
- Security group configured for HTTP (port 80) and SSH (port 22).

### Step 1: Launch an EC2 Instance

1. **Log in to AWS Console**:
    
    - Go to the [AWS EC2 Dashboard](https://console.aws.amazon.com/ec2/v2/home).
2. **Launch an EC2 Instance**:
    
    - Choose an **Amazon Machine Image (AMI)**: Select a Linux-based AMI (e.g., Amazon Linux 2 or Ubuntu).
    - **Instance Type**: Choose a basic instance type (e.g., `t2.micro`).
    - **Configure Instance**: Leave the default settings, but ensure you select a **public subnet**.
    - **Add Storage**: Leave as default.
    - **Configure Security Group**: Create a new security group allowing:
        - **SSH (port 22)** from your IP.
        - **HTTP (port 80)** from anywhere (0.0.0.0/0).
    - **Key Pair**: Create and download a key pair (`.pem` file) to access the instance via SSH.
3. **Launch the Instance**:
    
    - Click **Launch** and note the **Public IP Address** of the instance once it’s running.
		![[1-server.png]]



### Step 2: Connect to Your EC2 Instance

1. **SSH into Your EC2 Instance**:
    - Open a terminal on your local machine.
        
    - Use the command below, replacing the path to your `.pem` file and EC2 public IP address:
        
        ```copy
       sudo ssh -i /path/to/your-key.pem ec2-user@<EC2-PUBLIC-IP>
        ```
        
    - If using Ubuntu, replace `ec2-user` with `ubuntu`.
		![[Project 4/2-ssh.png]]

		![[Project 4/3-ssh.png]]

		![[Project 4/4-login.png]]

### Step 3: Install Nginx
sudocd
1. **Install Nginx on EC2**:
    
    - Run the following command to install Nginx (for Amazon Linux 2, use `yum`; for Ubuntu, use `apt`):
        
        - For **Amazon Linux 2**:
            
            ```copy
            sudo yum update -y
            sudo yum install nginx -y
            ```
            
        - For **Ubuntu**:
            
            ```copy
            sudo apt update
            sudo apt install nginx -y
            ```
            ![[Project 4/5-update.png]]
2. **Start Nginx**:
    
    - Start and enable Nginx to run on boot:
        
        - For **Amazon Linux 2**:
            
            ```copy
            sudo systemctl start nginx
            sudo systemctl enable nginx
            ```
            
        - For **Ubuntu**:
            
            ```copy
            sudo systemctl start nginx
            sudo systemctl enable nginx
            ```
            ![[6-start-enable.png]]
3. **Verify Nginx is Running**:
    
    - Open a web browser and enter the EC2 public IP address.
    - You should see the default Nginx welcome page.
	    ![[7-site.png]]

### Step 4: Set Up Your Website

1. **Create Your HTML/CSS Website**:
    
    - On your EC2 instance, create a directory for your website files:
        
        ```copy
        sudo mkdir -p /var/www/html/mywebsite
        ```
        ![[8-mkdir.png]]
2. **Upload HTML/CSS Files**:
    
```copy
	cd /var/www/html/mywebsite
```

	 Create index.html file and paste html code in it.

```copy
	sudo vi index.html
```

```copy
	 <!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>My Simple Website</title>
    <link rel="stylesheet" href="styles.css">
</head>
<body>
    <header>
        <h1>Welcome to My Simple Website</h1>
        <nav>
            <ul>
                <li><a href="#">Home</a></li>
                <li><a href="#">About</a></li>
                <li><a href="#">Contact</a></li>
            </ul>
        </nav>
    </header>
    
    <section>
        <h2>About Us</h2>
        <p>This is a simple website built with HTML and CSS.</p>
    </section>
    
    <footer>
        <p>&copy; 2024 My Simple Website</p>
    </footer>
</body>
</html>

```

	 Now create styles.css file and paste CSS code in it.

	

```copy
/* Basic reset */
* {
    margin: 0;
    padding: 0;
    box-sizing: border-box;
}

/* Body styling */
body {
    font-family: Arial, sans-serif;
    background-color: #f4f4f4;
    color: #333;
    line-height: 1.6;
}

/* Header styling */
header {
    background-color: #333;
    color: #fff;
    padding: 20px;
    text-align: center;
}

header h1 {
    font-size: 2.5em;
}

nav ul {
    list-style: none;
    margin-top: 10px;
}

nav ul li {
    display: inline;
    margin-right: 15px;
}

nav ul li a {
    color: #fff;
    text-decoration: none;
    font-size: 1.2em;
}

nav ul li a:hover {
    text-decoration: underline;
}

/* Section styling */
section {
    padding: 20px;
    background-color: #fff;
    margin: 20px auto;
    width: 80%;
    max-width: 800px;
}

section h2 {
    font-size: 2em;
    margin-bottom: 10px;
}

section p {
    font-size: 1.2em;
}

/* Footer styling */
footer {
    text-align: center;
    padding: 10px;
    background-color: #333;
    color: #fff;
    position: fixed;
    width: 100%;
    bottom: 0;
}

```


        
3. **Update Nginx Configuration**:
    
    - Edit the Nginx default server block to point to your new website directory:
        
        ```copy
        sudo nano /etc/nginx/sites-avaliable
        ```
        ![[10-vi-default.png]]
    - Find the following block and modify it to reflect your website directory:
        
        ```nginx
        server {
            listen       80;
            server_name  localhost;
        
            location / {
                root   /var/www/html/mywebsite;
                index  index.html;
            }
        
            # Other configuration (log files, etc.)
        }
        ```
        ![[11-location.png]]
4. **Test Nginx Configuration**:
    
    - Before restarting Nginx, make sure the configuration file is valid:
        
        ```copy
        sudo nginx -t
        ```
        ![[12-t.png]]
    - If everything is okay, restart Nginx:
        
        ```copy
        sudo systemctl restart nginx
        ```
        ![[13-restart.png]]

### Step 5: Access Your Website

- Open a web browser and enter your EC2 instance's public IP address:
    
    ```text
    http://<EC2-PUBLIC-IP>
    ```
    
- You should now see your HTML/CSS website.
    ![[14website.png]]

### Step 6: (Optional) Set Up a Domain Name

To make your site more professional, you can configure a domain name to point to your EC2 instance. This involves:

1. Purchasing a domain name from a registrar (e.g., GoDaddy, Route 53).
2. Updating the DNS records to point to the EC2 instance's public IP address.

### Step 7: Secure Your Website with HTTPS (Optional)

To enable HTTPS, you can set up SSL certificates using **Let's Encrypt**:

1. Install Certbot (the tool for obtaining SSL certificates):
    
    - For Amazon Linux 2:
        
        ```copy
        sudo yum install -y certbot
        ```
        
    - For Ubuntu:
        
        ```copy
        sudo apt install certbot python3-certbot-nginx
        ```
        
2. Obtain an SSL certificate:
    
    ```copy
    sudo certbot --nginx -d yourdomain.com
    ```
    
3. Certbot will automatically configure Nginx to use HTTPS and redirect HTTP to HTTPS.
    

### Conclusion

Your basic HTML/CSS website is now deployed on AWS EC2 using Nginx. You can expand the website with more pages, JavaScript, and interactivity as needed. Make sure to monitor your instance, apply security patches, and consider further optimizations for scalability and performance.
