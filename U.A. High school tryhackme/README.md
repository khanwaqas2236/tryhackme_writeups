![image](https://github.com/user-attachments/assets/2ff3f86c-675f-4b12-bc87-98a89ee66ad5)#  🛡️TryHackMe NetSec Challenge Writeup

            Platform: TryHackMe

            Challenge Name: U.A. High School

            Difficulty: (/Medium)

            Category: Networking / Security / Penetration Testing

            Author: [KWAT]

  ![nmap1 Screenshot](./Screenshot%20(147).png)

  ## Reconnaissance ##
  let’s run nmap and check what ports are open
  
  ![nmap1 Screenshot](./Screenshot%20(147).png)

  I discovered 2 open ports: 22 and 80,. Since port 80 is open, let’s check the website.

  ![nmap1 Screenshot](./Screenshot%20(147).png)


It is just a normal high school website.

So let’s check for any other hidden directories using dirsearch.


![nmap1 Screenshot](./Screenshot%20(147).png)

![nmap1 Screenshot](./Screenshot%20(147).png)

I found that there is a hidden directory (/assets). Let’s check /assets.

![nmap1 Screenshot](./Screenshot%20(147).png)

The directory is simply a blank page. Let’s proceed further by finding the subdirectory.

![nmap1 Screenshot](./Screenshot%20(147).png)

![nmap1 Screenshot](./Screenshot%20(147).png)

Let’s begin the command injection

![nmap1 Screenshot](./Screenshot%20(147).png)

 Oh, I got the base64. Let’s crack it using cyberchef.

 ![nmap1 Screenshot](./Screenshot%20(147).png)

 since the command injection works this is a clear picture of a reverse shell lets do that without wasting anytime.

 Go to https://www.revshells.com/ and generate the reverse shell as below.

![nmap1 Screenshot](./Screenshot%20(147).png)

Click on copy and then paste it into the URL after cmd = 


![nmap1 Screenshot](./Screenshot%20(147).png)

make sure to enter your machine ip not the eth0 but tun0 for tryhackme and port save as of listener

on the attacker machine means our machine we have to start a listener

                                                    nc -lvnp 4444

 ![nmap1 Screenshot](./Screenshot%20(147).png)

 Wow, I received the connection.

  ![nmap1 Screenshot](./Screenshot%20(147).png)

Run command: python3 -c ‘import pty;pty.spawn(“/bin/bash”)’ to use /bin/bash

Inside the images directory, there are 2 images.

Let’s Transfer these files from the Victim’s machine to the attacker’s system using Netcat.

  ![nmap1 Screenshot](./Screenshot%20(147).png)

  ![nmap1 Screenshot](./Screenshot%20(147).png)

when you recieve the connection on listener side simply close the connection by ctrl+c and verify using ls command

you must have oneforall.jpg.look in the same directory in which you were listening

upon further inspection found that the file uses the extension .jpg but is in data format. So we are going to Change the incorrect jpg file headers.

Opening the file using Hexedit. Command: hexedit oneforall.jpg

  ![nmap1 Screenshot](./Screenshot%20(147).png)

Change the initial header to FF D8 FF E0 00 10 4A 46 49 46 00 01 01 00 00 01. This is the correct signature for the jpeg file. Save the file.

  ![nmap1 Screenshot](./Screenshot%20(147).png)

Now it is showing the correct extension for the image and I can also view the image.

  ![nmap1 Screenshot](./Screenshot%20(147).png)

Now using this file we can use stegnography to check file contents Using steghide to extract the files inside the file

                                    steghide extract -sf oneforall.jpg

its going to ask for a passphrase

  ![nmap1 Screenshot](./Screenshot%20(147).png)

after decoding it in base64 we know that the passphrase is AllmightForEver!!!

  ![nmap1 Screenshot](./Screenshot%20(147).png)

 wow, finally I got the password for Deku.

Using the password that I got, I can easily log in to Duke using ssh.

  ![nmap1 Screenshot](./Screenshot%20(147).png)
  
  ![nmap1 Screenshot](./Screenshot%20(147).png)

  we have got our first flag




  ## Privilege Escalation ##

i am going to keep this straight forward.Inthe shell access i found that deku was only allowed restricted access
he could only access /opt/NewComponent/feedback.sh and it is a form

   ![nmap1 Screenshot](./Screenshot%20(147).png)

Deku can give feedback. Let’s try to give malicious feedback. The malicious feedback is ‘Deku ALL=NOPASSWD: ALL » /etc/sudoers’

 ![nmap1 Screenshot](./Screenshot%20(147).png)

It works and now I got the root access.


 ![nmap1 Screenshot](./Screenshot%20(147).png)


 ## Flag Captured ##
 
              user.txt flag: THM{W3lC0m3_D3kU_1A_0n3f0rAll??}

              root.txt flag: THM{Y0U_4r3_7h3_NUm83r_1_H3r0}




  

    









 




  
  
