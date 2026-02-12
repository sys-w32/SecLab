# TSH-001: pfSense Syslog Integration with Wazuh Manager

## Overview
This document details the procedure for troubleshooting and successfully forwarding pfSense system and firewall logs to a Wazuh Manager via standard Syslog (UDP port 514).

## 1. Wazuh Manager Configuration
To allow the Wazuh Manager to receive remote syslog data, the following changes were made to the manager's configuration:

* **Enable Remote Syslog**: The `/var/ossec/etc/ossec.conf` file was updated to include a `<remote>` block.
    * **Connection Type**: `syslog`
    * **Port**: `514`
    * **Protocol**: `udp`
    * **Allowed IPs**: Set to the pfSense interface IP address (e.g., `10.40.0.1`).
* **Service Restart**: The manager was restarted using `systemctl restart wazuh-manager` to apply the new listening port.

## 2. pfSense Remote Logging Settings
Configuration was performed in the pfSense WebGUI under **Status > System Logs > Settings**.

* **Log Message Format**: Set to **BSD (RFC 3164)** for maximum compatibility with default Wazuh decoders.
* **Remote Logging**: Enabled by checking **Send log messages to remote syslog server**.
* **Source Address**: Set to the **WAZUH** interface to ensure the logging daemon binds to the correct internal network.
* **IP Protocol**: Set to **IPv4**.
* **Remote Log Servers**: Configured to **10.40.0.51:514** (Wazuh Manager IP and Port).
* **Remote Syslog Contents**: Enabled **Everything** for initial verification, including System, Firewall, and DNS events.

## 3. pfSense Firewall Rules
To allow the log traffic to leave the pfSense firewall and reach the manager, a specific rule was added to the **WAZUH** interface:

* **Action**: Pass
* **Protocol**: UDP
* **Source**: WAZUH subnets
* **Destination**: 10.40.0.51 (Single host)
* **Destination Port**: 514 (Syslog)

## 4. Verification and Validation
Validation was performed on the Wazuh Manager terminal to ensure packets were arriving correctly:

* **Packet Capture**: `tcpdump -i any udp port 514 -A`
* **Result**: Confirmed raw **filterlog** messages hitting the manager with standard facility/severity headers (e.g., `Facility local0 (16), Severity info (6)`).
