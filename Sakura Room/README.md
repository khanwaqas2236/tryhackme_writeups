Task-1
Q.1.


Definitely!

Task-2
Q.1.

What username does the attacker goes by?
SakuraSnowAngelaiko


Don’t waste time looking for this binary and what it means. I wasted a lot on it. I simply converted this to decimal and then referred the ASCII table.
Finally => ‘A picture worth 1000 words but metadata is worth far more’

Simply right click and view page source to look through the metadata.
(‘metadata’ is information of an information. Here picture is the information whose further information is revealed via it’s metadata.)


                     filename = /home/sakurasnowangelaiko/Desktop/pwnedletter.png

                     
So, in the /home directory, if you are familiar with linux then you know it is a username.




TASK-3:
What is the full email address used by the attacker?
                           sakurasnowangel83@protonmail.com


This one was way too tricky and I too learned something new here. Initially I was trying to locate a LinkedIn profile
where I could have possibly found an email but couldn’t.
So, going through one of the writeups found the PGP keys can reveal emails.






                               *(adding pic)



So, here I was able to locate a Github account related to the name discovered in previous task.



                               *(adding pic)


So, by base64 decoding(use any online or burp decoder) we revail the email

Q.2.
What is the attacker's full real name ?
Found a twitter handle as well and there found one of the tweets.
@aikoabe3



Task-4
Q.1.

What cryptocurrency does the attacker uses???
Etherium it is
simply i went to @aikoabe3 github and found the repos

Q.2.
What is the attacker cryptocurrency wallet address???
0xa102397dbeeBeFD8cD2F73A89122fCdB53abB6ef

So, by going to the history section of the file located in ETH repo, found interesting commits.



Q.3.
What minning pool did the attacker recieve payments from on january23,2021 UTC???
ethermine



Use any site to search for this wallet address. I am using ‘etherscan.io’.



Q.4.
What other cryptocurrency did the attacker exchange using their wallet???
tether

same tool as above used etherscan.io

