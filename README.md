File Integrity Monitoring System

The File Integrity Monitoring System (FIMS) is a command-line application built to track and protect important folders by detecting any changes made to them. It allows users to watch one or more directories, check or reset original file data, view activity records, and analyze logs for unusual behavior. The tool also includes a secure login system so that only authorized users can use it. Additionally, it automatically creates a backup of all monitored folders before starting the monitoring process to ensure data safety.

Key Features

Directory Monitoring: Keep an eye on one or multiple folders and detect real-time changes.

Baseline View: Display the current baseline file data saved in the system.

Reset Baseline: Refresh or reset the baseline data for selected directories.

Log Viewer: View detailed logs generated while monitoring.

Log Analysis: Use machine learning to detect unusual patterns or anomalies in log files.

Exclude Files/Folders: Skip specific files or folders from being tracked.

User Login: Only verified users can access and use the tool.

Automatic Backup: Take a backup of monitored directories before monitoring begins.

Database Support: Store file details and baseline information in a MySQL database for faster and more organized tracking.

Installation Steps

1. Clone the repository:

git clone 
cd FIM


2. Create a Python virtual environment and install required packages:

python3 -m venv venv
source venv/bin/activate
pip install -r requirements.txt


3. Set up the MySQL database:

sudo mysql

SELECT user, host FROM mysql.user;

CREATE USER 'fim_user'@'localhost' IDENTIFIED BY 'strong_password';
GRANT ALL PRIVILEGES ON *.* TO 'fim_user'@'localhost' WITH GRANT OPTION;
FLUSH PRIVILEGES;


4. Update your .env file with the database details as shown below.

Configuration

Create a new .env file in the project’s root folder and include the following information:

DB_HOST=localhost
DB_NAME=<your_database_name>
DB_USER=<your_database_user>
DB_PASSWORD=<your_database_password>
DB_POOL_SIZE=32
AUTH_DB_NAME=<your_auth_database_name>  # Database for user login
PEPPER=<your_random_pepper_string>

How to Use

When you start the tool, it will first ask you to log in or register.

New users can sign up by entering a username and password.

Existing users can log in with their saved credentials.
Each session remains active for 15 minutes.

Supported Commands:

--monitor: Begin monitoring one or more directories.

--view-baseline: Show the stored baseline data.

--reset-baseline: Reset the baseline for selected folders.

--view-logs: Display log files created during monitoring.

--analyze-logs: Analyze log files using the ML-based anomaly detector.

--exclude: Skip chosen files or folders from monitoring.

--dir: Mention the directories you want to monitor.

Example Commands

Monitor Folders:

python cli.py --monitor --dir /path/to/dir1 /path/to/dir2


View Baseline Data:

python cli.py --view-baseline


Reset Baseline:

python cli.py --reset-baseline --dir /path/to/dir1 /path/to/dir2


View Logs:

python cli.py --view-logs


Analyze Logs:

python cli.py --analyze-logs


Exclude Files/Folders:

python cli.py --exclude /path/to/exclude

Machine Learning for Log Analysis

The system uses a Machine Learning model (Isolation Forest) to find unusual or suspicious patterns in log data.

To Train the Model:

python src/utils/anomaly_detection.py


To Analyze Logs:

python cli.py --analyze-logs

Project Folder Structure
File-Integrity-Monitor-FIM/
├── cli.py                     # Main command-line program
├── src/
│   ├── FIM/
│   │   ├── FIM.py             # Core monitoring features
│   │   ├── fim_utils.py       # Helper functions
│   ├── Authentication/
│   │   ├── Authentication.py  # User login and verification
│   ├── utils/
│   │   ├── backup.py          # Handles folder backups
│   │   ├── log_parser.py      # Converts logs into structured data
│   │   ├── anomaly_detection.py # Detects unusual log activity
│   │   ├── database.py        # Manages database connections
├── config/
│   ├── logging_config.py      # Logging configuration
├── logs/                      # Folder for storing log files
├── data/models/               # Stores trained ML models
├── Example/                   # Example files for testing
├── requirements.txt           # Required Python libraries
├── .env                       # Environment variables
└── README.md                  # Documentation

Log Files

All logs are saved in the logs/ directory. Each monitored folder gets its own file, named in this format:
FIM_<directory_name>.log

Each log entry contains:

Timestamp

Log level

Details about the detected change

This helps track and investigate all modifications quickly and efficiently.
