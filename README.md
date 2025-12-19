🚀 SonarQube Community Edition Installation Guide Ubuntu Server (AWS
EC2) 📌 Important Notes 1. Set passwords for all users (root, ubuntu,
sonarqube) Command: sudo passwd \<username\>

2\. SonarQube runs on port: 9000

3\. Default Login: Username: admin Password: admin

🧰 Prerequisites

AWS EC2 instance (Minimum t3.medium -- 2 CPU, 4 GB RAM)

Ubuntu 20.04 / 22.04 LTS

sudo access

Internet connectivity

🟢 Step 1: Update System Packages

Purpose: Updates package indexes and installs latest security patches.

sudo apt update && sudo apt upgrade -y

🟢 Step 2: Install Java 17 (Required for SonarQube)

Purpose: SonarQube requires Java 17 to run.

sudo apt install openjdk-17-jdk -y

✅ Verify Java Installation java -version

🟢 Step 3: Create Dedicated SonarQube User

Purpose: Running SonarQube as non-root improves security.

sudo useradd -m -d /home/sonarqube sonarqube sudo passwd sonarqube

🟢 Step 4: Grant Sudo Access to SonarQube User

Purpose: Allows administrative commands if required.

sudo usermod -aG sudo sonarqube

🟢 Step 5: Install Required Utilities

Purpose: Installs tools needed for downloading and extracting files.

sudo apt install wget unzip -y

🟢 Step 6: Download SonarQube Community Edition

Purpose: Downloads the official SonarQube binary package.

wget
https://binaries.sonarsource.com/Distribution/sonarqube/sonarqube-10.1.0.73491.zip

🟢 Step 7: Move SonarQube to /opt Directory

Purpose: Standard Linux location for third-party applications.

sudo mv sonarqube-10.1.0.73491.zip /opt/ cd /opt

🟢 Step 8: Extract SonarQube Package

Purpose: Unpacks SonarQube binaries.

sudo unzip sonarqube-10.1.0.73491.zip

🟢 Step 9: Set Ownership to SonarQube User

Purpose: Ensures SonarQube has required permissions.

sudo chown -R sonarqube:sonarqube sonarqube-10.1.0.73491

🟢 Step 10: Configure Kernel Parameters (Very Important)

Purpose: SonarQube requires higher virtual memory and file limits.

Edit sysctl config sudo vi /etc/sysctl.conf

Add at the end:

vm.max_map_count=262144 fs.file-max=65536

Apply changes:

sudo sysctl -p

🟢 Step 11: Configure User Limits

Purpose: Prevents startup failures related to open files.

sudo vi /etc/security/limits.conf

Add:

sonarqube - nofile 65536 sonarqube - nproc 4096

🟢 Step 12: Switch to SonarQube User

Purpose: Runs SonarQube under dedicated user.

su - sonarqube

🟢 Step 13: Start SonarQube Server

Purpose: Starts SonarQube application.

cd /opt/sonarqube-10.1.0.73491/bin/linux-x86-64/ ./sonar.sh start

🟢 Step 14: Check SonarQube Status

Purpose: Verifies SonarQube startup.

./sonar.sh status

Expected output:

SonarQube is running

🟢 Step 15: Open Port 9000 (AWS Security Group)

Purpose: Allows browser access to SonarQube UI.

Go to EC2 → Security Groups

Add Inbound Rule:

Type: Custom TCP

Port: 9000

Source: Your IP / 0.0.0.0/0 (testing only)

🟢 Step 16: Access SonarQube Dashboard

Purpose: Login to SonarQube web interface.

http://\<EC2-Public-IP\>:9000

🔐 Default Credentials Username: admin Password: admin

⚠️ You will be prompted to change the password after first login.

✅ SonarQube Installation Completed Successfully 🎉

You now have:

Java 17 installed

SonarQube running on Ubuntu

Ready for CI/CD (Jenkins, GitHub Actions, GitLab)

