Task-1 hello
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






                        
![Website Screenshot](./Screenshot%20(122).png)




So, here I was able to locate a Github account related to the name discovered in previous task.I then went to ETH to discover
base64 encoding.



![Website Screenshot2](./Screenshot%20(124).png)


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



Task-5
Q.1.What is the attacker current twitter handle???
@SakuraLoverAiko

Q.2.
![Website Screenshot](./Screenshot%20(125).png)

For this one I referred the SS provided in the hints. Tor was not actually setup and didn’t had much time for it.


![Website Screenshot](./Screenshot%20(126).png)

Also include the md5 hash as well else won’t be correct.


http://deepv2w7p33xa4pwxzwi2ps4j62gfxpyp44ezjbmpttxz3owlsp4ljid.onion/show.php?md5=b2b37b3c106eb3f86e2340a3050968e2




Q.3.
What is the BSSID for the attacker's home wifi???
84:AFËC:34:FC:F8

For this you need ‘wigle.net’. If you are going through some books or any OSINT resources, then you’ll definitely come up with this site to do lookups on SSIDs or BSSIDs.


Task-6
Q.1.

what airport is closest to the attacker shared the photo prior ....???
DCA



for this i did reverse google lookup u can also use yandex






