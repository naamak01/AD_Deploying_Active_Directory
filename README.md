![image](https://github.com/user-attachments/assets/71dc699e-4564-4e32-9a99-be54d167b1b1)
 
# Deploying Active Directory in Azure

## Description

For this portion of the Active Directory project, we will install Active Directory on the domain controller, create a domain admin, and join the client VM to the domain.

This project is a continuation of [[Active Directory: Preparing Infrastructure in Azure]](https://github.com/naamak01/AD_Preparing_Active_Directory_In_Azure), so this project picks up where I left off.


Environments and Utilities Used
* Microsoft Azure
* Virtual Machines
* Remote Desktop Connection
* Active Directory

## Operating Systems Used
* Windows Server
* Windows 10

Project Walk-through:

To start, in the DC (Domain Controller), bring up the Server Manager dashboard, if it's not up already:

![image](https://github.com/user-attachments/assets/5a0346e4-5120-48bd-bf19-c70e1ae38d4a)

Here, click on "Add roles and features" > Next > Next > For server selection there should only be one to select:

![image](https://github.com/user-attachments/assets/c7af1baf-4922-4ef9-a548-cbed27ac7c0a)

Now, check "Active Directory Domain Services" > Click "Add Features":

![image](https://github.com/user-attachments/assets/ce33d25f-9789-4536-bd82-f3f7df8a2848)

Select "Active Directory Domain Services" > Next > Next > Next > Now select "Restart the destination server automatically if required" and click "Install":

![image](https://github.com/user-attachments/assets/3797d8f1-b4ee-4516-95a1-1f3c598cfec3)

Next, I have to promote this machine as a DC by configuring it as a new forest as "mydomain.com" (this can be anything, mydomain.com is just easy to remember) So, to do this, I will click on the flag icon with the caution symbol, near the top-right of the Server Manager Dashboard and select "Promote this server to a domain controller":

![image](https://github.com/user-attachments/assets/25353917-6b1d-4e2b-b124-349604ca7092)

Then select "Add new forest" and in "Root domain name" I'll type "mydomain.com". Then "Next":

![image](https://github.com/user-attachments/assets/a1fd8ba5-7755-4197-bf21-c5f9ce154299)

Type in a password of your choosing on the next screen like so then hit "Next":

![image](https://github.com/user-attachments/assets/9b0b4c0c-f38a-4d48-a46f-c1ff760c967d)

Leave "Create DNS delegation" unchecked and click "Next". Hit "Next" until you reach this screen. On this screen, click "Install". The machine will restart after the installation:

![image](https://github.com/user-attachments/assets/0a9f1b20-691a-4ffc-b8af-156f43ecd5d0)

It may take some time to restart, but once it does, sign back in using "mydomain.com\labuser" (if you're using the same names as me) as the username. Use the password you set for "labuser". Here we have to sign in as "mydomain.com\labuser" instead of just "labuser", because now we've promoted this machine to a DC and it needs to know the context of who we're siging in as (someone in the domain, or a local user). I want to sign in as a user on the domain, so I specify that with the leading "mydomain.com\" part at sign in:

Now, we'll create an admin user. To do this, in the start search bar, search for "Active Directory Users and Computers":

![image](https://github.com/user-attachments/assets/f1c6e882-a419-4320-baff-585915b96ba4)

Right-click on "mydomain.com" and select "New" > "Organizational Unit" and name it EXACTLY "_EMPLOYEES" (In a future project we will run a script that uses this name to work). Do the same steps here to create an OU (Organizational Unit) named "_ADMINS":

![image](https://github.com/user-attachments/assets/ce1b8344-8b6b-400c-ae73-9c8a0e53193a)

Next, create a new user in "_ADMINS" by right-clicking on "_ADMINS" > New > User and fill it out like so:

![image](https://github.com/user-attachments/assets/df8093d6-8f2f-4509-8c60-6236b31406c8)

Then, create a password and uncheck "User must change password at next logon" and check "Password never expires" (You probably wouldn't do this in real life, but we'll do it for this lab where nothing's really at stake):

![image](https://github.com/user-attachments/assets/b4519af4-13d9-4cae-a417-85e41de09caa)

Jane Doe is now apart of the "_ADMINS" OU, but isn't actually an admin. To make her an admin right-click on her name > Properties > Member Of > Add... > Enter "domain admins" > Check Names > OK > Apply > OK:

![image](https://github.com/user-attachments/assets/a8e35919-2a55-4827-821e-bcc3018b3438)

Now, we can log out of DC and log back in using Jane's credentials:

Which is:

**mydomain.com\jane_admin and then password 

Once logged in, log into client-1 VM, if not already. Here I'll join this client to the domain by right-clicking the start menu > Systems > Rename this PC (advanced) > Change > Select "Domain" and enter "mydomain.com" and hit "OK":

![image](https://github.com/user-attachments/assets/5f1ba0b3-98ce-452c-b7e7-9ce1eafc8459)

It'll ask for an account with permission to join the domain. We can use our admin's credentials for Jane here. Then a pop-up saying Welcome to the domain will appear and the machine will try and restart:

![image](https://github.com/user-attachments/assets/607080b9-0a91-45aa-8eb6-2cbbbc9b3a8c)

After the restart, client-1 is now a member of the domain. To check, in the DC machine start search bar, search for "Active Directory Users and Computers" > mydomain.com > Computers > client-1 should be listed:

Successfully joined

![image](https://github.com/user-attachments/assets/e7a4e3f3-55fa-4699-9296-3faa31928b21)

Now we verify if client-1 was successfully added as user to DC-1

![image](https://github.com/user-attachments/assets/c2ac0e6a-c292-4fe0-8d16-7b554b3d430b)

Now we can create another OU as we did before and name it "_CLIENTS". Then drag and drop client-1 from "computers" to "_CLIENTS":

![image](https://github.com/user-attachments/assets/d10f6d09-7b44-48aa-afdd-d3d01f95e3d3)

## Active Directory is Deployed and Ready for Use!

**We've successfully installed AD on the domain controller, created a domain admin, and joined the client VM to the domain. Next, we'll create users in Powershell by running a script, and then manage the accounts and group policy!**







