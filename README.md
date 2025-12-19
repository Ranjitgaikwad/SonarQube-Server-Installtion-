# SonarQube-Server-Installtion-

# SonarQube Community Edition Installation Guide

# (Amazon Linux / RHEL / CentOS)

📌 Important Notes

# Set passwords for all required users:

  sudo passwd root
  
  sudo passwd ec2-user
  
  sudo passwd sonarqube

 SonarQube Default Port: 9000

# Default Credentials:

  Username: admin

  Password: admin

 Minimum EC2 Requirement: t2.medium (2 GB RAM)

🧰 Prerequisites

# Amazon Linux 2 / RHEL-based system

 Root or sudo privileges

 Java 17 (mandatory)

🔹 Step 1: Update System Packages

 This ensures all existing packages are up to date and avoids dependency issues.

  sudo yum update -y

🔹 Step 2: Install Java 17 (Amazon Corretto)

 SonarQube 10.x requires Java 17 to run.

  sudo yum install java-17-amazon-corretto-devel -y

 Verify Java Installation

 Confirms that Java is installed correctly.

  java -version

🔹 Step 3: Create a Dedicated SonarQube User

  SonarQube must run as a non-root user for security reasons.

  sudo useradd sonarqube


Set password for SonarQube user.

  sudo passwd sonarqube

🔹 Step 4: Grant Sudo Access to SonarQube User

 Allows SonarQube user to execute administrative commands if required.

  sudo usermod -aG wheel sonarqube


 OR using visudo (recommended):

 Safely edit sudo permissions.

  sudo visudo


Add the following line:

  sonarqube ALL=(ALL) NOPASSWD:ALL

🔹 Step 5: Configure System Limits (MANDATORY)

 SonarQube requires higher file descriptor and process limits.

Edit limits configuration file

  sudo vi /etc/security/limits.conf


 Add at the bottom:

  sonarqube   -   nofile   65536
  sonarqube   -   nproc    4096

🔹 Step 6: Configure Virtual Memory (MANDATORY)

 Elasticsearch (used by SonarQube) needs higher virtual memory mapping.

 Edit sysctl configuration
  sudo vi /etc/sysctl.conf


 Add:

  vm.max_map_count=262144


 Apply kernel parameter changes immediately.

  sudo sysctl -p


Verify the value.

  sysctl vm.max_map_count

🔹 Step 7: Download SonarQube Community Edition

Downloads the official SonarQube Community Edition package.

  wget https://binaries.sonarsource.com/Distribution/sonarqube/sonarqube-10.1.0.73491.zip

🔹 Step 8: Move SonarQube Package to /opt Directory

  /opt is the standard directory for optional third-party software.

  sudo mv sonarqube-10.1.0.73491.zip /opt/
  
  cd /opt

🔹 Step 9: Extract SonarQube Archive

Unzips the SonarQube installation files.

  sudo unzip sonarqube-10.1.0.73491.zip

🔹 Step 10: Change Ownership of SonarQube Directory

 Ensures SonarQube user has full access to its files.

  sudo chown -R sonarqube:sonarqube sonarqube-10.1.0.73491

🔹 Step 11: Switch to SonarQube User

SonarQube must be started using the dedicated user.

su - sonarqube

🔹 Step 12: Start SonarQube Server

 Starts SonarQube in background mode.

  cd /opt/sonarqube-10.1.0.73491/bin/linux-x86-64/
  
  ./sonar.sh start

🔹 Step 13: Check SonarQube Status

 Confirms whether SonarQube is running successfully.

  ./sonar.sh status


 Expected output:

  SonarQube is running

🔹 Step 14: Access SonarQube Web Interface

 Open SonarQube dashboard in a browser.

 http://<EC2-PUBLIC-IP>:9000


Login details:

Username: admin

Password: admin

 You will be asked to change the password after first login.

🔹 Step 15: Open Port 9000 in AWS Security Group

Allows external access to SonarQube UI.

AWS EC2 → Security Groups:

Type: Custom TCP

Port: 9000

Source: Your IP (recommended)

🔹 Step 16: Stop or Restart SonarQube (If Required)

Stops the SonarQube service.

  ./sonar.sh stop


Restarts SonarQube service.

  ./sonar.sh restart

📄 Logs & Troubleshooting

check SonarQube logs if service fails.

  tail -f /opt/sonarqube-10.1.0.73491/logs/sonar.log

✅ Installation Completed Successfully 🎉

SonarQube Community Edition is now installed and ready to use.
