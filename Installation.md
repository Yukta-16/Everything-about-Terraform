**Step-by-step: Launch EC2, SSH, then install Terraform**

**1) Launch an EC2 instance**
- Sign in to the AWS Management Console → EC2 → Instances → Launch Instances.
- Choose an Ubuntu Server if you prefer
- Choose instance type: t2.micro or t3.micro (free tier eligible if eligible).
- Configure key pair: Create new key pair (or use an existing one). Download the .pem file and keep it safe.
- Configure storage upto 15GiB (free upto 30GiB).
- Click Launch instance.

**2) SSH into your EC2 instance**

On your local machine (macOS/windows) open a terminal and run:

**make sure the key file is only readable**
chmod 400 "terra-server-key.pem"

**Connect to your instance using its Public DNS:**
ssh -i "terra-server-key.pem" ubuntu@<publicDNS>.compute.amazonaws.com

**3) Install Terraform on the EC2 instance**
Ensure that your system is up to date and that you have installed the gnupg and software-properties-common packages. You will use these packages to verify HashiCorp's GPG signature and install HashiCorp's Debian package repository.

$ sudo apt-get update && sudo apt-get install -y gnupg software-properties-common

# Install HashiCorp's GPG key.

$ wget -O- https://apt.releases.hashicorp.com/gpg | \
gpg --dearmor | \
sudo tee /usr/share/keyrings/hashicorp-archive-keyring.gpg > /dev/null

# Verify the GPG key's fingerprint.

$ gpg --no-default-keyring \
--keyring /usr/share/keyrings/hashicorp-archive-keyring.gpg \
--fingerprint

# The gpg command reports the key fingerprint:

/usr/share/keyrings/hashicorp-archive-keyring.gpg
-------------------------------------------------
pub   rsa4096 XXXX-XX-XX [SC]
AAAA AAAA AAAA AAAA
uid         [ unknown] HashiCorp Security (HashiCorp Package Signing) <security+packaging@hashicorp.com>
sub   rsa4096 XXXX-XX-XX [E]

# Add the official HashiCorp repository to your system.

$ echo "deb [arch=$(dpkg --print-architecture) signed-by=/usr/share/keyrings/hashicorp-archive-keyring.gpg] https://apt.releases.hashicorp.com $(grep -oP '(?<=UBUNTU_CODENAME=).*' /etc/os-release || lsb_release -cs) main" | sudo tee /etc/apt/sources.list.d/hashicorp.list

# Update apt to download the package information from the HashiCorp repository.

$ sudo apt update

# Install Terraform from the new repository.

$ sudo apt-get install terraform
