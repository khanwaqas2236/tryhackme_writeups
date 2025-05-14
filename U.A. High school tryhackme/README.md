#  🛡️TryHackMe NetSec Challenge Writeup

            Platform: TryHackMe

            Challenge Name: U.A. High School

            Difficulty: (/Medium)

            Category: Networking / Security / Penetration Testing

            Author: [KWAT]

            

  ![hg1 Screenshot](./Screenshot%20(193).png)

  

  ## Reconnaissance ##

  
  let’s run nmap and check what ports are open

  
  
  
  ![hg1 Screenshot](./Screenshot%20(194).png)


  

  I discovered 2 open ports: 22 and 80,. Since port 80 is open, let’s check the website.

  

  
  ![hg1 Screenshot](./Screenshot%20(195).png)
  


It is just a normal high school website.




So let’s check for any other hidden directories using dirsearch.



  ![hg1 Screenshot](./Screenshot%20(197).png)

  


  ![hg1 Screenshot](./Screenshot%20(198).png)

  

I found that there is a hidden directory (/assets). Let’s check /assets.




  ![hg1 Screenshot](./Screenshot%20(199).png)

  

The directory is simply a blank page. Let’s proceed further by finding the subdirectory.



  ![hg1 Screenshot](./Screenshot%20(200).png)


  ![hg1 Screenshot](./Screenshot%20(201).png)

  

Let’s begin the command injection



  ![hg1 Screenshot](./Screenshot%20(202).png)

  

 Oh, I got the base64. Let’s crack it using cyberchef.
 

 
  ![hg1 Screenshot](./Screenshot%20(203).png)

  

 since the command injection works this is a clear picture of a reverse shell lets do that without wasting anytime.
 

 Go to https://www.revshells.com/ and generate the reverse shell as below.


 
 ![hg1 Screenshot](./Screenshot%20(204).png)

  


Click on copy and then paste it into the URL after cmd = 



![hg1 Screenshot](./Screenshot%20(206).png)

  

make sure to enter your machine ip not the eth0 but tun0 for tryhackme and port save as of listener



on the attacker machine means our machine we have to start a listener



                                                    nc -lvnp 4444
                                                    

 
  ![hg1 Screenshot](./Screenshot%20(205).png)

  

 Wow, I received the connection.

 

  
  ![hg1 Screenshot](./Screenshot%20(207).png)

  

Run command: python3 -c ‘import pty;pty.spawn(“/bin/bash”)’ to use /bin/bash



Inside the images directory, there are 2 images.



Let’s Transfer these files from the Victim’s machine to the attacker’s system using Netcat.


  
  ![hg1 Screenshot](./Screenshot%20(208).png)
  

  
  ![hg1 Screenshot](./Screenshot%20(209).png)

  

when you recieve the connection on listener side simply close the connection by ctrl+c and verify using ls command


you must have oneforall.jpg.look in the same directory in which you were listening


upon further inspection found that the file uses the extension .jpg but is in data format. So we are going to Change the 

incorrect jpg file headers.



Opening the file using Hexedit. Command: hexedit oneforall.jpg



  
![hg1 Screenshot](./Screenshot%20(210).png)

  

Change the initial header to FF D8 FF E0 00 10 4A 46 49 46 00 01 01 00 00 01. This is the correct signature for the jpeg 


file. Save the file.



![hg1 Screenshot](./Screenshot%20(211).png)

    

Now it is showing the correct extension for the image and I can also view the image.



 ![hg1 Screenshot](./Screenshot%20(212).png)

    

Now using this file we can use stegnography to check file contents Using steghide to extract the files inside the file



                                    steghide extract -sf oneforall.jpg

                                    

its going to ask for a passphrase



![hg1 Screenshot](./Screenshot%20(213).png)

    

after decoding it in base64 we know that the passphrase is AllmightForEver!!!



![hg1 Screenshot](./Screenshot%20(214).png)

    

 wow, finally I got the password for Deku.
 

Using the password that I got, I can easily log in to Duke using ssh.


 ![hg1 Screenshot](./Screenshot%20(215).png)
 
  
![hg1 Screenshot](./Screenshot%20(216).png)

    

  we have got our first flag




  ## Privilege Escalation ##

  

i am going to keep this straight forward.Inthe shell access i found that deku was only allowed restricted access

he could only access /opt/NewComponent/feedback.sh and it is a form



 ![hg1 Screenshot](./Screenshot%20(217).png)

 

Deku can give feedback. Let’s try to give malicious feedback. The malicious feedback is ‘Deku ALL=NOPASSWD: ALL » /etc/sudoers’

![hg1 Screenshot](./Screenshot%20(218).png)

It works and now I got the root access.


 ![hg1 Screenshot](./Screenshot%20(219).png)


 ## Flag Captured ##
 
              user.txt flag: THM{W3lC0m3_D3kU_1A_0n3f0rAll??}

              root.txt flag: THM{Y0U_4r3_7h3_NUm83r_1_H3r0}




  

    









 




  
  
