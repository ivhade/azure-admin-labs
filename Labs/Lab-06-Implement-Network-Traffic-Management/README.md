# azure-admin-labs
az-104 lab portfolio: identity, networking, compute, storage, monitoring, governance (scripts, screenshots, cleanup)
# Lab 06- Implement Network Traffic Management

## Goal
Implement network traffic management for a public web workload by:

- Deploying a small baseline environment from an ARM template (VNet, NSG,VMs).
- Configuring a **public load balancer** to distribute HTTP traffic across multiple VMs.
- configuring an **Application Gateway** to route traffic based on URL paths

## What I did

- Deployed the lab environment via a template.
- created a **public Load Balancer** with frontend IP, backend pool, health probe, and load-balancing rule.
- Verified end-to-end connectivity to the Load Balancer frontend and confirmed traffic distribution across backends.
- Created an **Application GATEWAY** (with required subnet, frontend IP, listener and routing rules).
- Configured backend pools and health probes, then confirmed backends reported healthy.
- Implemented and tested path-based routing to direct requests to the intended backeend targets.
- Performed validation checks.


## Evidence
 - ![Deployed Template of Several Resources](screenshots/Custom-Deployment-Several_Resources.png)
 - ![Deployed Template of Several Resources1](screenshots/Custom-Deployment-Several-Resources1.png)
 - ![Load Balancer](screenshots/Loab-Balancer.png)
 - ![Load Balancer on VM1](screenshots/Testing-LoadBalancer-VM1.png)
 - ![Load Balancer on VM2](screenshots/Testing-LoadBalancer-VM1.png)
 - ![Default-Subnet](screenshots/Adding-Default-Subnet.png)
 - ![Application Gateway](screenshots/Application-Gateway-Frontend.png)
 - ![Application Gateway1](screenshots/Application-Gateway-Backend.png)
 - ![Application gateway Rule](screenshots/Application-Gateway-Rules.png)
 - ![Virtual Machine Running](screenshots/Virtual-Machine-Running.png)
 - ![Virtual Machine Image](screenshots/Image-Virtual-Machine.png)
 - ![Backend Health of Machines](screenshots/Backend-Health.png)
