
# 🚀 Phase 2: Docker + MySQL + Flask Setup on EC2

## 📌 Objective:
To set up **MySQL using Docker** and build a **Flask application** that connects to the MySQL database. All tasks are done inside the EC2 instance after SSH.

---

## 🔧 Prerequisites:
✔ You must be **SSH-ed into your EC2 instance**  
✔ Phase 1 (EC2 setup and connection) must be completed

---

## 🐳 Step 1: Install Docker on EC2

```bash
sudo yum update -y
sudo yum install docker -y
sudo systemctl start docker
sudo systemctl enable docker
docker --version
```

---

## 💾 Step 2: Pull & Run MySQL Docker Container

```bash
docker pull mysql:8.0
docker run --name mysql-container -e MYSQL_ROOT_PASSWORD=my-secret-pw -e MYSQL_DATABASE=feedback_db -p 3306:3306 -d mysql:8.0
```

If you get errors like "name already in use" or "port already allocated":

```bash
docker stop mysql-container
docker rm mysql-container
```

Then re-run:

```bash
docker run --name mysql-container -e MYSQL_ROOT_PASSWORD=my-secret-pw -e MYSQL_DATABASE=feedback_db -p 3306:3306 -d mysql:8.0
```

---

## 🔍 Step 3: Verify and Enter MySQL

```bash
docker ps
docker exec -it mysql-container mysql -u root -p
# Enter password: my-secret-pw
```

Inside MySQL:

```sql
SHOW DATABASES;
USE feedback_db;
exit;
```

---

## 🐍 Step 4: Install Flask & PyMySQL in EC2

```bash
pip3 install flask
pip3 install PyMySQL
pip3 install cryptography
```

---

## ✍ Step 5: Create the Flask App (app.py)

```bash
nano app.py
```

Paste this code:

```python
from flask import Flask
import pymysql

app = Flask(__name__)

@app.route('/')
def index():
    # Connect to the MySQL database
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
    
    return f'Hello, Flask is connected to MySQL! Connected to database: {result[0]}'

if __name__ == '__main__':
    app.run(host='0.0.0.0', port=5000)
```

Save it with:  
`Ctrl + X` → Press `Y` → Enter

---

## ▶ Step 6: Run Flask App

```bash
python3 app.py
```

Visit on browser:
```
http://<your-ec2-public-ip>:5000/
```

Expected Output:
```
Hello, Flask is connected to MySQL! Connected to database: feedback_db
```
