Setting up a Debian 12 Server on DigitalOcean

This tutorial will guide you through the process of setting up a fresh Debian 12 server on DigitalOcean. We will create a new regular user with administrative privileges, configure SSH access, prevent the root user from connecting via SSH, install Nginx, and configure it to serve a sample website.
1. Create a New Regular User

Replace <username> with your desired username.

bash

# Log in as root
ssh root@your_server_ip

# Create a new user with administrative privileges
adduser <username>
usermod -aG sudo <username>

2. Configure SSH Access for the New User

Ensure you have SSH access for the new user.

bash

# Switch to the new user
su - <username>

# Generate SSH key pair (if not already generated)
ssh-keygen -t rsa

# Copy the public key to the authorized_keys file
mkdir ~/.ssh
cat ~/.ssh/id_rsa.pub >> ~/.ssh/authorized_keys
chmod 700 ~/.ssh
chmod 600 ~/.ssh/authorized_keys

3. Prevent Root User from SSH Access

Edit the SSH configuration file to disable root login.

bash

# Edit the SSH configuration file
sudo nano /etc/ssh/sshd_config

Add or modify the following line:

text

PermitRootLogin no

Save and exit the editor, then restart the SSH service.

bash

sudo systemctl restart ssh

4. Install Nginx

Install Nginx using the package manager.

bash

# Update the package list
sudo apt update

# Install Nginx
sudo apt install nginx

5. Configure Nginx to Serve a Sample Website

Create a sample HTML file for testing purposes.

bash

# Create a sample HTML file
echo "<html><head><title>Sample Website</title></head><body><h1>Hello from Nginx!</h1></body></html>" | sudo tee /var/www/html/index.html

Configure Nginx to serve this file.

bash

# Create a new Nginx server block configuration file
sudo nano /etc/nginx/sites-available/default

Replace the content with the following configuration:

nginx

server {
    listen 80 default_server;
    listen [::]:80 default_server;

    root /var/www/html;
    index index.html;

    server_name _;

    location / {
        try_files $uri $uri/ =404;
    }
}

Save and exit the editor, then restart Nginx.

bash

sudo systemctl restart nginx

6. Access the Sample Website

Open a web browser and navigate to http://your_server_ip. You should see the sample website served by Nginx.

Congratulations! You have successfully set up a Debian 12 server on DigitalOcean, created a new regular user with administrative privileges, configured SSH access, prevented root user SSH access, installed Nginx, and configured it to serve a sample website.