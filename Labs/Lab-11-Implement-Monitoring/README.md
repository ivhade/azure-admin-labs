# azure-admin-labs
az-104 lab portfolio: identity, networking, compute, storage, monitoring, governance (scripts, screenshots, cleanup)
# Lab 11 - Implement Monitoring

## Goal 
- Enable end-to-end monitoring for Azure resources using **Azure Monitor**
- Create **log queries** and use them to validate telemetry (activity, metrics and log data)
- Build alerting with **Alert rules**, **Action Groups**, and **Alert processing rules**,
- Trigger a test condition and confirm the alert fires and the notification is received.

## What I did

- Deployed a **Virtual Machine** using a custom template,
- Enabled virtual machine monitoring on **Monitor**,
- Created and configure an **Alert** by creating an **Alert Rule**,
- Configured **Action Group** so I am able to get notified if there are any changes to my infrastruture,
- Tested my configuration by deleting the virtual machine,
- Configured an **Alert Processing Rule**,
- Used **Azure Monitor Log Queries** to count virtual machine **heartbeat**,
- Used the heartbeat table by deploying the **KQL Mode**


## Evidence
- ![Resource Deployed](screenshots/Deployed-Template-RG.png)
- ![Enable Virtual Machine Monitoring](screenshots/Enable-Virtual-Machine-Monitoring.png)
- ![Alert Creation](screenshots/Alert-creation.png)
- ![Alert Rule](screenshots/overview-alert-rule.png)
- ![processing Rule](screenshots/processing-rule.png)
- ![Alert processing rule](screenshots/alert-processing-rule.png)
- ![Alert Fired2](screenshots/alert-fired2.png)
- ![Alert received](screenshots/alert-received.png)
- ![Action Group](screenshots/Action-Group.png)
- ![Heartbeats](screenshots/Monitor-logs-VM-Heartbeats.png)
- ![KQL Mode](screenshots/Monitor-logs-KQL-mode.png)

