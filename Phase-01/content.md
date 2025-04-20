
# 🟢 **Phase 1: Launching EC2 Instance and Connecting via SSH**

This phase is about preparing your **virtual machine (EC2 instance)**, generating SSH access keys, and ensuring you're ready to work in the cloud environment.

---

## ✅ Step 1: Login to AWS Console

1. Visit [https://aws.amazon.com/](https://aws.amazon.com/)
2. Sign in with your credentials.
3. Go to the **EC2 Dashboard** (You can search "EC2" in the search bar).

---

## ✅ Step 2: Launch a New EC2 Instance

Click on **"Launch Instance"**.

### Provide the following:
- **Name:** `flask-mysql-project`
- **AMI (Amazon Machine Image):**  
  Choose **Amazon Linux 2 AMI** *(or Ubuntu 20.04 if you prefer apt over yum)*

- **Instance Type:**  
  Choose **t2.micro** (Free Tier eligible)

- **Key pair (login):**
  - Select **Create a new key pair**
  - Key pair name: `my-key-pair`
  - Key pair type: **RSA**
  - Private key format: `.pem`
  - Click **Create key pair** – it will download a `.pem` file (save this securely!)

- **Network settings (IMPORTANT):**
  - Click **Edit** under Security Group
  - Add inbound rules:
    - **SSH** – Port **22**, Source: Anywhere (0.0.0.0/0)
    - **HTTP** – Port **80**, Source: Anywhere (0.0.0.0/0)
    - **Custom TCP** – Port **5000**, Source: Anywhere (0.0.0.0/0)
    - **Custom TCP** – Port **8000**, Source: Anywhere (0.0.0.0/0)

- **Storage:** Leave default (8 GB)

Click **Launch Instance**

---

## ✅ Step 3: Connect to EC2 Using SSH

Once the instance is running:
1. Go to **Instances → Select your instance**
2. Click **Connect**
3. Copy your **public IPv4 address**, e.g., `54.233.123.45`

### 🧑‍💻 On your local terminal:

**Move to the directory where your `.pem` key is saved.**

Make sure your key has secure permissions:
```bash
chmod 400 my-key-pair.pem
```

Now connect:
```bash
ssh -i "my-key-pair.pem" ec2-user@<your-ec2-public-ip>
```

🔑 Example:
```bash
ssh -i "my-key-pair.pem" ec2-user@54.233.123.45
```

> If you're using Ubuntu AMI, replace `ec2-user` with `ubuntu`

---

## ✅ Step 4: Initial Configuration on EC2

Once connected via SSH:

### Update system packages:
```bash
sudo yum update -y      # For Amazon Linux
```
```bash
sudo apt update -y      # For Ubuntu
```

### Install core tools (optional but helpful):
```bash
sudo yum install nano git -y      # Amazon Linux
```
```bash
sudo apt install nano git -y      # Ubuntu
```

---

## ✅ Step 5: Prepare for Next Phase (Project Setup)

- You're now ready to install Python, Flask, Docker, and MySQL in **Phase 2**.
- Exit the EC2 instance (for now) with:
```bash
exit
```

---

## 📝 Recap of Files and Settings to Track (for GitHub later)

| Item | Description |
|------|-------------|
| EC2 Instance | Amazon Linux 2 (t2.micro) |
| Key Pair | `my-key-pair.pem` (saved locally) |
| Open Ports | 22 (SSH), 80 (HTTP), 5000 & 8000 (Custom TCP) |
| Access | SSH via terminal |

---
