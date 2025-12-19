# 🚀 SonarQube Community Edition Installation Guide
### Amazon Linux / RHEL / CentOS

---

## 📌 Important Notes

- Make sure to set passwords for required users:
  ```bash
  sudo passwd root
  sudo passwd ec2-user
  sudo passwd sonarqube
SonarQube Default Port: 9000

Default Credentials

Username: admin

Password: admin

Minimum EC2 Requirement: t2.medium (2 GB RAM)

🧰 Prerequisites
Amazon Linux 2 / RHEL based OS

Root or sudo access

Internet connectivity

Java 17 (mandatory)

🔹 Step 1: Update System Packages
Updates all system packages to latest versions.

bash
Copy code
sudo yum update -y
🔹 Step 2: Install Java 17 (Amazon Corretto)
SonarQube 10.x requires Java 17 to run.

bash
Copy code
sudo yum install java-17-amazon-corretto-devel -y
✅ Verify Java Installation
bash
Copy code
java -version
🔹 Step 3: Create Dedicated SonarQube User
SonarQube must run as a non-root user.

bash
Copy code
sudo useradd sonarqube
sudo passwd sonarqube
🔹 Step 4: Grant Sudo Access to SonarQube User
Allows SonarQube user to execute admin commands.

bash
Copy code
sudo usermod -aG wheel sonarqube
OR edit sudoers file safely:

bash
Copy code
sudo visudo
Add this line:

text
Copy code
sonarqube ALL=(ALL) NOPASSWD:ALL
🔹 Step 5: Configure System Limits (MANDATORY)
Required for SonarQube and Elasticsearch stability.

bash
Copy code
sudo vi /etc/security/limits.conf
Add at the bottom:

text
Copy code
sonarqube   -   nofile   65536
sonarqube   -   nproc    4096
🔹 Step 6: Configure Virtual Memory (MANDATORY)
Elasticsearch requires higher virtual memory limits.

bash
Copy code
sudo vi /etc/sysctl.conf
Add:

text
Copy code
vm.max_map_count=262144
Apply changes:

bash
Copy code
sudo sysctl -p
Verify:

bash
Copy code
sysctl vm.max_map_count
🔹 Step 7: Download SonarQube Community Edition
Downloads official SonarQube package.

bash
Copy code
wget https://binaries.sonarsource.com/Distribution/sonarqube/sonarqube-10.1.0.73491.zip
🔹 Step 8: Move SonarQube Package to /opt Directory
/opt is standard location for third-party software.

bash
Copy code
sudo mv sonarqube-10.1.0.73491.zip /opt/
cd /opt
🔹 Step 9: Extract SonarQube Archive
Unzips SonarQube installation files.

bash
Copy code
sudo unzip sonarqube-10.1.0.73491.zip
🔹 Step 10: Change Ownership
Ensures SonarQube user owns all files.

bash
Copy code
sudo chown -R sonarqube:sonarqube sonarqube-10.1.0.73491
🔹 Step 11: Switch to SonarQube User
bash
Copy code
su - sonarqube
🔹 Step 12: Start SonarQube Server
Starts SonarQube in background mode.

bash
Copy code
cd /opt/sonarqube-10.1.0.73491/bin/linux-x86-64/
./sonar.sh start
🔹 Step 13: Check SonarQube Status
bash
Copy code
./sonar.sh status
Expected Output:

text
Copy code
SonarQube is running
🔹 Step 14: Access SonarQube Dashboard
Open in browser:

text
Copy code
http://<EC2-PUBLIC-IP>:9000
Login details:

Username: admin

Password: admin

🔐 Change password after first login.

🔹 Step 15: Open Port 9000 in AWS Security Group
Required to access SonarQube UI externally.

Type: Custom TCP

Port: 9000

Source: Your IP (recommended)

🔹 Step 16: Stop / Restart SonarQube
bash
Copy code
./sonar.sh stop
./sonar.sh restart
📄 Logs & Troubleshooting
Check logs if SonarQube fails to start.

bash
Copy code
tail -f /opt/sonarqube-10.1.0.73491/logs/sonar.log
✅ Installation Completed Successfully 🎉
SonarQube Community Edition is now installed and running.
