
# 🧪 Phase 4: Testing Flask & Gunicorn App

This phase focuses on **testing** the Flask app both with the built-in server (`python3 app.py`) and using **Gunicorn** as a production WSGI HTTP server.

---

## ✅ Step 1: Run Flask App

Start the Flask app manually to test whether it's connecting to the MySQL Docker container properly.

```bash
python3 app.py
```

### 🔍 Browser Test

Visit the Flask application using the EC2 instance's public IP and port `5000`:

```text
http://<your-ec2-public-ip>:5000/
```

✅ Output (Expected):
```
Connected to database: feedback_db
```

---

## 🧪 Step 2: Test Using curl (Optional CLI Test)

You can test the endpoint from **another SSH terminal** or locally with curl:

```bash
curl http://<your-ec2-public-ip>:5000/
```

✅ Output:
```text
Connected to database: feedback_db
```

---

## 🧱 Step 3: Install and Run Gunicorn

Install Gunicorn if not already installed:

```bash
pip3 install gunicorn
```

Now run the Flask app using Gunicorn on port `8000`:

```bash
gunicorn --bind 0.0.0.0:8000 app:app
```

### 🔐 Configure EC2 Security Group

➡️ Allow traffic to **port 8000** in the EC2 **Security Group**:

- Go to your EC2 instance
- Click on **Security Groups**
- Edit **Inbound Rules**
- Add rule:  
  - Type: **Custom TCP**
  - Port: `8000`
  - Source: `Anywhere` (or your IP for safety)

---

## 🌐 Step 4: Visit Gunicorn App in Browser

Open this in your browser:

```text
http://<your-ec2-public-ip>:8000/
```

✅ Output:
```text
Connected to database: feedback_db
```

---

## 🧠 Summary

| Tool      | Port | Purpose                 |
|-----------|------|-------------------------|
| Flask     | 5000 | Development testing     |
| Gunicorn  | 8000 | Production testing      |
| MySQL     | 3306 | Database (via Docker)   |



✅ Both Flask and Gunicorn verified! You're now running a cloud-hosted Python app connected to MySQL via Docker.


