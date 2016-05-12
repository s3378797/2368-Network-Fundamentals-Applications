2368-Network-Fundamentals-Applications


Network Commands Challenge 


These are the answers to the challenge 2 questions, they were researched and answered by me. They could contain erroes, so please only use them as references and use at your own risks. :p


1. Find the IP address the computer you are currently using.

ifconfig


2. Find the IP address the computer you are currently using, plus MAC address, plus whether DHCP is turned on.

Plug in another network card see if the card can be assigned an ip address.


3. Release and restore a DHCP connection for the LAN.

Release DHCP addr: dhclient -r (eth0); connect to internet again

Restore DHCP addr: dhclient -4; ifconfig;


4. Display the host name of the computer & be able to use the GUI to change it.

hostname; hostname newname; or etc/hostname


5. Check for basic IP connectivity between two computers by name and IP address.

nmap -sP 192.168.201.1-254; or ; arp -a;


6. Quickly access one or two other computers using a command, now display the MAC to IP address of those computers.

arp dstIPaddress


7. Show the MAC address of the host without using ipconfig.

netstat -i;


8. Show what shared resource are available on the host.

smbtree

smbclient -L localhost

net rpc share


9. Show what user accounts are available on the host.

cat /etc/passwd

cat /etc/passwd | grep -v nologin


10.Show what hosts are currently connected to a computer.

netstat -tn

netstat -tn 2>/dev/null


11.Find out which ports on your host are connected to applications. Connect the browser to some external web page before running the appropriate command.

netstat -tulpn


12.Someone claims there is a lot of corrupted packets on the network. How can this be checked?

netstat -s | grep segments

netstat -s


13.Find all other hosts available on the network.

nmap -sP 192.168.201.1-254; or ; arp -a;


14.Select one host and find what ports they have open.

nmap -T5 192.168.201.*


15.Find the path to the domain name server.

dig +trace google

dig +trace +short google


16.Show the address of the gateway.

route -n

netstat -r -n


17.Find the path of routers to www.google.com ( note this may be a problem within RMIT).

tcptraceroute google.com


18.One host on the network has gone berserk and is flooding the network with packets. How would you detect which host is responsible? Use ping with continuous send to simulate this.

Below is the wireshark filter command.
 
(not icmp.resp_in and icmp.type==8)


19.Some fool is using Telnet which does not encode passwords. How can you spy on the Telnet transactions?

Found two ways to do it. 


tcpdump port http or port ftp or port smtp or port imap or port pop3 -l -A | egrep -i 'pass=|pwd=|log=|login=|user=|username=|pw=|passw=|passwd=|password=|pass:|user:|username:|password:|login:|pass |user ' --color=auto --line-buffered -B20


    #!/bin/bash

    #
    # tcptelnet.sh - extract password and session information tcpdump
    #
    # 2008 - Mike Golvach - eggi@comcast.net
    #
    # Creative Commons Attribution-Noncommercial-Share Alike 3.0 United States License
    #

    login_or_pwd=0
    sess_info=0

    while read line
    do
            echo $line| grep "Last" >/dev/null 2>&1
            ll_yes_or_no=$?
            if [ $ll_yes_or_no -eq 0 ]
            then
                    echo
                    login_or_pwd=0
                    continue
            fi
            echo $line| grep "gin:" >/dev/null 2>&1
            l_yes_or_no=$?
            if [ $l_yes_or_no -eq 0 ]
            then
                    echo
                    echo "Possible login ID to follow: "
                    login_or_pwd=1
                    sess_info=0
                    continue
            fi
            echo $line| grep "sword" >/dev/null 2>&1
            p_yes_or_no=$?
            if [ $p_yes_or_no -eq 0 ]
            then
                    echo
                    echo "Possible password to follow: "
                    login_or_pwd=1
                    sess_info=0
                    continue
            fi
            if [ $login_or_pwd -eq 1 ]
            then
                    echo $line|awk '{ if ( $NF ~ /UUUUU$/ ) print $NF }'|sed 's/^.*\(.\)UUUUU$/\1/'|sed 's/^U/\./' |xargs -ivar echo "var\c"
                    continue
            fi
            if [ $login_or_pwd -eq 0 -a $sess_info -eq 0 ]
            then
                    echo
                    echo "Possible session info to follow:"
                    sess_info=1
            fi
            if [ $sess_info -eq 1 ]
            then
                    echo $line|awk '{ if ( $NF ~ /UUUUU$/ ) print $NF }'|sed 's/^.*\(.\)UUUUU$/\1/'|sed 's/^U/\./' |xargs -ivar echo "var\c"
                    continue
            fi
    done
    echo
    exit 0

    
20.A ping to 192.168.0.2 works but a ping to the machine's name "reds_machine" fails.
What could be wrong?


Hosts File

The host name in the Hosts file or in the command is misspelled.

The IP address for the host name in the Hosts file is invalid or incorrect.


DNS Server


    The client contacts the DNS name server with a recursive query for name.reskit.com. The server must now return the answer or an error message.

    The DNS name server checks its cache and zone files for the answer, but doesn't find it. It contacts a server at the root of the Internet (a root DNS server) with an iterative query for name.reskit.com.

    The root server doesn't know the answer, so it responds with a referral to an authoritative server in the .com domain.

    The DNS name server contacts a server in the .com domain with an iterative query for name.reskit.com.

    The server in the .com domain does not know the exact answer, so it responds with a referral to an authoritative server in the reskit.com domain.

    The DNS name server contacts the server in the reskit.com domain with an iterative query for name.reskit.com.

    The server in the reskit.com domain does know the answer. It responds with the correct IP address.

    The DNS name server responds to the client query with the IP address for name.reskit.com.

