
# 시험

## instance 3-1과 instance 4-1

```bash
buntu@ip-10-40-1-10:~$ ping 10.30.1.10 -c 3
PING 10.30.1.10 (10.30.1.10) 56(84) bytes of data.
64 bytes from 10.30.1.10: icmp_seq=1 ttl=62 time=230 ms
64 bytes from 10.30.1.10: icmp_seq=2 ttl=62 time=229 ms
64 bytes from 10.30.1.10: icmp_seq=3 ttl=62 time=229 ms

--- 10.30.1.10 ping statistics ---
3 packets transmitted, 3 received, 0% packet loss, time 2002ms
rtt min/avg/max/mdev = 229.340/229.573/230.034/0.325 ms
ubuntu@ip-10-40-1-10:~$
ubuntu@ip-10-40-1-10:~$ ssh -i ./.ssh/redrabbit 10.30.1.10
The authenticity of host '10.30.1.10 (10.30.1.10)' can't be established.
ECDSA key fingerprint is SHA256:1M0IwCvNceEoxbqgocSC6BzPvE5Jjuc2A0JNeHhgE+E.
Are you sure you want to continue connecting (yes/no)? yes
Warning: Permanently added '10.30.1.10' (ECDSA) to the list of known hosts.
Welcome to Ubuntu 18.04.4 LTS (GNU/Linux 4.15.0-1065-aws x86_64)

 * Documentation:  https://help.ubuntu.com
 * Management:     https://landscape.canonical.com
 * Support:        https://ubuntu.com/advantage

  System information as of Sun Jun 21 09:57:18 UTC 2020

  System load:  0.0               Processes:           91
  Usage of /:   14.2% of 7.69GB   Users logged in:     0
  Memory usage: 20%               IP address for eth0: 10.30.1.10
  Swap usage:   0%


 * Canonical Livepatch is available for installation.
   - Reduce system reboots and improve kernel security. Activate at:
     https://ubuntu.com/livepatch

0 packages can be updated.
0 updates are security updates.

Failed to connect to https://changelogs.ubuntu.com/meta-release-lts. Check your Internet connection or proxy settings



The programs included with the Ubuntu system are free software;
the exact distribution terms for each program are described in the
individual files in /usr/share/doc/*/copyright.

Ubuntu comes with ABSOLUTELY NO WARRANTY, to the extent permitted by
applicable law.

To run a command as administrator (user "root"), use "sudo <command>".
See "man sudo_root" for details.

ubuntu@ip-10-30-1-10:~$ ping 10.40.1.10 -c 3
PING 10.40.1.10 (10.40.1.10) 56(84) bytes of data.
64 bytes from 10.40.1.10: icmp_seq=1 ttl=62 time=229 ms
64 bytes from 10.40.1.10: icmp_seq=2 ttl=62 time=228 ms
64 bytes from 10.40.1.10: icmp_seq=3 ttl=62 time=228 ms

--- 10.40.1.10 ping statistics ---
3 packets transmitted, 3 received, 0% packet loss, time 2002ms
rtt min/avg/max/mdev = 228.521/228.735/229.089/0.606 ms
ubuntu@ip-10-30-1-10:~$
ubuntu@ip-10-30-1-10:~$ ssh -i ./.ssh/redrabbit 10.40.1.10
The authenticity of host '10.40.1.10 (10.40.1.10)' can't be established.
ECDSA key fingerprint is SHA256:OlF/4lNYq4V9+obP+VS76lEPjDvlClHsPg103s6N4bw.
Are you sure you want to continue connecting (yes/no)? yes
Warning: Permanently added '10.40.1.10' (ECDSA) to the list of known hosts.
Welcome to Ubuntu 18.04.4 LTS (GNU/Linux 4.15.0-1065-aws x86_64)

 * Documentation:  https://help.ubuntu.com
 * Management:     https://landscape.canonical.com
 * Support:        https://ubuntu.com/advantage

  System information as of Sun Jun 21 09:58:34 UTC 2020

  System load:  0.0               Processes:           92
  Usage of /:   14.1% of 7.69GB   Users logged in:     0
  Memory usage: 21%               IP address for eth0: 10.40.1.10
  Swap usage:   0%


 * Canonical Livepatch is available for installation.
   - Reduce system reboots and improve kernel security. Activate at:
     https://ubuntu.com/livepatch

0 packages can be updated.
0 updates are security updates.

Failed to connect to https://changelogs.ubuntu.com/meta-release-lts. Check your Internet connection or proxy settings



The programs included with the Ubuntu system are free software;
the exact distribution terms for each program are described in the
individual files in /usr/share/doc/*/copyright.

Ubuntu comes with ABSOLUTELY NO WARRANTY, to the extent permitted by
applicable law.

To run a command as administrator (user "root"), use "sudo <command>".
See "man sudo_root" for details.

ubuntu@ip-10-40-1-10:~$
```

#### Instance 3-1에서 인터넷 접근

```bash
ubuntu@ip-10-30-1-10:~$ sudo apt update
Hit:1 http://eu-west-1.ec2.archive.ubuntu.com/ubuntu bionic InRelease
Get:2 http://security.ubuntu.com/ubuntu bionic-security InRelease [88.7 kB]
Get:3 http://eu-west-1.ec2.archive.ubuntu.com/ubuntu bionic-updates InRelease [88.7 kB]
Get:4 http://security.ubuntu.com/ubuntu bionic-security/main amd64 Packages [748 kB]
Get:5 http://eu-west-1.ec2.archive.ubuntu.com/ubuntu bionic-backports InRelease [74.6 kB]
Get:6 http://eu-west-1.ec2.archive.ubuntu.com/ubuntu bionic/universe amd64 Packages [8570 kB]
Get:7 http://security.ubuntu.com/ubuntu bionic-security/main Translation-en [237 kB]
Get:8 http://security.ubuntu.com/ubuntu bionic-security/restricted amd64 Packages [50.8 kB]
Get:9 http://security.ubuntu.com/ubuntu bionic-security/restricted Translation-en [12.3 kB]
Get:10 http://security.ubuntu.com/ubuntu bionic-security/universe amd64 Packages [673 kB]
Get:11 http://security.ubuntu.com/ubuntu bionic-security/universe Translation-en [223 kB]
Get:12 http://security.ubuntu.com/ubuntu bionic-security/multiverse amd64 Packages [7808 B]
Get:13 http://security.ubuntu.com/ubuntu bionic-security/multiverse Translation-en [2856 B]
Get:14 http://eu-west-1.ec2.archive.ubuntu.com/ubuntu bionic/universe Translation-en [4941 kB]
Get:15 http://eu-west-1.ec2.archive.ubuntu.com/ubuntu bionic/multiverse amd64 Packages [151 kB]
Get:16 http://eu-west-1.ec2.archive.ubuntu.com/ubuntu bionic/multiverse Translation-en [108 kB]
Get:17 http://eu-west-1.ec2.archive.ubuntu.com/ubuntu bionic-updates/main amd64 Packages [969 kB]
Get:18 http://eu-west-1.ec2.archive.ubuntu.com/ubuntu bionic-updates/main Translation-en [329 kB]
Get:19 http://eu-west-1.ec2.archive.ubuntu.com/ubuntu bionic-updates/restricted amd64 Packages [60.5 kB]
Get:20 http://eu-west-1.ec2.archive.ubuntu.com/ubuntu bionic-updates/restricted Translation-en [14.7 kB]
Get:21 http://eu-west-1.ec2.archive.ubuntu.com/ubuntu bionic-updates/universe amd64 Packages [1085 kB]
Get:22 http://eu-west-1.ec2.archive.ubuntu.com/ubuntu bionic-updates/universe Translation-en [337 kB]
Get:23 http://eu-west-1.ec2.archive.ubuntu.com/ubuntu bionic-updates/multiverse amd64 Packages [15.9 kB]
Get:24 http://eu-west-1.ec2.archive.ubuntu.com/ubuntu bionic-updates/multiverse Translation-en [6420 B]
Get:25 http://eu-west-1.ec2.archive.ubuntu.com/ubuntu bionic-backports/main amd64 Packages [7516 B]
Get:26 http://eu-west-1.ec2.archive.ubuntu.com/ubuntu bionic-backports/main Translation-en [4764 B]
Get:27 http://eu-west-1.ec2.archive.ubuntu.com/ubuntu bionic-backports/universe amd64 Packages [7484 B]
Get:28 http://eu-west-1.ec2.archive.ubuntu.com/ubuntu bionic-backports/universe Translation-en [4436 B]
Fetched 18.8 MB in 11s (1673 kB/s)
Reading package lists... Done
Building dependency tree
Reading state information... Done
73 packages can be upgraded. Run 'apt list --upgradable' to see them.
ubuntu@ip-10-30-1-10:~$
```

```azurepowershell
ubuntu@ip-10-30-1-10:~$ wget export.jhin.net/a.pptx
--2020-06-21 11:09:47--  http://export.jhin.net/a.pptx
Resolving export.jhin.net (export.jhin.net)... 13.224.67.110, 13.224.67.80, 13.224.67.49, ...
Connecting to export.jhin.net (export.jhin.net)|13.224.67.110|:80... connected.
HTTP request sent, awaiting response... 301 Moved Permanently
Location: https://export.jhin.net/a.pptx [following]
--2020-06-21 11:09:48--  https://export.jhin.net/a.pptx
Connecting to export.jhin.net (export.jhin.net)|13.224.67.110|:443... connected.
HTTP request sent, awaiting response... 200 OK
Length: 30446086 (29M) [application/vnd.openxmlformats-officedocument.presentationml.presentation]
Saving to: 'a.pptx'

a.pptx                               100%[======================================================================>]  29.04M  3.11MB/s    in 12s

2020-06-21 11:10:03 (2.49 MB/s) - 'a.pptx' saved [30446086/30446086]

ubuntu@ip-10-30-1-10:~$
```

```azurecli
ubuntu@ip-10-30-1-10:~$ sudo apt install traceroute
Reading package lists... Done
Building dependency tree
Reading state information... Done
The following NEW packages will be installed:
  traceroute
0 upgraded, 1 newly installed, 0 to remove and 73 not upgraded.
Need to get 45.4 kB of archives.
After this operation, 152 kB of additional disk space will be used.
Get:1 http://eu-west-1.ec2.archive.ubuntu.com/ubuntu bionic/universe amd64 traceroute amd64 1:2.1.0-2 [45.4 kB]
Fetched 45.4 kB in 2s (24.4 kB/s)
Selecting previously unselected package traceroute.
(Reading database ... 56588 files and directories currently installed.)
Preparing to unpack .../traceroute_1%3a2.1.0-2_amd64.deb ...
Unpacking traceroute (1:2.1.0-2) ...
Setting up traceroute (1:2.1.0-2) ...
update-alternatives: using /usr/bin/traceroute.db to provide /usr/bin/traceroute (traceroute) in auto mode
update-alternatives: using /usr/bin/lft.db to provide /usr/bin/lft (lft) in auto mode
update-alternatives: using /usr/bin/traceproto.db to provide /usr/bin/traceproto (traceproto) in auto mode
update-alternatives: using /usr/sbin/tcptraceroute.db to provide /usr/sbin/tcptraceroute (tcptraceroute) in auto mode
Processing triggers for man-db (2.8.3-2ubuntu0.1) ...
ubuntu@ip-10-30-1-10:~$
```

```bash
ubuntu@ip-10-30-1-10:~$ traceroute export.jhin.net
traceroute to export.jhin.net (13.224.67.15), 30 hops max, 60 byte packets
 1  * * *
 2  * * *
 3  ip-10-20-2-103.eu-west-1.compute.internal (10.20.2.103)  230.104 ms  230.165 ms  234.580 ms
 4  ec2-52-79-0-92.ap-northeast-2.compute.amazonaws.com (52.79.0.92)  242.783 ms ec2-52-79-0-130.ap-northeast-2.compute.amazonaws.com (52.79.0.130) 237.966 ms ec2-52-79-0-132.ap-northeast-2.compute.amazonaws.com (52.79.0.132)  238.329 ms
 5  100.66.8.116 (100.66.8.116)  247.321 ms 100.66.8.106 (100.66.8.106)  237.415 ms 100.66.8.74 (100.66.8.74)  233.120 ms
 6  100.66.10.100 (100.66.10.100)  238.173 ms 100.66.10.66 (100.66.10.66)  241.730 ms 100.66.11.96 (100.66.11.96)  234.367 ms
...
```