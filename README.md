Networking Assignment

**www.khalidomar1.com**
​

The task for this networking assignment was to purchase my own domain, deploy an NGINX web server on an AWS EC2 instance, and configure DNS so that the website was accessible via my custom domain. This demonstrates practical application of networking concepts including DNS resolution, IP addressing, routing, firewall configuration, and HTTP.
​

**Step 1: Purchase a domain**

Navigate to Cloudflare and sign up for an account.
​

Search for an available domain, ideally one based on your own name so it is easy to identify.
​

Domains typically cost around the 5 to 10 USD range.
​

I purchased khalidomar1.com through the Cloudflare registrar.
​
<img width="940" height="289" alt="image" src="https://github.com/user-attachments/assets/df501313-630f-4a58-ae71-655d3490742a" />

**Step 2: Launch an EC2 instance**

In the AWS Management Console, go to EC2 and select “Launch instance,” then choose Ubuntu.
​
<img width="940" height="283" alt="image" src="https://github.com/user-attachments/assets/841cb57e-b910-432c-bab5-d4089498faaa" />

Choose the t3.micro instance type, which is free-tier eligible.
​
<img width="940" height="169" alt="image" src="https://github.com/user-attachments/assets/980118f6-fdde-4f20-88e9-b35fd4e69dbb" />

Before launching, create a key pair so you can SSH into the instance; choose a key pair name, set the key type to RSA, and select .pem as the private key file format.

<img width="626" height="556" alt="image" src="https://github.com/user-attachments/assets/60aba93a-2d90-4c15-b131-cae876458631" />
​
Add inbound rules in the security group to allow HTTP (port 80) and SSH (port 22) traffic.

<img width="940" height="297" alt="image" src="https://github.com/user-attachments/assets/7280e23d-2b06-44f4-8c54-822c79fa42b7" />

After configuring these settings, launch the instance.

**Step 3: Connect to the EC2 instance via SSH**

From your local machine, connect to the EC2 instance using SSH by replacing the placeholders with your private key path and the EC2 public IP address.
​
Example command:
ssh -i "my-key.pem" ubuntu@ec2-ip-address
​
**Step 4: Install and start NGINX on EC2**

Once connected to the instance, run the following commands in order:
​
sudo apt update

sudo apt install nginx -y

sudo systemctl start nginx

sudo systemctl enable nginx

sudo systemctl status nginx

These commands update package lists, install NGINX, start the service, enable it to start on boot, and check its status.
​
**Step 5: Point your domain to the EC2 IP**

In Cloudflare, go to your domain’s DNS settings and create two A records.
​
Type: A, Name: @ (root domain, e.g. khalidomar1.com), IPv4 address: <EC2_PUBLIC_IP>, TTL: Auto, Proxy status: DNS only (grey cloud).
​
Type: A, Name: www (e.g. www.khalidomar1.com), IPv4 address: <EC2_PUBLIC_IP>, TTL: Auto, Proxy status: DNS only.
​
Save the records so that Cloudflare DNS points your domain to the EC2 public IP address.
​
<img width="940" height="411" alt="image" src="https://github.com/user-attachments/assets/793222ab-5089-463b-8876-98a20b6c03e1" />

**Step 6: Confirm DNS is live**

From your local machine, verify that DNS is correctly resolving to your EC2 public IP.
​
You can run:

nslookup your.domain

dig +short your.domain

If configured correctly, these commands should return the EC2 public IP.

<img width="653" height="239" alt="image" src="https://github.com/user-attachments/assets/d45971e6-ba91-433d-b3a4-af488b0c0ebd" />
​

At this point, visiting your domain (for example, khalidomar1.com) in a browser should show the NGINX default landing page hosted on your EC2 instance.

<img width="940" height="504" alt="image" src="https://github.com/user-attachments/assets/73253d15-e818-4e8e-bbad-765e0adfc476" />

**What I learned**

Through this assignment, I gained a clearer understanding of how domain names, DNS records, and public IP addresses work together to make a website accessible on the internet. I learned how to provision an EC2 instance, configure security groups, and securely access a remote Linux server via SSH. I also became more comfortable installing and managing a web server (NGINX) using basic Linux commands. Finally, I saw how changes in Cloudflare DNS settings propagate and how to verify them using tools like nslookup and dig.
​
**Challenge I faced**

A key challenge I encountered was SSH’ing into the EC2 instance using the private key on my Windows machine. I had saved the .pem file on my local computer, but locating the exact file path and referencing it correctly in the SSH command was difficult on Windows. This issue forced me to pay closer attention to how file paths differ between operating systems and how terminal tools on Windows (such as PowerShell or WSL) handle paths. Once I solved this, I was able to connect successfully, which improved my understanding of SSH authentication and local environment configuration.
