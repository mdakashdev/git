softzino@Mds-MacBook-Pro aws % ls
my-project-key.pem
softzino@Mds-MacBook-Pro aws % chmod 400 my-project-key.pem
softzino@Mds-MacBook-Pro aws % ls -l my-project-key.pem
-r--------@ 1 softzino  staff  1678 Sep 23 08:39 my-project-key.pem
softzino@Mds-MacBook-Pro aws % chmod 400 my-project-key.pem
softzino@Mds-MacBook-Pro aws % ls -l my-project-key.pem
-r--------@ 1 softzino  staff  1678 Sep 23 08:39 my-project-key.pem
softzino@Mds-MacBook-Pro aws % ssh -i my-project-key.pem ubuntu@56.10.120.85
ssh: connect to host 56.10.120.85 port 22: Operation timed out
softzino@Mds-MacBook-Pro aws % ssh -i my-project-key.pem ubuntu@56.10.120.85
ssh: connect to host 56.10.120.85 port 22: Operation timed out
softzino@Mds-MacBook-Pro aws % ssh -i my-project-key.pem ubuntu@56.10.120.85
ssh: connect to host 56.10.120.85 port 22: Operation timed out
softzino@Mds-MacBook-Pro aws % curl -4 ifconfig.me
59.153.28.84%                                                                                                   softzino@Mds-MacBook-Pro aws % nc -vz 56.10.120.85 22
nc: connectx to 56.10.120.85 port 22 (tcp) failed: Operation timed out
softzino@Mds-MacBook-Pro aws % ssh -i "my-project-key.pem" ubuntu@ec2-56-10-120-85.ap-southeast-1.compute.amazonaws.com
ssh: connect to host ec2-56-10-120-85.ap-southeast-1.compute.amazonaws.com port 22: Operation timed out
softzino@Mds-MacBook-Pro aws % chmod 400 "my-project-key.pem"
softzino@Mds-MacBook-Pro aws % ssh -i "my-project-key.pem" ubuntu@ec2-56-10-120-85.ap-southeast-1.compute.amazonaws.com
ssh: connect to host ec2-56-10-120-85.ap-southeast-1.compute.amazonaws.com port 22: Operation timed out
softzino@Mds-MacBook-Pro aws % ssh -i my-project-key.pem ubuntu@56.10.120.85                                           
ssh: connect to host 56.10.120.85 port 22: Operation timed out
softzino@Mds-MacBook-Pro aws % ec2-instance-connect ssh \
--instance-id i-0742fb935572579b3 \
--region ap-southeast-1
zsh: command not found: ec2-instance-connect
softzino@Mds-MacBook-Pro aws % aws --version
zsh: command not found: aws
softzino@Mds-MacBook-Pro aws % ssh -i my-project-key.pem ubuntu@56.10.120.85
ssh: connect to host 56.10.120.85 port 22: Operation timed out
softzino@Mds-MacBook-Pro aws % curl https://checkip.amazonaws.com

59.153.28.85
softzino@Mds-MacBook-Pro aws % curl https://checkip.amazonaws.com
59.153.28.86
softzino@Mds-MacBook-Pro aws % nc -vz 56.10.120.85 22

nc: connectx to 56.10.120.85 port 22 (tcp) failed: Operation timed out
softzino@Mds-MacBook-Pro aws % curl https://checkip.amazonaws.com
59.153.28.87
softzino@Mds-MacBook-Pro aws % nc -vz 56.10.120.85 22

nc: connectx to 56.10.120.85 port 22 (tcp) failed: Operation timed out
softzino@Mds-MacBook-Pro aws % curl https://checkip.amazonaws.com

59.153.28.86
softzino@Mds-MacBook-Pro aws % nc -vz 56.10.120.85 22

nc: connectx to 56.10.120.85 port 22 (tcp) failed: Operation timed out
softzino@Mds-MacBook-Pro aws % traceroute 56.10.120.85

traceroute to 56.10.120.85 (56.10.120.85), 64 hops max, 40 byte packets
1  192.168.0.1 (192.168.0.1)  5.505 ms  4.600 ms  5.573 ms
2  10.134.128.62 (10.134.128.62)  4.752 ms  6.293 ms  5.614 ms
3  10.134.128.61 (10.134.128.61)  7.406 ms  6.851 ms  5.270 ms
4  172.16.158.185 (172.16.158.185)  6.167 ms  5.765 ms  5.455 ms

softzino@Mds-MacBook-Pro aws %
softzino@Mds-MacBook-Pro aws % ssh -i my-project-key.pem ubuntu@56.10.120.85
The authenticity of host '56.10.120.85 (56.10.120.85)' can't be established.
ED25519 key fingerprint is: SHA256:nP2TQQgUcnJDMEFZ2dX6pbwAy51lkdAS03qYNH9TFp0
This key is not known by any other names.
Are you sure you want to continue connecting (yes/no/[fingerprint])? yes
Warning: Permanently added '56.10.120.85' (ED25519) to the list of known hosts.
Welcome to Ubuntu 24.04.4 LTS (GNU/Linux 6.17.0-1017-aws x86_64)

* Documentation:  https://help.ubuntu.com
* Management:     https://landscape.canonical.com
* Support:        https://ubuntu.com/pro

System information as of Fri Sep 25 01:39:06 UTC 2026

System load:  0.08              Temperature:           -273.1 C
Usage of /:   42.3% of 6.71GB   Processes:             113
Memory usage: 31%               Users logged in:       1
Swap usage:   0%                IPv4 address for ens5: 172.31.5.244

* Ubuntu Pro delivers the most comprehensive open source security and
  compliance features.

  https://ubuntu.com/aws/pro

Expanded Security Maintenance for Applications is not enabled.

45 updates can be applied immediately.
To see these additional updates run: apt list --upgradable

Enable ESM Apps to receive additional future security updates.
See https://ubuntu.com/esm or run: sudo pro status


*** System restart required ***
Last login: Fri Sep 25 01:03:39 2026 from 3.0.5.35
ubuntu@ip-172-31-5-244:~$ 

----


