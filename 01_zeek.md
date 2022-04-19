# Zeek aka bro

##  Intro to zeek 
  - Zeek is a passive network monitoring system
  - zeek can be installed on a "sensor", which can be hardware, software, virtual, or cloud platform that passively sniffs the network traffic
  - zeek generates compact, high-fidelity log files bases on what it sees in the network traffic
  - log files are fully customizable, suitble for manual review on disk or in a more analyst-firendly tool like a security and info event management (SIEM).
  
  ## Installing zeek
  ```bash
  # sudo su
● # sudo apt-get install cmake make gcc g++ flex bison libpcap-dev libssl-dev python-dev swig
zlib1g-dev
● # sudo sh -c "echo 'deb
http://download.opensuse.org/repositories/security:/zeek/xUbuntu_18.04/ /' >
/etc/apt/sources.list.d/security:zeek.list"
● # wget -nv
https://download.opensuse.org/repositories/security:zeek/xUbuntu_18.04/Release.key -O
Release.key
● # sudo apt-key add - < Release.key
● # sudo apt-get update
● # sudo apt-get install zeek-lts
● # /opt/zeek/bin/zeek -h

``` 
## Or 
[zeek docs](https://docs.zeek.org/en/master/install.html)


