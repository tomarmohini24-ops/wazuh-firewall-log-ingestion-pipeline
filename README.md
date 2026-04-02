# wazuh-firewall-log-ingestion-pipeline
SOC lab demonstrating firewall log ingestion pipeline using Wazuh with syslog-based log collection.
# Wazuh Firewall Log Ingestion Pipeline

## Overview

This project demonstrates building a firewall log ingestion pipeline using Wazuh in a SOC environment.

## Architecture

Firewall → syslog → Wazuh Agent → Wazuh Server → Dashboard

## Step 1: Syslog Verification

### Objective

Verify that system logging is working correctly.

### Commands Used

```bash
sudo tail -f /var/log/syslog
logger "TEST FROM MOHINI"
```

### Result

Test log successfully appeared in syslog.
## Step 2: Firewall Logging (UFW)

### Objective

Verify firewall logs are generated and captured in syslog.

### Commands Used

```bash
sudo ufw enable
sudo ufw logging high
sudo ufw deny 8080
```

### Result

Firewall logs (UFW AUDIT/BLOCK) appeared in syslog when external traffic was generated.

### Conclusion

Firewall logging is successfully integrated with syslog.


### Conclusion

Syslog is working correctly and ready for firewall log ingestion.
