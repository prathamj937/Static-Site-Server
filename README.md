# Static Site Server Setup on AWS EC2
This project demonstrates how to set up a basic Linux server (Ubuntu) on AWS EC2 to serve a static website using Nginx, including how to deploy the site files securely from a local machine.

Project Overview
Create an EC2 Ubuntu instance on AWS

Install and configure Nginx to serve static files

Upload local static site files to the server

Configure server to serve the site on port 80

Use scp to securely transfer files

Troubleshoot common issues with permissions and AWS security groups

Steps to Setup and Deploy
1. Create EC2 Instance
Logged in to AWS Console and launched an Ubuntu 22.04 LTS instance

Generated and downloaded .pem SSH key file

Configured security group to allow HTTP (port 80) and SSH (port 22) access

2. Connect to the Server
Connected to the server using the web-based EC2 Instance Connect terminal

Verified server was running and updated packages using:

bash
Copy
Edit
sudo apt update && sudo apt upgrade -y
3. Install Nginx
Installed Nginx web server:

bash
Copy
Edit
sudo apt install nginx -y
Verified Nginx service status:

bash
Copy
Edit
sudo systemctl status nginx
4. Prepare Static Site Files Locally
Created a simple static site folder with index.html, CSS, and images on local machine

5. Transfer Files to Server
Initially tried using rsync on Git Bash but encountered rsync: command not found error

Switched to using scp from Git Bash on local machine to copy files:

bash
Copy
Edit
scp -i "/path/to/nginx.pem" -r /path/to/static-site/* ubuntu@<public-ip>:~/site
Used absolute Windows path conversion for .pem key when needed

6. Move Files on Server and Set Permissions
Via EC2 web terminal, moved files to Nginx root directory:

bash
Copy
Edit
sudo rm -rf /var/www/html/*
sudo mv ~/site/* /var/www/html/
sudo chmod -R 755 /var/www/html
Restarted Nginx:

bash
Copy
Edit
sudo systemctl restart nginx
7. Open Firewall Ports
Confirmed HTTP port 80 was open in the EC2 instance’s security group inbound rules

8. Test Website
Opened browser and accessed site via public IP:
http://<public-ip>/

Site loaded successfully!

Challenges & Solutions
Problem: rsync was not available on Windows Git Bash by default
Solution: Used scp instead for file transfer

Problem: Permission denied errors when copying files directly to /var/www/html
Solution: Uploaded files to ~/site/ and then moved them to /var/www/html/ using sudo commands on server

Problem: Site not loading initially
Solution: Opened port 80 in EC2 Security Group inbound rules to allow HTTP traffic

Problem: Confused about which IP to use (private vs public)
Solution: Verified and used the public IPv4 address from AWS EC2 dashboard

How to Deploy Updates
From local machine, run scp command to copy updated files to ~/site/ on server

SSH or use EC2 web terminal to run:

bash
Copy
Edit
sudo rm -rf /var/www/html/*
sudo mv ~/site/* /var/www/html/
sudo systemctl restart nginx
Optional Next Steps
Automate deployment with a shell script (deploy.sh)

Configure custom domain with Route 53 or other DNS provider

Enable HTTPS with Let’s Encrypt SSL certificates

Contact
For questions or help, feel free to reach out!


https://roadmap.sh/projects/static-site-server
