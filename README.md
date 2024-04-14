# El-Doctor-
* El Doctor - A monitoring tool to rule them all! 
* Module: Scripting Competence: is able to write a custom script to monitor machines Type of Challenge: 
* Consolidation Duration: 3 day 
* Deadline: 12/04/2024 
* Participants: : solo


<font color="red">

# The mission
There exist a plethora of amazing monitoring tools out there, some of which go as far as offering a full blown graphical dashboard collecting metrics on your entire system in a single unified interface, isn't it great?! Well, this challenge will have you throw all those pre-made solution out the window to create your own monitoring script!

</font>


You will have multiple days to make it as useful (collect the data you want or need) and fancy (interactive interface, features, ...) as possible, the goal is for you to be creative and make it your own! As such, we won't give you clear instructions to follow nor specific features to implement. Still, we are no monster so here are some idea to inspire you:

# Make an interactive curses interface (or similar) for your script.
Deploy your script on a machine you manage and use something like cron to execute once an hour.
Collect metrics every hour and store them in an CSV file.
If the state of the machine is critical (not enough ram, ...), notify yourself by mail.
Send yourself a system report once a week.
In order to validate this challenge, you must have a repository containing your script and its documentation along with your script.

* NOTE: As mentioned you should make the script as complex and fancy as you can, use this opportunity to practice both your understanding of monitoring and your scripting capabilities.

- Complementary Resources
- Article: Five ways to send email from the CLI
- Article: Cron Job: A Comprehensive Guide for Beginners
- Final Words
Even though the modern IT ecosystem is full of wonderfully designed tools it's always a good thing to know how it works under the hood and to be able to spin up your own solution when needed. Sometimes a bazooka, no matter how pretty it is, is a bit overkill to hit the target!


Server Monitoring Script
This repository contains a Bash script (monitoring_script.sh) designed to monitor server metrics and store them in a CSV file. 
The script is intended to be run periodically using a scheduler like cron.

* Features:
Hourly Metric Collection: The script collects various server metrics such as CPU usage, memory usage, storage usage, network services, and more.

CSV File Storage: Metrics are appended to a CSV file at regular intervals. This allows for easy tracking and analysis of server performance over time.

Email Notifications: Email notifications are sent for critical conditions such as high CPU usage, memory usage, or storage usage.

Weekly Report: A weekly server monitoring report is emailed every Sunday, providing a summary of server performance throughout the week.

Usage:
Clone Repository: Clone this repository to your server:



This Bash script is designed to monitor various aspects of system performance, such as CPU usage, memory usage, disk usage, network usage, and system uptime. 
It provides valuable insights into the health and status of the system by gathering relevant metrics and displaying them in a human-readable format. 
Additionally, it includes functionality for sending email notifications for critical conditions using Postfix with a Gmail account.

https://youtu.be/UuEx_JwNI2s?si=vW2iCzsTrsDJyHxW
```
#!/bin/bash

# This script monitors server metrics and sends email notifications for critical conditions.

# Define the CSV file path
CSV_FILE="/path/to/metrics.csv"

# Define your email address
EMAIL="your_email@example.com"

# Function to send email notification
send_notification() {
    local subject="$1"
    local message="$2"
    echo "$message" | mail -s "$subject" "$EMAIL"
}

# Date and Time
# Append server monitoring report timestamp to CSV file
echo "## Server Monitoring Report - $(date +"%F %k:%M:%S %Z")" >> "$CSV_FILE"

# OS Information
# Append OS information to CSV file
echo "" >> "$CSV_FILE"
echo "## OS Information" >> "$CSV_FILE"
echo "  Hostname: $(hostname | cut -f 1 -d.)" >> "$CSV_FILE"
echo "  Architecture: $(uname -m)" >> "$CSV_FILE"
echo "  Kernel: $(uname -r)" >> "$CSV_FILE"

# Services
# Append running services to CSV file
echo "" >> "$CSV_FILE"
echo "## Services" >> "$CSV_FILE"
sudo systemctl list-units --type=service | grep "running" | sed -e 's/loaded.*running.*/running/g' -e 's/.service//g' >> "$CSV_FILE"

# Internet Connectivity
# Check internet connectivity and append status to CSV file
echo "" >> "$CSV_FILE"
echo "## Internet Connectivity" >> "$CSV_FILE"
ping -c 1 google.com &> /dev/null && echo "  Status: Connected" || echo "  Status: Disconnected" >> "$CSV_FILE"

# IP Addresses
# Append public and private IP addresses to CSV file
echo "" >> "$CSV_FILE"
echo "## IP Addresses" >> "$CSV_FILE"
echo "  Public IP: $(curl -s ipecho.net/plain;echo)" >> "$CSV_FILE"
echo "  Private IP: $(/sbin/ip -o -4 addr list enp0s8 | awk '{print $4}' | cut -d/ -f1)" >> "$CSV_FILE"

# DNS Server
# Append DNS server to CSV file
echo "" >> "$CSV_FILE"
echo "## DNS Server" >> "$CSV_FILE"
cat /etc/resolv.conf | grep nameserver | awk '{print $2}' >> "$CSV_FILE"

# Network Services
# Append network services to CSV file
echo "" >> "$CSV_FILE"
echo "## Network Services" >> "$CSV_FILE"
sudo ss -tlnp | tail -n+2 | tr -s ' ' | cut -d ' ' -f 1,4,7 | column -ts ' ' >> "$CSV_FILE"

# CPU Usage
# Check CPU usage and append to CSV file
echo "" >> "$CSV_FILE"
echo "## CPU Usage" >> "$CSV_FILE"
sar -P ALL -h 0 | tail -n +3 | tr -s ' ' | tr '[:lower:]' '[:upper:]' | cut -d ' ' -f 2,3,5,8 | sed -e '1 a \ ' -e 's/ALL/\*/g' | column -tes ' ' >> "$CSV_FILE"
cpu_usage=$(sar -P ALL 1 1 | grep -E '^Average:' | awk '{print 100 - $NF}')
echo "CPU Usage: $cpu_usage%" >> "$CSV_FILE"
if [ $cpu_usage -gt 80 ]; then
    send_notification "CPU Usage Alert" "CPU usage is above 80%."
fi

# Memory Usage
# Check memory usage and append to CSV file
echo "" >> "$CSV_FILE"
echo "## Memory Usage" >> "$CSV_FILE"
mem_total=$(grep MemTotal /proc/meminfo | awk '{print $2}')
mem_used=$(grep MemAvailable /proc/meminfo | awk '{print $2}')
mem_percent=$((($mem_total - $mem_used) * 100 / $mem_total))
echo "Memory Usage: $mem_percent%" >> "$CSV_FILE"
if [ $mem_percent -gt 80 ]; then
    send_notification "Memory Usage Alert" "Memory usage is above 80%."
fi

# Storage Usage
# Check storage usage and append to CSV file
echo "" >> "$CSV_FILE"
echo "## Storage Usage" >> "$CSV_FILE"
df -h >> "$CSV_FILE"
storage_usage=$(df --output=pcent / | tail -n 1 | sed 's/%//')
if [ $storage_usage -gt 80 ]; then
    send_notification "Storage Usage Alert" "Storage usage is above 80%."
fi

# Disk I/O
# Append disk I/O to CSV file
echo "" >> "$CSV_FILE"
echo "## Disk I/O" >> "$CSV_FILE"
iostat -d, >> "$CSV_FILE"

# Send weekly report via email
# If it's Sunday, send the weekly server monitoring report via email
if [ $(date +%u) -eq 7 ]; then
    mail -s "Weekly Server Monitoring Report" "$EMAIL" < "$CSV_FILE"
fi

# Schedule execution using cron
# Add cron job to execute this script every hour
echo "0 * * * * /bin/bash /path/to/monitoring_script.sh" >> mycron
crontab mycron
rm mycron


```


Modify Configuration: Update the script (monitoring_script.sh) with your desired configurations such as the path to store the CSV file and your email address.

Make Executable: Make the script executable:
chmod +x monitoring_script.sh
Set Up Cron Job: Add a cron job to execute the script every hour:


crontab -e
Add the following line to the cron table:

cron

0 * * * * /path/to/monitoring_script.sh
Replace /path/to/monitoring_script.sh with the actual path to your Bash script.

Requirements:
Bash shell
cron scheduler
mail command for sending email notifications
- Make the script executable by running: chmod +x MonitoringScript.sh
- Configure Postfix with your Gmail account following the instructions here
- Run the script by executing: ./MonitoringScript.sh
- The script will execute various monitoring functions sequentially and display the results. Email notifications will be sent for critical conditions.
Contribution:
Contributions are welcome! Feel free to submit issues, feature requests, or pull requests to improve the functionality of this script.

License:
This project is licensed under the MIT License.

