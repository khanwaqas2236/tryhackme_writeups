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
- Examining job postings related to the target website.
- Reading stories about the target company in the news.
# 2. Active Recon —- 

It involves direct interaction with the target. Consider checking the locks on doors and windows, as well as other potential entry points.

- Connecting to a company server using HTTP, FTP, or SMTP.
- 
- Attempting to obtain information by calling the company (social engineering).
- 
- Pretending to be a repairman and entering the company's premises.
-
- Take notice that without necessary legal authorization is obtained, it may result in legal complications.

- 

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

Answer: A

Because you are physically performing a “activity” that necessitates direct contact with the target.



# TASK3:WHOIS

WHOIS — It is a (1) request and (2) response protocol in which the “WHOIS” server listens for incoming requests on TCP port

43


Method to use in a console or terminal box:

whois <domain_name> 

example: whois tryhackme.com

add pic)(


# [Question 3.1]

When was TryHackMe.com registered?

Answer: 20180705

# [Question 3.2] 

What is the registrar of TryHackMe.com?

Answer: namecheap.com

# [Question 3.3]

Which company is TryHackMe.com using for name servers?

Answer: cloudflare.com


# TASK4:nslookup and dig

nslookup — It means “Name Server Look Up”, and the objective is to look for the IP address of a domain name.

                               nslookup domain_name.com






