Here's a sample `README.md` for your project to help users understand how to use the Oracle-to-PostgreSQL synchronization script.

---

# **Oracle to PostgreSQL Data Sync Script**

This project provides a Python-based solution to synchronize data between an Oracle database and a PostgreSQL database using **SQLAlchemy**. The script supports incremental syncs, handles updates, and allows you to sync multiple tables based on the last modified timestamp.

## **Table of Contents**
- [Features](#features)
- [Prerequisites](#prerequisites)
- [Project Structure](#project-structure)
- [Setup](#setup)
- [Configuration](#configuration)
- [Running the Script](#running-the-script)
- [Scheduling the Sync](#scheduling-the-sync)
- [Logging](#logging)
- [Additional Notes](#additional-notes)

---

## **Features**
- Sync data between Oracle and PostgreSQL databases.
- Handles data updates and can be extended to support deletes.
- Supports multiple tables with batch processing.
- Customizable sync intervals using cron or task scheduler.
- Logs synchronization progress and errors for easy monitoring.

---

## **Prerequisites**
Before using the script, ensure you have the following:
- **Python 3.7+** installed.
- **Oracle Database** access credentials.
- **PostgreSQL Database** access credentials.
- Oracle Instant Client libraries (required by `cx_Oracle`).
- Python packages: `SQLAlchemy`, `cx_Oracle`, `psycopg2-binary`.

---

## **Project Structure**

```
data_sync/
│
├── config.py           # Database and sync configuration
├── models.py           # ORM models for Oracle and PostgreSQL tables
├── sync.py             # Main sync script
├── requirements.txt    # List of required Python packages
└── README.md           # Project documentation
```

---

## **Setup**

### **1. Clone the Repository**

```bash
git clone https://github.com/AbdelhalimBj/oracle-postgres-sync.git
cd oracle-postgres-sync
```

### **2. Install Dependencies**

Install the required Python packages listed in `requirements.txt`:

```bash
pip install -r requirements.txt
```

### **3. Oracle Instant Client**

Ensure you have [Oracle Instant Client](https://cx-oracle.readthedocs.io/en/latest/user_guide/installation.html) installed and properly configured. Set the appropriate environment variables (`LD_LIBRARY_PATH` for Linux/macOS or `PATH` for Windows).

---

## **Configuration**

Edit the `config.py` file to provide the connection details for both Oracle and PostgreSQL databases.

```python
# config.py

ORACLE = {
    'username': 'your_oracle_username',
    'password': 'your_oracle_password',
    'dsn': 'your_oracle_host:port/service_name',
    'encoding': 'UTF-8',
}

POSTGRESQL = {
    'username': 'your_postgres_username',
    'password': 'your_postgres_password',
    'host': 'your_postgres_host',
    'port': 5432,
    'database': 'your_postgres_db',
}

SYNC_CONFIG = {
    'tables': ['Table1', 'Table2'],  # List the tables you want to sync
    'last_sync_table': 'sync_metadata',  # Metadata table to track last sync timestamps
    'batch_size': 1000,  # Number of records to process per batch
}
```

> **Note:**  
> Replace the connection details with your Oracle and PostgreSQL credentials.

---

## **Running the Script**

Once everything is set up, you can run the sync script:

```bash
python sync.py
```

The script will:
- Sync the specified tables in batches.
- Track and update the last synchronization timestamp for incremental syncs.
- Log progress and any issues during the sync process.

---

## **Scheduling the Sync**

To ensure that your PostgreSQL database remains up to date, you can schedule the script to run at regular intervals using cron (Linux/macOS) or Task Scheduler (Windows).

### **a. Using Cron (Linux/macOS)**

1. Open the cron editor:

   ```bash
   crontab -e
   ```

2. Add a cron job to run the script every hour:

   ```cron
   0 * * * * /usr/bin/python3 /path/to/data_sync/sync.py >> /path/to/data_sync/sync.log 2>&1
   ```

### **b. Using Task Scheduler (Windows)**

1. Open Task Scheduler.
2. Create a new task to run the `sync.py` script at your preferred interval.
3. Set the action to run `python.exe` with the script's path as an argument.

---

## **Logging**

The script uses Python’s `logging` module to log sync progress and errors. Logs are saved to `data_sync.log`. You can configure the logging behavior in `sync.py`:

```python
import logging

logging.basicConfig(
    filename='data_sync.log',
    level=logging.INFO,
    format='%(asctime)s %(levelname)s:%(message)s'
)
```

---

## **Additional Notes**

### **Data Types and Compatibility**
Ensure that data types in both Oracle and PostgreSQL databases are compatible. Modify the `models.py` file as needed to define the correct column types and table schemas.

### **Initial Full Sync**
The script performs incremental syncs based on the last modified timestamps. If this is your first time syncing, ensure all data is fully migrated before starting incremental syncs.

### **Batch Processing**
To avoid memory issues, the script processes data in batches. You can adjust the `batch_size` parameter in `config.py` according to your performance requirements.

---

## **Contributing**
If you encounter issues or have suggestions for improvements, feel free to open an issue or submit a pull request.

---

## **License**
This project is licensed under the MIT License.

---

## **Acknowledgments**
- Thanks to the developers of **SQLAlchemy**, **cx_Oracle**, and **psycopg2**, which make this synchronization possible.
