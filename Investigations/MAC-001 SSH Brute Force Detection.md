# MAC-001 SSH Brute Force Detection

## Objective:
Detect repeated failed SSH authentication attempts against a macOS endpoint.

### Attack Source:
Kali Linux VM

### Target:
macOS endpoint

### Telemetry
sshd-session
PAM: authentication error

### MITRE ATT&CK:
T1110 - Brute Force

## Troubleshooting
### Problem: 
SIEM does not detect Brute force SSH attempts

#### Test 1: 
1. On macOS terminal Run:
	``log stream --predicate 'process == "sshd"' --info`` 
2. Now, attempt a couple failed brute force SSH authentication from attack source.
Result: macOS detected SSH initial connection, but not the failed authentication

#### Test 2:
1. Make a failed SSH attempt from attack source
2. Then Run on macOS terminal: 
``sudo log show --last 1m --style compact | grep -Ei 'ssh|auth|fail|password|invalid'

Result: The terminal command outputted strings of text, one of which included:
``DF sshd-session[85733:951626] error: PAM: authentication error for [user] from [ip-addresss]``
Yields: We now know that the macOS endpoint does detect and logs the failed authentication. Now, the event detected just needs to be received by the Manager and Wazuh dashboard.

### New Adjustments
- Within macOS using using the Command line:
	``sudo nano /Library/Ossec/etc/ossec.conf``
- Under `<localfile>` inside `<query type>` following the statements already between the brackets, "``or (process=="sshd-session" and message contains[c] "PAM: authentication error")``" is added
	- Example: ``<query type ="trace,log,activity" level="info"> ([pre-existing statememnts])... or (process=="sshd-session" and message contains[c] "PAM: authentication error")</query>``
- Save ``ossenc.conf`` change and restart Wazuh agent

Reference: https://documentation.wazuh.com/current/user-manual/capabilities/log-data-collection/configuration.html#macos 

### Result
Wazuh dashboard now generates alerts after T1110 Brute Force attacks on the Mac device via ssh. 

## Lessons learned
- Troubleshooting via lots of experimenting helps, as the first test led to a failure that may have led to the conclusion that the macOS agent does not detect the failed authentication attempts.

## Future Improvements
- Apply more detailed rules for events using ``local_rules.xm`` and condense the agent's ``ossec.conf`` as to avoid building towards massive nested logic all in one line.
- Add a detecting threshold of a certain amount of brute force attempts.

