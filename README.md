# 🚀 Static Site Server Setup on AWS EC2 🖥️🌐
Welcome to my project! This shows how I set up a basic Linux server (Ubuntu) on AWS EC2 to serve a static website using Nginx, and how I deployed my site from my local machine. Follow along for the full adventure! 🎉

📋 Project Overview
🌟 Launch an Ubuntu EC2 instance on AWS

🔧 Install & configure Nginx to serve static files

📁 Upload static site files securely

🌍 Serve the site on HTTP port 80

🔐 Use scp to transfer files (since rsync gave me trouble!)

🐛 Troubleshoot permission & firewall issues

🛠️ Step-by-Step Setup & Deployment
1️⃣ Create EC2 Instance
Launched Ubuntu 22.04 LTS instance on AWS ☁️

Generated & downloaded SSH .pem key 🔑

Opened Security Group ports: SSH (22) & HTTP (80) 🚪

2️⃣ Connect to the Server
Connected using AWS EC2 Instance Connect web terminal 💻

Ran system updates:

bash
Copy
Edit
sudo apt update && sudo apt upgrade -y
3️⃣ Install Nginx
Installed Nginx web server:

bash
Copy
Edit
sudo apt install nginx -y
Checked Nginx was running:

bash
Copy
Edit
sudo systemctl status nginx
4️⃣ Prepare Static Site Files Locally
Built a simple site with index.html, CSS, images 🎨🖼️

5️⃣ Transfer Files to Server
Tried rsync on Git Bash but got ❌ command not found error

Switched to scp for file transfer:

bash
Copy
Edit
scp -i "/path/to/nginx.pem" -r /path/to/static-site/* ubuntu@<public-ip>:~/site
Handled Windows path quirks with .pem file 🔄

6️⃣ Move Files & Set Permissions on Server
On server terminal:

bash
Copy
Edit
sudo rm -rf /var/www/html/*
sudo mv ~/site/* /var/www/html/
sudo chmod -R 755 /var/www/html
Restarted Nginx to apply changes:

bash
Copy
Edit
sudo systemctl restart nginx
7️⃣ Open Firewall Ports
Ensured HTTP (port 80) was allowed in EC2 Security Group inbound rules 🔓

8️⃣ Test Website
Opened browser at:
http://<public-ip>/ 🌐

Voila! Site loaded perfectly! 🎉🎉

🐞 Challenges & How I Overcame Them
❌ rsync missing on Git Bash → ✔️ used scp instead

❌ Permission denied uploading directly to /var/www/html → ✔️ uploaded to ~/site/ then moved with sudo

❌ Site not loading → ✔️ opened port 80 in security group

❌ Confused about which IP to use → ✔️ used AWS public IPv4 address

🔄 How to Deploy Updates
Run scp from your local machine again:

bash
Copy
Edit
scp -i "/path/to/nginx.pem" -r /path/to/static-site/* ubuntu@<public-ip>:~/site
SSH into server or use web terminal and run:

bash
Copy
Edit
sudo rm -rf /var/www/html/*
sudo mv ~/site/* /var/www/html/
sudo systemctl restart nginx
🚀 Next Steps to Improve
Write a deploy.sh script for one-command deployment 🖥️💨

Set up a custom domain with Route 53 or other DNS provider 🌐🔗

Add HTTPS with Let’s Encrypt SSL certificates 🔒✨

📬 Contact / Questions?
Feel free to reach out! Let’s build awesome things together. 🤝💡
