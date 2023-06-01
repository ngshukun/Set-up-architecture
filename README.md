# Set-up-architecture

In this Assignment, you are tasked to design an architecture on the Swift Commercial Cloud (SCC) so that they can deploy their resources swiftly.

The application consist of the following techstack:
- Frontend: React
- Backend: NodeJS
- Database: PostgreSQL

![Screenshot 2023-05-31 171820](https://github.com/ngshukun/Set-up-architecture/assets/68421074/1733e0d8-4df4-485b-bd72-51d6750ba4b3)


You are required to design an Azure, AWS or GCP architecture to host the
application and explain how it achieves the 5 pillars of Well-Architected Framework.
The 5 Pillars of the Well-Architected Framework are
1) Operational Excellence
2) Security
3) Reliability
4) Performance Efficiency
5) Cost Optimization
The Chief of Air Force (CAF) has personally messaged you reminding you that the
system has to be secure, run smoothly throughout the parade including the
rehearsals and there is no room for downtime.

## Proposed Set Up Architecture

The Proposed Set up Architecture will be as shown below
![Screenshot 2023-06-01 115630](https://github.com/ngshukun/Set-up-architecture/assets/68421074/f6b9ca5f-4f23-4b08-978a-602c6df16ec4)


In this architecture, there are 2 ways to access the web, 
Public access and Admin access.

Public access:
when user first access the website, traffice manager will first direct the traffic to the primary region, where it will first encountered a firewall to accept only port 443 incoming request. 

Upon accept the request, traffic will route to the DMZ network, application gateway will serve as reverse proxy to prevent exposing actual IP address to the public. before reaching the bacckend service, the traffic will go through another firewall that accepts IP from the selected NSG.

Admin access:
Admin will required to log in to azure portal, and connect through bastion/RDP in order to connect to access the application.

## 5 pillar of Well- Architected Framwork

### 1. Operational Excellance 
Usage of Terraform as Infrastructure as code
We are able to spin up all required resources in a short moment. if you need to change any requirement, just edit the yaml script in terraform, and terraform will update the changes. this allow frequent, small changes to be make within short period of time.
Usage of azure monitoring metrics
Azure will provision monitoring chart for us to monitor for any abnormalities, we can also create a playbook to define the actions to take for any situation. for example, if the CPU usage is above 70%, alert willl be trigger, addition vm will be provision or etc..

### 2. Cost Optimization 
In order to keep the total cost of ownership (TCO) low and maintain high availability at a same time, the committee will need to define the peak and off peak period, on which time of the day or period it will had the heaviest traffic, and if there a need to keep the same resource running during silent hours. we can reference to previous years data if application to determine the number of resources required and achieve optimise TCO. As there are 2 set of region, the secondary region will had fewer resource to keep the cost down, in the event of primary region down, secondary region will kicks in and autoscale to desire state at the expense of some time comsumed.

### 3. Security
The environment is secured through design by security approach. Firewall is first installed in few location to prevent unathorised access of the application. the 1st layer of firewall only accepts https (port 443) request, while the 2nd layer of firewall only accept incoming IP from the frontend application. Application gateway serve as reverse proxy prevent the application IP address being exposed to the public. 

### 4. Reliability
To minimise any software or hardware failure, individual tier will had spare VM. Load balencer will distribute traffics to serviceable VM while terraform will ensure the state for these infrastructure will remain the same.There will be secondary DB in the event primary DB went down. A replicate region (secondary region) will also activated if the entire primary region is taken down. DB will be replicate in both region through secure virtual network peering.

### 5. Performance Efficiency
Having a secondary region not only improve the reliabiliy of the application, it will alo improve the latency for user that visit the application within the region. with azure monitoring in place, administrator are able to oversee the performance through the UI available, and respond immediate if anything is abnormal.
