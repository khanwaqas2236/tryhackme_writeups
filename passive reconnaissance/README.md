# 🛡️TryHackMe NetSec Challenge Writeup

            Platform: TryHackMe

            Challenge Name: NetSec

            Difficulty: (/Medium)

            Category: Networking / Security / Penetration Testing

            Author: [KWAT]

            Lab Access: https://tryhackme.com/room/passiverecon


# TASK1:Introduction



In this room, after we define passive reconnaissance and active reconnaissance, we focus on essential tools related to 



passive reconnaissance. We will learn three command-line tools:



whois to query WHOIS servers


nslookup to query DNS servers


dig to query DNS servers



We use whois to query WHOIS records, while we use nslookup and dig to query DNS database records. These are all publicly 

available records and hence do not alert the target.


We will also learn the usage of two online services:


DNSDumpster



Shodan.io



# [Question 1.1]



This room does not use a target virtual machine (VM) to demonstrate the discussed topics. Instead, we will



query public WHOIS servers and DNS servers for domains owned by TryHackMe. Start the AttackBox and make sure it is ready. You



will use the AttackBox to answer the questions in later tasks, especially tasks 3 and 4.



# Answer: 



No answer is needed.


# TASK2:ACTIVE RECON VS PASSIVE RECON



Reconnaissance is classified into two types:



# 1. Passive Recon -- 



Without personally engaging with the target, rely on publicly available knowledge and access to publicly available resources.



Consider it as if you are looking at a target territory from afar without ever setting foot on it.



- Obtaining a domain's DNS records from a public DNS server.

- 
- Examining job postings related to the target website.

- 
- Reading stories about the target company in the news.
- 

- 
# 2. Active Recon —- 



It involves direct interaction with the target. Consider checking the locks on doors and windows, as well as other potential


entry points.

- Connecting to a company server using HTTP, FTP, or SMTP.

  
  
- Attempting to obtain information by calling the company (social engineering).

  
  
- Pretending to be a repairman and entering the company's premises.

- 

- Take notice that without necessary legal authorization is obtained, it may result in legal complications.

  

# [Question 2.1]



You visit the Facebook page of the target company, hoping to get some of their employee names. What kind of reconnaissance 



activity is this? (A for active, P for passive)



# Answer:


P


# [Question 2.2] 



You ping the IP address of the company webserver to check if ICMP traffic is blocked. What kind of reconnaissance activity is



this? (A for active, P for passive)



# Answer:



A


Because the concept of “ping” is comparable to ringing the doorbell



# [Question 2.3] 



You happen to meet the IT administrator of the target company at a party. You try to use social engineering to get more 



information about their systems and network infrastructure. What kind of reconnaissance activity is this? (A for active, P



for passive)



# Answer:


A


Because you are physically performing a “activity” that necessitates direct contact with the target.



# TASK3:WHOIS



WHOIS — It is a (1) request and (2) response protocol in which the “WHOIS” server listens for incoming requests on TCP port

43


Method to use in a console or terminal box:


                                       whois <domain_name> 


                                       example: whois tryhackme.com
                                       


![nmap1 Screenshot](./Screenshot%20(165).png)



# [Question 3.1]



When was TryHackMe.com registered?



# Answer: 


20180705




# [Question 3.2] 



What is the registrar of TryHackMe.com?



# Answer: 


namecheap.com


# [Question 3.3]



Which company is TryHackMe.com using for name servers?



# Answer:


cloudflare.com


# TASK4:nslookup and dig



nslookup — It means “Name Server Look Up”, and the objective is to look for the IP address of a domain name.



                               nslookup domain_name.com


![nmap1 Screenshot](./Screenshot%20(166).png)


These three main parameters are as follows:



OPTIONS — contains the query type, and you can use “A” for IPv4 addresses and “AAAA” for IPv6 addresses, for example.


DOMAIN_NAME — the domain name you are looking up.


SERVER — the DNS server that you want to query, and you can query any local or public DNS server. Cloudflare offers 1.1.1.1


and 1.0.0.1, Google offers 8.8.8.8 and 8.8.4.4, and Quad9 offers 9.9.9.9 and 149.112.112.112



For example, nslookup -type=A tryhackme.com 1.1.1.1 (or nslookup -type=a tryhackme.com 1.1.1.1 due to case insensitivity)



can be used to return all of tryhackme.com’s IPv4 addresses.



![nmap1 Screenshot](./Screenshot%20(167).png)



Recognizing “nslookup” is beneficial for penetration testing because, in the example above, we started with “1 domain name”


and ended up with “3 IPv4 addresses”.


Another example is in the area of “mail server,” such as which we wish to learn more, and we can use the approach below:



                        nslookup -type=MX tryhackme.com

                        


![nmap1 Screenshot](./Screenshot%20(168).png)




Tryhackme.com’s current email settings appears to be Google.



Since MX is searching for Mail Exchange servers, we note that when a mail server attempts to deliver email to 



@tryhackme.com, it will attempt to connect to aspmx.l.google.com, which has order 1.



If it is busy or unavailable, the mail server will try to connect to the next mail exchange server in line, 



alt1.aspmx.l.google.com or alt2.aspmx.l.google.com.



dig (Domain Information Groper) — It appears to be for more advanced DNS queries and more functionality, 



and it is similar to “nslookup,” which gathers DNS information but is also useful for troubleshooting DNS issues.



                          dig <domain_name>
                          


![nmap1 Screenshot](./Screenshot%20(169).png)




Next, suppose we wish to learn more about the information from the mail server.



                               dig <domain_name> <type>
                               
                               example: dig tryhackme.com MX

                               


![nmap1 Screenshot](./Screenshot%20(170).png)



A simple comparison of the output of “nslookup” and “dig” reveals that dig returned additional information by default,



such as the TTL (Time To Live).



TTL (Time To Live) — It is a value that represents the amount of time a packet or data should exist on a computer



or network before being discarded. TTL, or packet lifetime, has many meanings depending on the context.



TTL in networking stops data packets from flowing forever over a network.



TTL regulates data caching and improves speed in programs.



TTL is also employed in other contexts, such as content delivery network (CDN) caching and domain name system (DNS) caching.




# [Question 4.1]



Check the TXT records of thmlabs.com. What is the flag there?


![nmap1 Screenshot](./Screenshot%20(171).png)



# Answer: 



THM{a5b83929888ed36acb0272971e438d78}



# TASK5:DNSDUMPSTER



# [Question 5.1] 


Lookup tryhackme.com on DNSDumpster. What is one interesting subdomain that you would discover



in addition to www and blog?



1st — Access into DNSDumpster


                                 www.dnsdumpster.com
                                 


2nd — Search “tryhackme.com” and you will see this




![nmap1 Screenshot](./Screenshot%20(172).png)




# Answer:



remote


# TASK6:SHODAN.IO



It offers a wide range of capabilities within this open source tool, and one of my highlights



is that it is remarkably close to “Nmap.”



Shodan.io — A tool such as this is notably useful for learning various pieces of



information about the client’s network during penetration testing (without actively connecting to it).



![nmap1 Screenshot](./Screenshot%20(173).png)


# [Question 6.1] 


According to Shodan.io, what is the 2nd country in the world in terms of the number of publicly accessible 


Apache servers?


Shodan will return the result of a search for “Apache Server.”





![nmap1 Screenshot](./Screenshot%20(174).png)




# Answer: Germany




# [Question 6.2]


Based on Shodan.io, what is the 3rd most common port used for Apache?



![nmap1 Screenshot](./Screenshot%20(175).png)



# Answer: 8080



# [Question 6.3]


Based on Shodan.io, what is the 3rd most common port used for nginx?


![nmap1 Screenshot](./Screenshot%20(174).png)



# Answer:



8888












