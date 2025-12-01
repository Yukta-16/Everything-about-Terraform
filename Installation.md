# **Terraform Installation Guide (Using AWS EC2)**

## **Step-by-step: Launch EC2, SSH, then Install Terraform**

---

## **1) Launch an EC2 Instance**

1. Sign in to the **AWS Management Console** → **EC2** → **Instances** → **Launch Instances**
2. Choose an **Ubuntu Server** AMI
3. Select an instance type: **t2.micro** or **t3.micro** (Free Tier eligible)
4. Create or select a **Key Pair** (`.pem` file).

   * Download the key and store it safely.
5. Configure storage: **15 GiB** (Free tier supports up to 30 GiB)
6. Click **Launch Instance**

---

## **2) SSH into Your EC2 Instance**

Open a terminal on your **MacOS / Windows (Git Bash)** and run:

### **Make the key file readable**

```bash
chmod 400 "your-key-file.pem"
```

### **Connect to the instance using its Public DNS**

```bash
ssh -i "terra-server-key.pem" ubuntu@<publicDNS>.compute.amazonaws.com
```

> Replace `terra-server-key.pem` and `<publicDNS>` with your actual key and instance DNS.

---

## **3) Install Terraform on the EC2 Instance (Ubuntu)**

Ensure your system is up to date and install required packages:

```bash
sudo apt-get update && sudo apt-get install -y gnupg software-properties-common
```

---

### **Install HashiCorp's GPG Key**

```bash
wget -O- https://apt.releases.hashicorp.com/gpg | \
gpg --dearmor | \
sudo tee /usr/share/keyrings/hashicorp-archive-keyring.gpg > /dev/null
```

---

### **Verify the GPG Key Fingerprint**

```bash
gpg --no-default-keyring \
--keyring /usr/share/keyrings/hashicorp-archive-keyring.gpg \
--fingerprint
```

Expected output example:

```
/usr/share/keyrings/hashicorp-archive-keyring.gpg
-------------------------------------------------
pub   rsa4096 XXXX-XX-XX [SC]
AAAA AAAA AAAA AAAA
uid         [ unknown] HashiCorp Security (HashiCorp Package Signing) <security+packaging@hashicorp.com>
sub   rsa4096 XXXX-XX-XX [E]
```

---

### **Add the Official HashiCorp Repository**

```bash
echo "deb [arch=$(dpkg --print-architecture) signed-by=/usr/share/keyrings/hashicorp-archive-keyring.gpg] https://apt.releases.hashicorp.com $(grep -oP '(?<=UBUNTU_CODENAME=).*' /etc/os-release || lsb_release -cs) main" | sudo tee /etc/apt/sources.list.d/hashicorp.list
```

---

### **Update apt to Load HashiCorp Packages**

```bash
sudo apt update
```

---

### **Install Terraform**

```bash
sudo apt-get install terraform
```

Start learning Terraform!!
