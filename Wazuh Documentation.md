##### Setting up
1. Start up Ubuntu VM to use as server to run as central components for Wazuh.
2. From Ubuntu VM:
	1. Open Firefox
	2. Go to Wazuh.com
	3. Navigate to the Documentation tab
	4. Open Quickstart
	5. Under "Installing Wazuh", copy the following command to clipboard
	6. Setting the Firefox window aside, open Terminal
	7. Elevate permissions:
		 **`sudo bash`**
	8. paste command from clipboard onto Terminal and run
		1. When download is complete, a User and password will be given.
	9.  Copy the IP of Ubuntu machine:
		(Run **`ip a`** in a separate terminal to find IP address)
	10. Paste IP address into the search box of a new Firefox window and a Wazuh login should appear.
	11. Copy the corresponding User and password given from the original Terminal into the Wazuh log into dashboard.
3. Setting up new Windows agent
	1. From Ubuntu VM:
		1. Select "Deploy new agent"
		2. Select the package to download and install on your system: 
			1. Select the checkbox under Windows (MIS 32/64 bits)
		3. Server address:
			1. Paste the address of the Wazuh server (The same address that's being displayed in the Firefox search box)
			2. Select "Remember server address", for the sake of future agents
		4. Optional settings:
			1. Assign a unique name for the agent.
				Example: WindowsHost1
		5. Copy the command
	2. From Windows machine:
		1. Open Windows PowerShell as administrator
		2. Paste and run the copied command
		3. Run **`NET START Wazuh`**

##### Troubleshooting
Problem: After selecting "Deploy new agent", and running given command on Windows machine, the Wazuh dashboard would not detect the new agent.

1. Connection test
	1. From Windows machine PowerShell
		**`Ping <Ubuntu-IP>`**
			Result: All 4 packets sent and received.
	Yields: Windows can Ping the Ubuntu VM
2. Check if the manager is listening
	On Ubuntu run:
		**`sudo ss -tulpn | grep 1514`**
			Result: tcp LISTEN 0 128 0.0.0.0:1514
	Yields: The manager is listening
3. Test the port from Windows
	In Windows machine PowerShell: 
		**`Test-NetConnection <Ubuntu-IP> -Port 1514`**
			Result: TcpTestSucceeded : False
		**`Test-NetConnection <Ubuntu-IP> -Port 1515`**
			Result: TcpTestSucceeded : False
	Yields: The Windows agent cannot reach the manager

The Windows machine can reach the VM, but TCP port 1514 and 1515 are no accepting connections

From Ubuntu VM:
	 1. Check the Ubuntu firewall status:
		**`sudo ufw status`**
			Result: Status active
	2. Allow the Wazuh ports:
		**`sudo ufw allow 1514/tcp`**
		**`sudo ufw allow 1515/tcp`**
		**`sudo ufw reload`**
Test again from Windows Powershell:
	**`Test-NetConnection <Ubuntu-IP> -Port 1514`**
		Result: TcpTestSucceeded : True
	**`Test-NetConnection <Ubuntu-IP> -Port 1515`**
		Result: TcpTestSucceeded : True

The Wazuh dashboard now displays an active connection with the Windows agent.

Somme important Commands:
**`sudo nano /var/ossec/etc/ossec.conf`**
**`sudo systemctl restart wazuh-manager`**
**`sudo systemctl status wazuh-manager`**