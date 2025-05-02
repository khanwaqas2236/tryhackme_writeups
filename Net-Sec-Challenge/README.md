#  🛡️TryHackMe NetSec Challenge Writeup

            Platform: TryHackMe

            Challenge Name: NetSec

            Difficulty: (/Medium)

            Category: Networking / Security / Penetration Testing

            Author: [KWAT]

# Enumeration

                             nmap -sC -sV -T4 -Pn <target-ip> -oN nmap.txt
                             

This should give us plenty of information and as expected, once we get back the results

we can answer the first three questions.


![nmap1 Screenshot](./Screenshot%20(147).png)


What is the highest port number being open less than 10,000? 8080

There is an open port outside the common 1000 ports; it is above 10,000. What is it? 10021

How many TCP ports are open? 6

The next question wants us to find the flag hidden in the HTTP server header.


  ![nmap2 Screenshot](./Screenshot%20(148).png)

  

  We’ll get quite a few results when we list all the http scripts that are available,
  
  but there is one that should catch your eye for this particular task.

  

  ![nmap3 Screenshot](./Screenshot%20(149).png)

  

  the script works and we have a flag

  

  ![nmap4 Screenshot](./Screenshot%20(150).png)

  

  We can try looking through the nmap scripts for ssh, but we find nothing promising this time.
  
  What if we connect to the port with telnet?

  

  ![nmap5 Screenshot](./Screenshot%20(151).png)



  As soon as we do that, we are greated with the header and the flag we need.
  
  
 {QUESTION} We have an FTP server listening on a nonstandard port. What is the version of the FTP server?

 The nmap scan we ran in the beginning already gave us that information.
 
 The port is 10021 and the FTP server version is vsftpd 3.0.3.


 After that we are given two usernames that can give us FTP access — eddie and quinn.

We’re going to have to find the passwords that go along with those usernames however.

Hydra should come in handy here along with our trusty rockyou.txt wordlist.


                              hydra -l eddie -P /usr/share/wordlists/rockyou.txt
                              
                              hydra =l quinn -P /usr/share/wordlists/rockyou.txt


We run Hydra for both accounts and very quickly receive the corresponding passwords. 

Now we have to log in as both users and see if we can find any files.

We are met with success when we log in as quinn.



  ![nmap6 Screenshot](./Screenshot%20(152).png)

  


All we need to do is download the file to our local machine with get and then read it to obtain the flag.


                                   cat any_filename.txt

{LAST_FLAG}

doing a null scan 


                                    nmap -sN ip-OF-machine


  ![nmap7 Screenshot](./Screenshot%20(153).png)
  
