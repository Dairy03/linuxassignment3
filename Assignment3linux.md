Setting up a Debian 12 Server on DigitalOcean

This tutorial will guide you through the process of creating a new regular user with administrative privileges, configuring SSH access, prevent the root user from connecting via SSH, install Nginx, and configure it to serve a sample website.

# 1. Create a New Regular User

Replace ```<username>``` with your desired username.

## Log in as root
```
ssh -i .ssh/ssh-key-name root@your_server_ip
```

## Create a new user with administrative privileges
```
adduser <username>
usermod -aG sudo <username>
```

# Configure SSH Access for the New User

Ensure you have SSH access for the new user.

1. Switch to the new user
```
su - <username>
```


2. Copy the public key to the authorized_keys file

Go into ```.ssh/authorized_keys``` and paste the ssh-key-name.

Then enter the following commands:
```
chmod 700 ~/.ssh
chmod 600 ~/.ssh/authorized_keys
```
# Prevent Root User from SSH Access

Edit the SSH configuration file to disable root login.

```
sudo vim /etc/ssh/sshd_config
```
Add or modify the following line:

```PermitRootLogin no```

Save and exit the editor, then restart the SSH service.

```
sudo systemctl restart ssh
```
# Install Nginx

Install Nginx using the package manager.

Update the package list
```
sudo apt update
```
Install Nginx
```
sudo apt install nginx
```
# Configure Nginx to Serve a Sample Website

create the directory ```my-site``` in ```/var/www``` with ```sudo mkdir my-site```.

create the document ```index.html``` in the ```my-site``` directory with ```sudo vim index.html```.

Copy and paste the following code into the ```index.html``` file:
```
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>2420</title>
    <style>
        body {
            display: flex;
            align-items: center;
            justify-content: center;
            height: 100vh;
            margin: 0;
        }
        h1 {
            text-align: center;
        }
    </style>
</head>
<body>
    <h1>Hello, World</h1>
</body>
</html>
```
save and close the file.    

# Create a new Nginx server block configuration file
Change the directory with ```cd /etc/nginx/sites-available```.
Create the file ```my-site.conf``` in the ```sites-available``` directory with ```sudo vim my-site.conf```

Replace the content with the following configuration:

nginx
```
server {
    listen 80 default_server;
    listen [::]:80 default_server;

    root /var/www/my-site;
    index index.html;

    server_name _;

    location / {
        try_files $uri $uri/ =404;
    }
}
```

Save and exit the editor.

Now enter the command: ```sudo ln-s /etc/nginx/sites-available/my-site.conf /etc/nginx/sites-enabled/```

Run the command ```sudo nginx -t``` to test the nginx configurations.



# Access the Sample Website

Open your machine's command line and type ```curl <your_server_ip>```. You should see the sample website served by Nginx.

Congratulations! You have successfully created a new regular user with administrative privileges, configured SSH access, prevented root user SSH access, installed Nginx, and configured it to serve a sample website.


<!-- for testing: -->
<!-- ssh -i .ssh/do-key root@137.184.85.245
ssh -i .ssh/do-key assignment3@137.184.85.245 -->

https://github.com/Dairy03/linuxassignment3/blob/main/Assignment3linux.md


