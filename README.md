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

<img width="1364" height="1044" alt="Wazuh_Architecture" src="https://github.com/user-attachments/assets/3142789b-ba40-4e69-bc42-8bd8dd918819" />
