#!/bin/bash
# by Greg Lawler greg@zinkwazi.com
# 2/20/2013
# to do list:
# confirm successful tunnel creation
# verbose output from ps
# default config and ability to override from command line
# show list of available tunnels from config
 
set -e
# Let's have fancy colors!
txtgrn=$(tput setaf 2) # Green
txtred=$(tput setaf 1) # Red
txtrst=$(tput sgr0) # Text reset
 
# Tell the user some stuff...
echo "${txtgrn}Stopping existing tunnels..."
echo ""
ps -ef | grep autossh | grep -v grep | awk '{print $2}' | xargs kill -9
ps -ef | grep ssh | grep -v grep | grep -v sshTunnel.sh | awk '{print $2}' | xargs kill -9
ps -ef | grep ssh | grep 5061: | grep -v grep | awk '{print $2}' | xargs kill -9
ps -ef | grep ssh | grep 10[8-9][0-9] | grep -v grep | awk '{print $2}' | xargs kill -9
echo "${txtgrn}Starting dynamic SSH proxy tunnel on localhost:1080"
 
#Set up the dynamic tunnel with autossh
/usr/local/bin/autossh -p 80 -f -N -M 0 -D 1080 -o ServerAliveInterval=3 -o ServerAliveCountMax=3 admin@office.outsideopen.com
#/usr/local/bin/autossh -f -N -M 0 -D 1080 -o ServerAliveInterval=3 -o ServerAliveCountMax=3 admin@vpn.dbntm.com
#/usr/local/bin/autossh -f -N -M 0 -D 1080 -o ServerAliveInterval=3 -o ServerAliveCountMax=3 admin@192.168.5.1
#/usr/local/bin/autossh -f -N -M 0 -D 1080 -o ServerAliveInterval=3 -o ServerAliveCountMax=3 glawler@pilot.westmont.edu
#/usr/local/bin/autossh -f -N -M 0 -D 1080 -o ServerAliveInterval=3 -o ServerAliveCountMax=3 glawler@webmail.sbcardio.com
#/usr/local/bin/autossh -f -N -M 0 -D 1080 glawler@dev.outsideopen.com
#/usr/local/bin/autossh -f -N -M 20000 -D 1080 glawler@zinkwazi.com
#/usr/local/bin/autossh -f -N -M 0 -D 1080 -o ServerAliveInterval=3 -o ServerAliveCountMax=3 glawler@ring.brooks.edu
#/usr/local/bin/autossh -f -N -M 0 -D 1080 -o ServerAliveInterval=3 -o ServerAliveCountMax=3 admin@office.venturevisuals.com
#/usr/local/bin/autossh -f -N -M 0 -D 1080 glawler@sofa.zncg.com
#/usr/local/bin/autossh -f -N -M 20000 -D 1080 vv
#/usr/local/bin/autossh -f -N -M 20000 -D 1080 admin@10.0.0.253
#/usr/local/bin/autossh -f -N -M 0 -D 1080 -o ServerAliveInterval=3 -o ServerAliveCountMax=3 glawler@ftp.colorservices.com
#/usr/local/bin/autossh -f -N -M 0 -D 1080 -o ServerAliveInterval=3 -o ServerAliveCountMax=3 root@ftp.petunia.com
 
#echo "${txtgrn}Starting SSH tunnel for oooffice on localhost:1088"
 
# Static tunnel for ssh to rind
/usr/local/bin/autossh -o ServerAliveInterval=3 -o ServerAliveCountMax=13 -f -N -M 0 -L 1088:10.10.1.16:22 glawler@dev.outsideopen.com
#/usr/bin/ssh -f -N -L 1088:10.10.1.16:22 glawler@dev.outsideopen.com
 
# Lync client connection
#/usr/bin/ssh -f -N -L 5061:10.50.113.59:5061 root@ring.brooks.edu
/usr/local/bin/autossh -o ServerAliveInterval=3 -o ServerAliveCountMax=13 -f -N -M 0 -L 5061:10.50.113.59:5061 root@ring.brooks.edu
 
#asterisk
/usr/local/bin/autossh -f -N -M 0 -L 1089:10.10.1.8:22 glawler@dev.outsideopen.com
 
#/usr/bin/ssh -f -N -L 1099:10.52.32.20:22 root@ring.brooks.edu
#/usr/bin/ssh -f -N -L 5902:10.0.1.224:5900 vv
 
#static tunnel to home asterisk
#/usr/bin/ssh -f -N -L 1099:192.168.5.20:22 glawler@sofa.zncg.com
#echo ""
ps auxw | grep -v grep | grep "bin/ssh"
echo "${txtrst}"
