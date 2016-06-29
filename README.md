# SeeSpot

SeeSpot provides a utility for AWS Spot instance that handles the
health check if used with an AWS ELB and also handles cleanup of the
instance when a termination notice is sent. When the termination
notice is sent there is a 2 minute period before termination this
handles the cleanup.

## Usage

```sh
seespot -health-port=8686 -health-path=/health -app-health="https://localhost:8080/health" -cleanup-task=/path/to/cleanup.sh
```

```sh
$ seespot -help
Usage of ./seespot:
  -app-health string
        Application health check (default "http://127.0.0.1:8080/health")
  -cleanup-task string
        Script to run upon termination
  -health-path string
        Default health path the Load Balancer hits (default "/health")
  -health-port string
        Default health port to use with Load Balancers (default ":8686")
```

This should be run within an upstart or systemd unit file.

## Description

SeeSpot watches for a
[termination notification](https://aws.amazon.com/blogs/aws/new-ec2-spot-instance-termination-notices/)
every 5 seconds and upon notification closes off the health check on
the application and initiates the cleanup tasks. If no termination
notice is found then it continues to run.

This provides the following:

 - Make sure there are not new connections being made while the
   instance is going down.
 - Take care of removing machine from ELBs via the health check.
 - Take care of the Application health check.


## Acksin

This is a project of
[Acksin](https://www.acksin.com/?source=seespot). Acksin provides
tools to make your infrastructure more efficient and cost
effective. Check out our spot instance bidding tool
[ParkingSpot](https://www.acksin.com/parkingspot?source=seespot).

## License

This Source Code Form is subject to the terms of the Mozilla Public
License, v. 2.0. If a copy of the MPL was not distributed with this
file, You can obtain one at http://mozilla.org/MPL/2.0/.
