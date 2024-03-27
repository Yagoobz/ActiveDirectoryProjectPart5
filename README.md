<h2>Active Directory Project Part 5</h2>

<h3>Objectives</h3>
- Use Kali Linux to Perform Brute Force Attack
<br />
- View Telemetry via Splunk
<br />
- Setup and Install Atomic Red Team 
<br />
- Run Tests 
<br />
<br />

On the Kali Linux machine, I configure a static IP address of 192.168.10.250 by navigating to Network Connections and changing the IPv4 settings from DHCP to Manual. Then, I add the IP address, Netmask, Gateway, and DNS servers accordingly.
<br />
<br />
<img src="https://github.com/Yagoobz/ActiveDirectoryProjectPart5/assets/145611184/86b313e5-18e4-4103-8e49-b839da89903b" height="30%" width="70%" alt="Disk Sanitization Steps"/>

After configuring the static IP address on the Kali Linux machine, I proceed to open a terminal and execute the commands to update and upgrade the system. Despite taking a considerable amount of time, the update and upgrade processes are crucial for system stability and security. Moving on to the attack, I utilize a tool called Crowbar by installing it through the terminal with the command "sudo apt-get install -y crowbar." Once Crowbar is installed, I access the "rockyou" wordlist that comes pre-installed with Kali Linux by navigating to the directory "/usr/share/wordlists/" in the terminal. Then, I unzip the "rockyou" file and copy its contents onto a separate file for further use in the attack.
<br />
<br />
<img src="https://github.com/Yagoobz/ActiveDirectoryProjectPart5/assets/145611184/956e7f18-651b-4282-a10c-ac76d13402be" height="30%" width="70%" alt="Disk Sanitization Steps"/>

Before launching the attack, I enable Remote Desktop on the target machine by accessing "Advanced system settings" and navigating to the "Remote" tab where I enable the option for "Allow remote connections to this computer." Subsequently, I add the two users I created for remote access and ensure their names are correctly entered for authentication.
<br />
<br />
<img src="https://github.com/Yagoobz/ActiveDirectoryProjectPart5/assets/145611184/f62c92ea-0202-4299-bf45-43f5eb8c4e1d" height="30%" width="70%" alt="Disk Sanitization Steps"/>

On Kali Linux, I initiate the attack using Crowbar by executing the command "crowbar -b rdp -u gjones -C passwords.txt -s 192.168.10.100/32." Here's a breakdown of the command parameters: "-b" specifies the service (in this case, RDP), "-u" designates the username to be tested (gjones in this instance), "-C" indicates the password list to be used (previously created as passwords.txt), and "-s" specifies the source IP address. Additionally, I use the CIDR notation "/32" to target only the specific IP address. Upon running the command, I successfully obtain an RDP-SUCCESS result with the username and password, showcasing the effectiveness of the attack.
<br />
<br />
<img src="https://github.com/Yagoobz/ActiveDirectoryProjectPart5/assets/145611184/bd79268d-ce6d-4d4c-a507-06d5238e3560" height="30%" width="70%" alt="Disk Sanitization Steps"/>

Let's analyze the telemetry data in Splunk to gain insights into the attack. In the Splunk search tab, I enter the query "index=endpoint gjones" and then focus on the "Event Code." This reveals that there are 41 instances of event ID 4625, indicating failed login attempts to the gjones account. This aligns with expectations, as the "passwords.txt" file used in the Crowbar attack contained 42 passwords, including two correct ones that I deliberately included for the users. By clicking on the event ID 4625, I observe that all events occurred nearly simultaneously, suggesting a brute force activity pattern. This analysis in Splunk provides valuable visibility into the attack attempts and highlights the importance of monitoring and analyzing telemetry data for security purposes.
<br />
<br />
<img src="https://github.com/Yagoobz/ActiveDirectoryProjectPart5/assets/145611184/273c5082-c6dc-4b9e-a010-18cc428bb0ac" height="30%" width="70%" alt="Disk Sanitization Steps"/>

That's a crucial observation. Alongside the 4625 event ID indicating failed login attempts, there's also a 4624 event ID signifying a successful login to the account. By expanding the details of this event, we can gather additional information such as the workstation name from Kali Linux and the corresponding IP address used for the successful login. This provides further context and insight into the attack scenario, allowing for a more comprehensive understanding of the security incident.
<br />
<br />
<img src="..." height="30%" width="70%" alt="Disk Sanitization Steps"/>

Firstly, open PowerShell with administrator privileges and execute the command "Set-ExecutionPolicy Bypass CurrentUser" to bypass the execution policy. Next, install the Atomic Red Team framework, but before doing so, set an exclusion for the entire C drive in Windows Security settings to prevent Microsoft Defender from removing some Atomic Red Team files. Navigate to Windows Security, then Virus & Threat Protection, Manage Settings, Add or Remove Exclusions, and add the C drive. With the exclusion set, proceed to install Atomic Red Team by copying and pasting the commands from a GitHub page, making the process straightforward and efficient.
<br />
<br />
<img src="..." height="30%" width="70%" alt="Disk Sanitization Steps"/>

In the C drive, navigate to the Atomic Red Team folder, which contains a subfolder named "atomics" housing various technique IDs that align with the MITRE ATT&CK framework. To test one of these techniques, refer to the MITRE ATT&CK Matrix for Enterprise website and select a technique code, such as T1136. Among the available codes, choose the first one (e.g., T1136.001). In PowerShell, execute the command "Invoke-AtomicTest T1136.001" to trigger the Atomic Red Team framework to generate telemetry simulating the creation of a local account, providing valuable insights into system security and detection capabilities.
<br />
<br />
<img src="..." height="30%" width="70%" alt="Disk Sanitization Steps"/>

This created a username that I can easily locate in Splunk by performing a targeted search. To find it, I simply enter "index=endpoint NewLocalUser" in the search tab. Boom! There it is, ready for me to explore.
<br />
<br />
<img src="..." height="30%" width="70%" alt="Disk Sanitization Steps"/>
