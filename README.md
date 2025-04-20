# Feedback-System-Web-App
Flask + MySQL on AWS EC2 (Docker + Gunicorn Deployment)


---

# 🔧 Flask + MySQL on AWS EC2 (Docker + Gunicorn Deployment)

This project demonstrates a **Feedback System Web App** using **Flask + MySQL**, containerized with **Docker**, hosted on an **AWS EC2** instance, and served using **Gunicorn**.

---

## ✅ Project Setup in 5 Phases:

---

## 🟢 **Phase 1: Launching EC2 Instance and Connecting via SSH**

### Steps:
- Launch an **EC2 instance** (Amazon Linux 2 or Ubuntu).
- Choose appropriate security group rules (allow SSH, HTTP, Custom TCP 5000 and 8000 from anywhere).
- Generate a key pair and download the `.pem` file.

### SSH into EC2:
```bash
chmod 400 your-key.pem
ssh -i "your-key.pem" ec2-user@<your-ec2-public-ip>
```

---

## 🟠 **Phase 2: Installing Dependencies After SSH**

### 🔧 System Updates:
```bash
sudo yum update -y      # For Amazon Linux
sudo apt update -y      # For Ubuntu
```

### 🔧 Install Python, pip, Docker:
```bash
sudo yum install python3 -y
sudo yum install docker -y         # (Ubuntu: sudo apt install docker.io -y)
sudo service docker start          # (Ubuntu: sudo systemctl start docker)
sudo usermod -aG docker ec2-user
```

### 🔧 Install Flask & MySQL client:
```bash
pip3 install Flask
pip3 install PyMySQL
pip3 install cryptography
```

> **Reboot or re-login to apply Docker group changes.**

---

## 🔵 **Phase 3: Set Up MySQL with Docker**

### 🔽 Pull and Run MySQL Docker Container:
```bash
docker pull mysql:8.0

docker run --name mysql-container \
  -e MYSQL_ROOT_PASSWORD=my-secret-pw \
  -e MYSQL_DATABASE=feedback_db \
  -p 3306:3306 \
  -d mysql:8.0
```

### ⚠️ In case of errors:
```bash
docker stop mysql-container
docker rm mysql-container
# Re-run the above `docker run` command
```

### ✅ Verify Running Container:
```bash
docker ps
```

### 🔐 Connect to MySQL inside Container:
```bash
docker exec -it mysql-container mysql -u root -p
```
> Enter password: `my-secret-pw`

### Inside MySQL Prompt (optional table creation):
```sql
USE feedback_db;
CREATE TABLE feedback (id INT AUTO_INCREMENT PRIMARY KEY, comment TEXT);
exit;
```

---

## 🟣 **Phase 4: Create and Run Flask Application**

### 📝 Create `app.py`:
```bash
nano app.py
```

Paste the following code:
```python
from flask import Flask
import pymysql

app = Flask(__name__)

@app.route('/')
def index():
    connection = pymysql.connect(
        host='localhost',
        user='root',
        password='my-secret-pw',
        database='feedback_db'
    )
    cursor = connection.cursor()
    cursor.execute("SELECT DATABASE();")
    result = cursor.fetchone()
    cursor.close()
    connection.close()
    return f"Hello, Flask is connected to MySQL! Connected to database: {result[0]}"

if __name__ == '__main__':
    app.run(host='0.0.0.0', port=5000)
```

### ▶️ Run Flask App:
```bash
python3 app.py
```

Visit in browser:  
```
http://<your-ec2-public-ip>:5000/
```

You should see:  
`Hello, Flask is connected to MySQL! Connected to database: feedback_db`

### ✅ Test using curl (on another terminal):
```bash
curl http://<your-ec2-public-ip>:5000/
```

---

## 🔴 **Phase 5: Run Using Gunicorn on Port 8000**

### 🧩 Install Gunicorn:
```bash
pip3 install gunicorn
```

### 🔁 Open another terminal and SSH into EC2 again.

### ▶️ Run app using Gunicorn:
```bash
gunicorn --bind 0.0.0.0:8000 app:app
```

### ✅ Update EC2 Security Group:
- Go to EC2 Dashboard → Security Groups → Inbound Rules
- Allow **Port 8000** from **Anywhere (0.0.0.0/0)**

### 🌐 Visit:
```
http://<your-ec2-public-ip>:8000/
```

You should see:
`Hello, Flask is connected to MySQL! Connected to database: feedback_db`



### 📦 Sample `requirements.txt`:
```
Flask
PyMySQL
cryptography
gunicorn
```

---

## ✅ What You Learn from This Project:

- Launching and configuring AWS EC2 instances
- Installing and running Docker + MySQL containers
- Creating and deploying Flask applications
- Connecting Flask with MySQL
- Deploying Python apps using Gunicorn
- Working with security groups and ports

