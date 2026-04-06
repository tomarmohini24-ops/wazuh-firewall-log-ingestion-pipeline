# Wazuh Firewall Monitoring and Threat Analysis using UFW

## Overview

This project demonstrates how to build a firewall monitoring pipeline using UFW and Wazuh. It captures blocked network traffic, forwards logs to a SIEM system, and enables analysis through the Wazuh dashboard.

---

## Architecture

```
External Traffic
      ↓
UFW Firewall (Block + Log)
      ↓
/var/log/ufw.log
      ↓
Wazuh Agent
      ↓
Wazuh Manager
      ↓
Filebeat
      ↓
OpenSearch Index (wazuh-archives-*)
      ↓
Wazuh Dashboard
```

---

## Step 1: Firewall Configuration

Install and enable UFW:

```
sudo apt install ufw
sudo ufw enable
sudo ufw logging high
sudo ufw deny 8080
```

---

## Step 2: Generate Traffic

Simulate external access attempts:

```
Test-NetConnection <public-ip> -Port 8080
```

This generates real network traffic to trigger firewall logs.

---

## Step 3: Verify Firewall Logs

Check logs on the agent machine:

```
tail -f /var/log/ufw.log
```

Example:

```
UFW BLOCK IN=ens4 SRC=72.x.x.x DPT=8080
```

---

## Step 4: Wazuh Agent Configuration

Edit configuration:

```
sudo nano /var/ossec/etc/ossec.conf
```

Add:

```
<localfile>
  <log_format>syslog</log_format>
  <location>/var/log/ufw.log</location>
</localfile>
```

Restart agent:

```
sudo systemctl restart wazuh-agent
```

---

## Step 5: Enable Log Indexing

Edit Filebeat configuration:

```
sudo nano /etc/filebeat/filebeat.yml
```

Update:

```
filebeat.modules:
  - module: wazuh
    alerts:
      enabled: true
    archives:
      enabled: true
```

Restart Filebeat:

```
sudo systemctl restart filebeat
```

---

## Step 6: Log Analysis

Query used in dashboard:

```
UFW AND BLOCK
```

---

## Observations

* Multiple external IP addresses attempted connections
* Common target ports:

  * 22 (SSH)
  * 443 (HTTPS)
  * 8080

---

## Security Analysis

The logs indicate automated scanning activity from external sources. These are common reconnaissance attempts against publicly exposed servers. All attempts were successfully blocked by the firewall.

---

## Key Learnings

* Difference between journald and syslog logging
* Log ingestion pipeline from endpoint to SIEM
* Debugging Wazuh agent and Filebeat issues
* Real-world exposure of public servers to scanning activity

---

## Result

Firewall logs were successfully:

* Generated using UFW
* Collected by Wazuh agent
* Forwarded to Wazuh manager
* Indexed in OpenSearch
* Visualized in dashboard

---

## Future Improvements

* Implement detection rules for repeated attempts
* Add alerting for suspicious IP activity
* Integrate geo-location enrichment
* Build dashboards for attack patterns

---
