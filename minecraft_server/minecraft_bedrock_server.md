# Guide to Managing Your Bedrock Server (Headless Linux)
This guide covers everything you need to know about accessing your running Minecraft Bedrock Dedicated Server (BDS) session, adjusting its configuration, and maintaining optimal performance on your headless Ubuntu mini PC.

## I. Accessing and Managing the Server Console
Because you launched your server within the screen utility, you can disconnect from your SSH session and reconnect later without stopping the server.

### A. Reattaching Your Session
To see the console output and enter commands:

1. Log in to your Mini PC via SSH as the `server_mc` user.

2. Reattach the session:
```
Bash

screen -r bedrock_session
```

### B. Detaching the Session
To leave the server running in the background (and return to your Linux shell):

Press __Ctrl + A__

Then press __D__

### C. Issuing Commands
You can execute in-game commands directly from the server console (you do not use the / prefix here).

| Command   | Usage                            | Description                                                                                           |
|-----------|----------------------------------|-------------------------------------------------------------------------------------------------------|
| `stop`      | `stop`                             | Gracefullt shuts down the server, saving the world, and players. __(Always use this before restarting!)__ |
| `op`        | `op <playername>`                  | Grants a player __Operator__ (admin) privileges.                                                          |
| `deop`      | `deop <playername>`                | Revokes Operator privileges                                                                           |
| `say`       | `say Welcome! Our server is live.` | Broadcasts a message in the in-game chat.                                                             |
| `whitelist` | `whitelist add <playername>`       | Adds a player tot he whitelist (if enabled)                                                           |

## II. Configuring Server Settings (server.properties)
All persistent server behavior is controlled by the server.properties file. Changes require the server to be stopped and restarted.

### A. How to Edit the File
1. __Stop the Server:__ Reattach to the screen session and type stop.

2. __Open the File:__ Use the nano editor.
```
Bash

nano server.properties
```
3. __Make Changes:__ Locate the setting and change the value (see list below).

4. __Save and Exit:__ Press __Ctrl+X__, then __Y__ (to confirm save), and __Enter__.

%. __Restart the Server:__
```
Bash

LD_LIBRARY_PATH=. ./bedrock_server
```
### B. Key Settings Explained
Setting	Default	Purpose and Tips
server-name	Dedicated Server	Your server's displayed name. Change this to something descriptive.
view-distance	32	The number of chunks sent to players. This is the most important performance setting. For a Mini PC, set this to 10 or 12 for stable performance.
max-players	10	The player capacity.
allow-cheats	false	Enables coordinates and command usage for non-Op players. Set to true to allow showing coordinates.
gamemode	survival	Sets the default mode (survival, creative, or adventure).
difficulty	easy	Sets the game difficulty (peaceful, easy, normal, hard).
online-mode	true	Keep this as true for security; it validates players via Xbox Live.

Export to Sheets

💡 III. Maintenance, Backup, and Reliability
A. Performance and Stability Tips
Schedule Restarts: The BDS can suffer from memory leaks over time. Schedule a weekly restart (or even daily, if heavily used) to clear memory and keep performance smooth.

Use Whitelist: Set whitelist=true in server.properties and add only your friends using whitelist add <playername> in the console. This reduces resources used by unauthorized connection attempts.

B. Backup Strategy (Critical!)
Always back up your world when the server is stopped to prevent corruption.

Stop the Server.

Compress the World Folder: The folder is named by your level-name setting.

Bash

tar -czf world_backup_$(date +%F).tar.gz "Bedrock level"
($(date +%F) ensures the backup file has today's date, e.g., world_backup_2025-11-14.tar.gz)

Transfer Off-Site: Copy the .tar.gz file to cloud storage (Google Drive, OneDrive) or an external drive.

C. Improving Reliability (Systemd)
For a dedicated server, you should upgrade from screen to a Systemd Service. This ensures the server automatically starts on boot and restarts after a crash. You can look up tutorials for setting up a Bedrock Systemd service for your Ubuntu version.

🎮 IV. Connecting from Xbox and Nintendo Switch
Since consoles do not allow direct IP input for external servers, you must use a DNS redirect service to access your server.

A. Prerequisites
You have your server's Public IP Address.

Your server is running on UDP Port 19132.

B. Switch/Xbox DNS Setup (Switch Instructions Shown)
Go to the console's System Settings -> Internet -> Internet Settings.

Select your Wi-Fi Network -> Change Settings.

Change DNS Settings from Automatic to Manual.

Enter the following DNS addresses:

Primary DNS: 104.238.130.180 (A common public DNS redirect service)

Secondary DNS: 8.8.8.8 (Google Public DNS)

Save the settings.

C. Connecting in Minecraft
Launch Minecraft on the console.

Go to the Servers tab.

Click on any of the Featured Servers (e.g., The Hive, Mineplex).

Instead of connecting to the commercial server, a Custom Server List screen will load due to the DNS redirect.

Use the "Add Server" option on this custom list.

Input your server details:

Server Address (IP): Your Public IP Address

Port: 19132

You can now connect to your Bedrock server from the console!