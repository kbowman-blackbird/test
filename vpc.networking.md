Overview of network communications in and around the VPC
=======================================================

Application Layer
-----------------

- Hub application
 - instances
  - subnets 10.x.200.x, 10.x.201.x
  - route - NAT devices
  - TCP communications
   - outbound mysql/3306 -> rds mysql instance 
   - outbound http/80 -> internet
   - outbound https/443 -> internet
   - outbound https/443 -> IDMSInternal
   - inbound http/8080 <- from ELB
   - inbound https/8443 <- from ELB
   - inbound ssh/22 <- from bastion
 - ELBs
  - subnets 10.x.208.x, 10.x.209.x - ELBs
  - route - IGW
  - TCP communications
   - inbound http/80 <- from ELB
   - inbound https/443 <- from ELB
   - note - SSL ports need TCP listener to pass-through the SSL authentication


- IDMS External application
 - instances
  - subnets 10.x.220.x, 10.x.221.x
  - route - IGW
  - TCP communications
   - outbound oracle/1521 -> rds oracle instance 
   - inbound http/8080 <- from ELB
   - inbound https/8443 <- from ELB
   - inbound ssh/22 <- from bastion
 - ELBs
  - same subnets
  - TCP communications
   - inbound http/80 <- from internet
   - inbound https/443 <- from internet


- IDMS Internal application
 - instances
  - subnets 10.x.210.x, 10.x.211.x
  - route - local only 
  - TCP communications
   - outbound oracle/1521 -> rds oracle instance 
   - inbound http/8080 <- from ELB
   - inbound https/8443 <- from ELB
   - inbound ssh/22 <- from bastion
 - ELBs
  - same subnets
  - TCP communications
   - inbound http/80 <- from VPC
   - inbound https/443 <- from VPC

Database Layer
-------------

- Hub MySQL RDS instance
 - instances
  - subnets 10.x.100.x, 10.x.101.x
  - route - local only 
  - TCP communications
   - inbound mysql/3306 <- from Hub Application
   - inbound mysql/3306 <- from bastion

- IDMS Oracle RDS instance
 - instances
  - subnets 10.x.210.x, 10.x.211.x (same as IDMS Internal App)
  - route - local only 
  - TCP communications
   - inbound oracle/1521 <- from IDMS Internal Application
   - inbound oracle/1521 <- from IDMS External Application
   - inbound oracle/1521 u- from bastion
