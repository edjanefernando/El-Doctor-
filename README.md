# El-Doctor-
* El Doctor - A monitoring tool to rule them all! 
* Module: Scripting Competence: is able to write a custom script to monitor machines Type of Challenge: 
* Consolidation Duration: 3 day 
* Deadline: 12/04/2024 
* Participants: : solo

# The mission
There exist a plethora of amazing monitoring tools out there, some of which go as far as offering a full blown graphical dashboard collecting metrics on your entire system in a single unified interface, isn't it great?! Well, this challenge will have you throw all those pre-made solution out the window to create your own monitoring script!

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

# Step 1: Decide on Metrics to Monitor
Think about the metrics you want to monitor on your system, such as CPU usage, memory usage, disk space, network traffic, etc.
Consider whether you want to collect additional information, like system uptime or the number of logged-in users.

CPU Usage:

Command: top or htop
Description: Displays real-time information about CPU usage, including a list of processes sorted by their CPU consumption.
Usage: Run the top command in your terminal. Press 1 to see individual CPU cores if you're using top.
Memory Usage:

Command: free -m or htop
Description: Displays the amount of free and used memory in the system.
Usage: Execute the free -m command in your terminal to see memory usage in megabytes.
Disk Space:

Command: df -h
Description: Displays the amount of disk space used and available on mounted file systems.
Usage: Run the df -h command in your terminal.
Network Traffic:

Command: iftop, nload, or vnstat
Description: Provides real-time monitoring of network interfaces, displaying incoming and outgoing traffic rates.
Usage: Install and run iftop, nload, or vnstat in your terminal to monitor network traffic.
System Uptime:
**sudo nload**
![image](https://github.com/edjanefernando/El-Doctor-/assets/141014730/d209ee3e-6c82-4239-aa5d-0d819e585736)


Command: uptime
Description: Displays how long the system has been running.
Usage: Execute the uptime command in your terminal.
Number of Logged-in Users:
**sudo uptime**
![image](https://github.com/edjanefernando/El-Doctor-/assets/141014730/898c7838-f88b-4f9e-b010-1a65ca239ee5)

Command: who or w
Description: Displays information about currently logged-in users.
Usage: Run the who or w command in your terminal.
**sudo who**
![image](https://github.com/edjanefernando/El-Doctor-/assets/141014730/1894def3-671a-4542-a55e-b1cddb4e1992)


# Step 2: Choose a Scripting Language
Decide which scripting language you want to use to create your monitoring script. Common choices include Bash, Python, or Perl.
Choose a language that you're comfortable with or interested in learning.

# Step 3: Create the Monitoring Script
Start by writing the script to collect the desired metrics.
Use system commands to gather information about CPU, memory, disk, network, etc.
Implement logic to process and store the collected data.
Add error handling and logging to make the script robust.

# Step 4: Design an Interactive Interface (Optional)
If you want to make your script interactive, consider using a library like curses in Python to create a text-based interface.
Design the interface to display the collected metrics in a user-friendly manner.
Implement interactive features, such as navigation or menu options.

# Step 5: Set Up Cron Job for Periodic Execution
Once your script is ready, configure a cron job to execute it at regular intervals (e.g., every hour).
Use the crontab -e command to edit your cron jobs and add an entry to schedule the script's execution.


# Step 6: Implement Email Notifications
Add functionality to send email notifications if the system's state is critical (e.g., low memory or disk space).
Utilize command-line tools like mail or sendmail to send emails from your script.
Implement logic to check for critical conditions and trigger email alerts accordingly.


# Step 7: Generate Weekly System Report
Implement a feature to generate a weekly system report containing the collected metrics.
Store the report in a file or format (e.g., CSV) and send it via email or save it to a specific directory.

# Step 8: Document Your Script
Write documentation for your monitoring script, including its purpose, usage instructions, and any dependencies.
Include information on how to set up and configure the script, as well as any customizations or options available.
