# SIEM Project Documentation

This project implements a Secuiry Information and Event Management (SIEM) platform using Wazuh.

The SIEM is used to gather and analyze logs and events from the connected devices/agents. This allows me to monitor security alerts, network intrusions, and unusual network/device activity.

Tools used for the Environment: VirtualBox, Ubuntu Linux, Wazuh

##  SIEM Architecture
### Overview
The architecture consists of three primary components:
- Monitored Endpoints - Systems running the Wazuh agent and generating security telemetry.
- Wazuh Server - Receives and analyzes security events and applies detection rules.
- Wazuh Dashboard - Provides visualization, alert investigation, vulnerability analysis, compliance monitoring, and MITRE ATT&CK analysis.

### Architecture Diagram
![[Wazuh_Architecture 2.png|525]]
