# AWS VPC Public Subnet Lab

## What I built
I built a custom VPC (`10.0.0.0/16`) in the Sydney region with a public subnet, an internet gateway, and a route table, then launched an EC2 instance running NGINX inside it. I connected through Session Manager and opened only HTTP (port 80).

## Architecture
```
Browser -> Internet gateway -> Route table (0.0.0.0/0 -> igw) -> Public subnet -> Security group (port 80) -> EC2 + NGINX
```

## Steps
1. Created the VPC and a subnet (`10.0.1.0/24`)
2. Created an internet gateway and attached it to the VPC
3. Created a route table with `0.0.0.0/0` pointing to the gateway
4. Associated the route table with the subnet and enabled auto-assign public IP
5. Launched an EC2 instance in the subnet with only HTTP allowed

## Troubleshooting
- **Symptom:** the browser showed `ERR_CONNECTION_TIMED_OUT`.
- **Diagnosis:** I checked the instance status and the route and gateway setup. The browser had switched the address to `https://`, and port 443 was closed.
- **Fix:** I typed `http://` before the public IP and the page loaded.

## What I learned
- A subnet is public because of its route to an internet gateway, not because of its name.
- A public server needs a route and a public IP address.
- An attached gateway does nothing until the route table points to it.
- Timeout means blocked before the server, so I check the network path and the protocol.

## Clean-up
I terminated the instance and deleted the VPC and its related resources.
