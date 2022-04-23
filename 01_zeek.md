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

## Comprehensive logs = Attackers can't hide

<p>
   Zeek turns raw network traffic into Comprehensive logs organized by protocol, with key fiels extracted specifically for security use cases
   </p>
   
   
`network.cfg`
 - Monitor the network you want enternal or external
 
 `node.cfg`
 - Select the interface like eth0, wlan0
 
 `zeekctl.cfg`
 - All the configrations, where are the roots files etc
 
 ### Runing the zeek
 ```bash
 zeekctl deploy
 
 ## Check status
 zeekctl status
 ```
 ### Check the logs
  
```bash
/opt/zeel/spool

```
### stop the zeek
```bash
zeekctl stop
```
#### Check all the logs
```
/opt/zeek/logs/
```

[Wazuh] can be used to view the details
-----------------------------------------------------------------------------------

### Installing zeek-cut

 - From the dithub page download [zeek-aux](https://github.com/zeek/zeek-aux) 
 - Extract the zeek-aux clone in to `/opt/` folder
 - Now download the dependencies for zeek-aux [cmake](https://github.com/zeek/cmake)
 - Now install the zeek-aux by
 ```bash
 ./configure
 # step 2
 sudo make
 # step 3
 sudo make install
```
 - Set the path for zeek-cut
 - update the Database by `updatedb`
 - locate the file by `locate zeek-cut`
 - type $PATH in terminal 
 - select the path you want to copy zeek-cut into
 ```bash
 mv /use/bin/zeek-cut /use/bin
 ```
 - Now type `zeek-cut -h` in terminal for help menu
 - [Zeek cheat sheet](https://github.com/corelight/bro-cheatsheets/blob/master/Corelight-Bro-Cheatsheets-2.5.pdf)
 


