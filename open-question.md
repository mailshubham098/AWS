- relation of NACL with ELB , which hits first NACL or ELB
- VPC bounds the main route table with a private subnet and a custom route table with a public subnet -> is this statement correct



A user has setup a VPC with CIDR 20.0.0.0/16. The VPC has a private subnet (20.0.1.0/24) and a public subnet (20.0.0.0/24). The user’s data centre has CIDR of 20.0.54.0/24 and 20.1.0.0/24. If the private subnet wants to communicate with the data centre, what will happen?
- It will allow traffic communication on both the CIDRs of the data centre
- It will not allow traffic with data centre on CIDR 20.1.0.0/24 but allows traffic communication on 20.0.54.0/24
- It will not allow traffic communication on any of the data centre CIDRs
- It will allow traffic with data centre on CIDR 20.1.0.0/24 but does not allow on 20.0.54.0/24 (as the CIDR block would be overlapping) ( correct answer)



- quick way to check if two cidr overlaps
