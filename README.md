#### Reaper 
is a high-performance periodic vulnerability scanner powered by Nuclei with Telegram notifications. 
Reaper automates periodic scans of subdomains or single URLs, sending results to Telegram and logging outputs locally when connectivity is lost.
Built for reliability, it ensures seamless operation with automatic updates and robust error handling.

---
## Summray
* [Features](#Features)
* [Installation](#Installation)
* [How worked?](#Usage)


##### Features

###### Periodic Scanning: Runs Nuclei scans on a single URL or a list of subdomains at customizable intervals (10-60 minutes).

###### Telegram Notifications: Sends scan results and hourly reports to a specified Telegram chat for real-time updates.

###### Local Logging: Saves scan results to daily log directories (./logs/YYYY-MM-DD/) when Telegram connectivity is lost.

###### Connection Recovery: Detects past connection issues, notifies via Telegram, and cleans up stale services to prevent conflicts.

###### Automatic Updates: Checks for new versions in the repository and updates the binary in /usr/bin/reaper automatically.

###### Lock Mechanism: Prevents concurrent executions using machine-specific lock files to ensure stability.

###### Flexible Input: Supports scanning a single URL (-u/--url) or a list of subdomains (-l/--list) with Nuclei templates.

###### User-Friendly Interface: Provides a colorful, bold help menu (-h/--help) and version display (--version).

---

##### Installation

###### Clone the repository:
```bash
git clone https://github.com/Nowafen/Reaper.git
cd Reaper
```
```bash
Copy the binary to /usr/bin:
sudo cp reaper /usr/bin/reaper
sudo chmod +x /usr/bin/reaper
```

###### Create and configure the config.yaml file:
```bash
reaper -h
```
###### This will create config.yaml in the current directory (./config.yaml). Edit it to add your Telegram Bot Token and Chat ID:
```bash
nano config.yaml
```
 Example config.yaml:
 ```yaml
telegram_bot_token: "your_actual_bot_token"
telegram_chat_id: your_actual_chat_id
```

Ensure Nuclei is installed:
```bash
go install -v github.com/projectdiscovery/nuclei/v3/cmd/nuclei@latest
```

Test the tool: 
```bash
reaper -h
```

---

##### Usage
###### Reaper runs as a background service using systemd, performing periodic scans and sending results to Telegram. Below are the available flags and usage examples.

<details>
 <summary>Expand full help flags
  </summary>
  
  ```
reaper -h

Reaper - A Periodic Nuclei Scanner with Telegram Notifications

Description:
  Reaper is a service that runs Nuclei scans periodically and sends results to Telegram.
  It supports scanning a single URL or a list of subdomains with customizable intervals.

Usage:
  reaper [flags]

Flags:
  -l, --list <file>
    Path to a file containing a list of subdomains (one per line).
  -u, --url <url>
    A single URL to scan (e.g., http://example.com).
  -t <template>
    Path to the Nuclei template (e.g., cves). Required.
  -time <minutes>
    Interval between scans in minutes (10-60, default: 10).
  -h, --help
    Show this help message.
  --version
    Show the version of Reaper.

Configuration:
  Reaper uses a config file at ./config.yaml
  It must contain a valid Telegram Bot Token and Chat ID.
  If the config file doesn't exist, it will be created in the current directory on first run.

Example:
  reaper -l domains.txt -t cves -time 15
  reaper -u http://example.com -t cves

Notes:
  - Either -l/--list or -u/--url must be provided, but not both.
  - Ensure Nuclei is installed and templates are accessible.
  - Reaper runs as a background service and sends scan results to Telegram.
  - A hourly report is sent to Telegram with the number of scans performed.
  - Logs are saved in ./logs/YYYY-MM-DD/ if Telegram is unavailable.



Examples

Scan a single URL:
reaper --url http://payasafar.com -t http/mnm/self/backup.yaml -time 15


Scan a list of subdomains:
reaper --list domains.txt -t cves -time 20


Check version:
reaper --version

 Output:
The version of reaper is 1.1



How It Works

Configuration: On first run with -h, Reaper creates config.yaml in the current directory. Fill in your Telegram Bot Token and Chat ID.
Version Check: Before running any command (except --version), Reaper checks the local version file. If a newer version is found, it clones the repository to /tmp/reaper-update, updates /usr/bin/reaper, and exits.
Service Creation: When running with -l/--list or -u/--url, Reaper creates a systemd service (e.g., reaper_123456789), runs the first scan, and exits. The service continues scanning at the specified interval.
Scanning: Uses Nuclei to scan the provided URL or subdomain list with the specified template. Results are sent to Telegram.
Logging: If Telegram is unavailable, results are saved in ./logs/YYYY-MM-DD/scan_*.log.
Connection Recovery: If a previous connection loss is detected (via ./logs/state.yaml), Reaper notifies via Telegram, cleans up old services, and exits, prompting a restart.
Hourly Reports: Sends a Telegram message every hour with the number of scans performed.

Notes

Ensure git is installed for automatic updates:sudo apt install git


The version file in the repository root determines the latest version.
Logs are stored in ./logs/YYYY-MM-DD/ relative to the cloned directory.
Use sudo for operations requiring /usr/bin or systemd access.
To stop a service:sudo systemctl stop reaper_123456789
sudo systemctl disable reaper_123456789
sudo rm /etc/systemd/system/reaper_123456789.service
```
</details>





##### Contact 
[***](https://t.me/Tellmejs)
