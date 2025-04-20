
## 🖥️ Using Separate Terminals (Important)

To run both the Flask/Gunicorn server **and** access the EC2 instance or Docker, you’ll need to use **multiple terminal sessions**. Here's how to manage them effectively:

---

### 🔁 Terminal 1 – For Gunicorn Server (Port 8000)
Use this terminal **exclusively** to run the production server:
```bash
gunicorn --bind 0.0.0.0:8000 app:app
```
✅ Keep this terminal **open and running** to serve your Flask app at:
```
http://<your-ec2-public-ip>:8000/
```

---

### 🔁 Terminal 2 – For Flask Development Server (Port 5000)
If testing via Flask’s built-in server:
```bash
python3 app.py
```
✅ Access this at:
```
http://<your-ec2-public-ip>:5000/
```
📌 Tip: Don't run both `python3 app.py` and `gunicorn` at the same time **in the same terminal** — use **two separate terminals** instead.

---

### 🐳 Terminal 3 – For Docker & MySQL Container Management
Use this terminal to:
- Check MySQL Docker container status
- Access MySQL shell
- Run Docker commands

Example:
```bash
docker ps
docker exec -it mysql-container mysql -u root -p
```

---

### 🧪 Terminal 4 – For Curl Testing or Logs
Use this terminal to:
- Run test requests using `curl`
- Monitor logs
- Debug issues

Example:
```bash
curl http://localhost:8000/
```

---

## 🧠 Summary: Terminal Use Table

| Terminal | Purpose                          | Sample Commands                                      |
|----------|----------------------------------|------------------------------------------------------|
| T1       | Run Gunicorn server              | `gunicorn --bind 0.0.0.0:8000 app:app`              |
| T2       | Flask dev server (optional)      | `python3 app.py`                                    |
| T3       | Docker & MySQL container access  | `docker exec -it mysql-container mysql -u root -p`  |
| T4       | Curl testing, logs, SSH sessions | `curl`, `top`, `ps`, etc.                           |

