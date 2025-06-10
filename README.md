# # HTTP Traffic Analysis using Wireshark & Python

This project demonstrates how attackers can exploit insecure HTTP traffic to intercept sensitive data using tools like **Wireshark**. Since modern websites use HTTPS, this exercise simulates a vulnerable HTTP environment to study network traffic patterns, form submissions, and common security issues.


![Flask Server](screenshots/Flask%20Server%20Running.png)


## Overview

- Created a **local HTTP server** using Python and Flask to simulate a basic login page.
- Used **Wireshark** to capture and analyze HTTP traffic from browser submissions and scripted attacks.
- Simulated a **public API endpoint** to observe how attackers can target exposed services.
- Developed a **Python login script** to mimic automated client behavior (commonly used in recon and brute force attacks).
- Identified security vulnerabilities such as:
- Credentials transmitted in clear text
- Unauthenticated API access
- Script-based requests (e.g., using `python-requests` User-Agent)

## Objectives

- Understand how HTTP traffic looks on the wire.
- Demonstrate the risks of transmitting sensitive data over HTTP.
- Practice packet analysis from a SOC analyst perspective.
- Simulate potential attacker behavior in a controlled environment.

## Tools & Technologies

- **Python 3**
- **Flask** (for simulating HTTP web services)
- **Wireshark** (for packet capture and analysis)
- **Browser** (to simulate user interactions)
- **Python Requests library** (for automation simulation)

##  Project Structure
http-traffic-analysis/ 
├── app.py                 # Flask web server with login & API endpoint 
├── login_script.py        # Python script to send automated login POST request 
├── requirements.txt       # Flask & requests dependencies 
├── README.md              # Project documentation
├── screenshots/           # Screenshots used in the README
     ├── ![Flask Server](screenshots/Flask%20Server%20Running.png)
     ├── ![Login Page](screenshots/Login%20form%20page%20.png)
     ├── ![Wireshark Capture Form](screenshots/Wireshark%20capture%20showing%20form%20data.png)
     ├── ![Python Script Server](screenshots/Login%20script%20Server.png)
     ├── ![Login Script Execution](screenshots/Python%20script%20execution.png)
     ├── ![Wireshark Capture API JSON](screenshots/Wireshark%20capture%20of%20API%20JSON%20response.png)
     ├── ![Wireshark Capture User-Agent](screenshots/Wireshark%20User-Agent%20Header.png)
     
     
## How It Works

### 1. Start Local Server
![Flask Server](screenshots/Flask%20Server%20Running.png)

Launch the Flask app:
''' bash
python app.py
This starts a local HTTP server at http://127.0.0.1:8080 serving:

a. /: Login form ![Login Form](screenshots/Loginform.png)

b. /login: POST handler for credentials ![POST handler](screenshots/POSThandler.png)

c. /api/data: A mock API returning JSON data ![Mock API](screenshots/MockAPIreturningJSONdata.png)


### 2. Wireshark Traffic Capture

Open Wireshark

Select the Loopback or Wi-Fi interface (depending on setup)

Start capture

Apply filter: http
![Wireshark Capture Form](screenshots/Wireshark%20capture%20showing%20form%20data.png)


### 3. Submit Login Form

Open browser

Navigate to: http://127.0.0.1:8080
![Login Page](screenshots/Login%20form%20page%20.png)

Enter dummy credentials (i entered KeneGod, hackme)


Observe POST request and Form Data in Wireshark

![POST Request](screenshots/postrequest.png)


![Form Data](screenshots/credentials.png)


### 4. Test the API Endpoint

Visit http://127.0.0.1:8080/api/data in your browser
![API Return](screenshots/MockAPIreturningJSONdata.png)

Observe the JSON response in cleartext

Capture and analyze the HTTP response packet in Wireshark
![Wireshark Capture API JSON](screenshots/Wireshark%20capture%20of%20API%20JSON%20response.png)


### 5. Simulate Automated Attack

Run the provided script:

python login_script.py
![Python Login Script](screenshots/Simpleloginscript.png)


This sends a POST login using the Python requests module, allowing you to inspect:

User-Agent string (e.g., python-requests/2.x)
![Wireshark Capture User-Agent](screenshots/Wireshark%20User-Agent%20Header.png)

Suspicious client behavior (non-browser)

Form payloads from automation tools
![Python Script Credentials](screenshots/Pythonscriptlogincredentials.png)

🕵 Security Issues Identified

Issue	Description

🔓 Clear Text Credentials	Username and password visible in HTTP POST
🛑 Lack of TLS (HTTPS)	All data transmitted unencrypted
❌ No API Authentication	/api/data accessible without login
🤖 Scripted Access	Python scripts can bypass UI protections

