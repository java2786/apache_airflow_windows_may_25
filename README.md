# Apache Airflow 3.0.1 Local Setup Guide using Debian WSL
This step-by-step guide will help your team install and run Apache Airflow 3.0.1 locally using Debian via WSL on Windows. The approach is reliable, clean, and reproducible.

## Local Apache Airflow Server Setup on Debian (WSL)
This step-by-step guide will help your team install and run Apache Airflow 3.0.1 locally using Debian via WSL on Windows. The approach is reliable, clean, and reproducible. The setup includes:

- Using a Python virtual environment
- Installing all necessary dependencies
- Initializing the Airflow metadata database
- Running the Airflow API webserver
- Accessing the Airflow UI locally via http://localhost:8080
- Using the generated admin user credentials

All necessary configurations are made using environment variables and no manual user creation is required.

## Step 1: Install Debian from Microsoft Store
Open Microsoft Store on Windows, search for Debian, and install it.
Launch Debian from the Start Menu once installed.

## Step 2: Update and install Python packages
```bash
sudo apt update -y
sudo apt upgrade -y
sudo apt install python3 python3-pip python3-venv

# If any error occurs:
sudo apt install python3 python3-pip python3-venv --fix-missing
```

## Step 3: Setup Airflow Project Directory and Virtual Environment
```bash
mkdir apache_airflow
cd apache_airflow
python3 -m venv my_env
source my_env/bin/activate
```

## Step 4: Install Apache Airflow and flask-appbuilder
Replace X.Y in the constraint URL with your Python version, e.g., 3.11 or 3.7.
For example, if using Python 3.11, use: constraints-3.0.1/constraints-3.11.txt
```bash
pip install 'apache-airflow==3.0.1' --constraint "https://raw.githubusercontent.com/apache/airflow/constraints-3.0.1/constraints-X.Y.txt"

pip install flask-appbuilder
```

## Step 5: Install Required Build Tools (if needed)
```bash
sudo apt-get install build-essential
```

## Step 6: Set Environment Variables and Initialize Database
```bash

export AIRFLOW_HOME=$(pwd)

export AIRFLOW__DATABASE__SQL_ALCHEMY_CONN="sqlite:///$AIRFLOW_HOME/airflow.db"

airflow db migrate
```


## Step 7: Start Airflow API Webserver
This will start the server at: http://localhost:8080
```bash
airflow api-server -p 8080
```


## Step 8: Get Admin Login Credentials
In the project folder apache_airflow, check the file:

```bash
cat simple_auth_manager_passwords.json.generated
```

#### Example output:

```json
{"admin": "DeTnEvt43nFkqF3b"}
```

## Open your browser, visit and login:
- Use these credentials on the UI login screen at: http://localhost:8080 to get login
- You can now explore DAGs, workflows, and Airflow’s full functionality locally.

# Notes
- The user is auto-created on first run.
- To stop the server, press Ctrl + C
- To restart, just run:
```bash
source my_env/bin/activate && airflow api-server -p 8080
```

# Congratulations!
You have successfully set up Apache Airflow with a local webserver on your Windows machine. You're now ready to start creating and managing your own workflows!
