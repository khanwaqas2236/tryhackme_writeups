# 🛡️TryHackMe NetSec Challenge Writeup

            Platform: TryHackMe

            Challenge Name: NetSec

            Difficulty: (/Medium)

            Category: Networking / Security / Penetration Testing

            Author: [KWAT]
            

![ohsint1 Screenshot](./Screenshot%20(154).png)



 # Task 1: OhSINT
 
 first, you need to download the image given above 
 
 
on your kali linux system or parrot os or cent etc run


Download the tool using sudo apt install exiftool

     

![ohsint2 Screenshot](./Screenshot%20(155).png)



Q1: What is this users avatar of?




![ohsint1 Screenshot](./Screenshot%20(156).png)



Here is the links to all the account:


the twitter profile


the github profile


the blog

Our best bet to see avatar image among these three account is Twitter.


Let us open that:



![ohsint1 Screenshot](./Screenshot%20(157).png)






# Q1: What is this user's avatar of?
 cat

# Q2: What city is this person in?


london

Ok, we need to find BSSID and use Wigle.net to search the BSSID.

BSSID is basically your router’s ID.



![ohsint1 Screenshot](./Screenshot%20(158).png)



Wigle.net is a search engine to search network related items.



For BSSID, we just need to scroll down the twitter to found this tweet:






Ok, for wigle.net


register to the website (you can put dummy account in here)


Go to View > Basic Search


 Use Advanced Search

 ![ohsint1 Screenshot](./Screenshot%20(159).png)

 

 ![ohsint1 Screenshot](./Screenshot%20(160).png)

 
 
 ![ohsint1 Screenshot](./Screenshot%20(161).png)
 


 
# Q3:Whats the SSID of the WAP he connected to?


 UnileverWiFi(already found in q2)
 

# Q4: What is his personal email address?


OWoodflint@gmail.com


We can’t find the email on Twitter.


So, let us open the github account and there just scroll a bit and u will find it

# Q5: What site did you find his email address on?

github

# Q6: Where has he gone on holiday?

New York

We can’t found the answer in Github and Twitter.

Now, let us open the blog:


![ohsint1 Screenshot](./Screenshot%20(162).png)


# Q7: What is this persons password?


pennYDr0pper.!



this one was a bit tricky for this we have to view the page source just simply right click and select



view page source and skimming through u will find this 



![ohsint1 Screenshot](./Screenshot%20(163).png)



i have a short trick too sice the password is hidden in the webpage just highlight the whole page



and you will see it booooom!!!



![ohsint1 Screenshot](./Screenshot%20(164).png)
