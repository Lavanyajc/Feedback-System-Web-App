
# ✅ Phase 3: Connecting Flask with MySQL & Testing Application

This phase involves coding the Flask app, installing missing libraries, testing database connection, and verifying the Flask application via both browser and curl.

---

## 🔹 Step 1: Install Required Python Libraries

After SSH into EC2:
```bash
pip3 install flask
pip3 install PyMySQL
```

> These are necessary to run a Flask application and interact with the MySQL database.

If you encounter a missing module error like `cryptography`, fix it using:
```bash
pip3 install cryptography
```

---

## 🔹 Step 2: Create the Flask Application File

Create `app.py`:
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

Save & exit:
- Press `Ctrl + X`, then `Y`, then `Enter`

---

## 🔹 Step 3: Run Flask App

Start the app:
```bash
python3 app.py
```

> Output should confirm that the Flask server is running on port `5000`.

---

## 🔹 Step 4: Test in Browser and with curl

Open browser:

```
http://<your-ec2-public-ip>:5000/
```

> Expected Output:  
`Hello, Flask is connected to MySQL! Connected to database: feedback_db`

✅ Also test using curl in another terminal:
```bash
curl http://<your-ec2-public-ip>:5000/
```

> Output:  
`Hello, Flask is connected to MySQL! Connected to database: feedback_db`

---

## 🧪 Summary of Phase 3:

- ✔ Installed Flask, PyMySQL, and cryptography
- ✔ Wrote a Python Flask app (`app.py`)
- ✔ Connected to Docker-hosted MySQL DB (`feedback_db`)
- ✔ Tested via browser and curl command
