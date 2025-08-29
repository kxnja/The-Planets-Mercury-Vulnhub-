# Walkthrough on Mercury CTF by Vulnhub

## Tools used
- Netdiscover
- Nmap
- Gobuster
- Sqlmap



## Step 1 Enumeration
Using netdiscover we can find the IP of the mercury machine.
```
sudo netdiscover
```
It outputs the following;
<img width="636" height="187" alt="image" src="https://github.com/user-attachments/assets/ac1e0f17-4ae2-418a-b50e-77eb48175e06" />
The target machine's IP is 10.38.1.112.

Now we can use Nmap to find the open ports.
```
(kali kali)-[~]
$ nmap -sC -sV 10.38.1.112
```
We find two of them, port 22 for ssh and port 80 for http which also tells us that it is a web server.

Using your preferred browser type in the IP address in the search box (http://10.38.1.112:8080).

<img width="955" height="888" alt="image" src="https://github.com/user-attachments/assets/c3113911-9775-4705-9473-9024388bfea5" />

We can find hidden pages in the web application using gobuster as follows;
```
(kali@ kali)-[~]
$ gobuster dir -u http://10.38.1.112:8080/ -w /usr/share/wordlists/dirb/common.txt
```
#### Expalnation

`gobuster` → A tool used for brute-forcing directories and files on web servers.

`dir `→ Tells Gobuster to use directory enumeration mode (looks for hidden folders and files).

`-u http://10.38.1.112:8080/` → The target URL to scan.

`10.38.1.112` is the IP address.

`8080` is the port (custom HTTP service, not the default 80).

`-w /usr/share/wordlists/dirb/common.txt` → The wordlist Gobuster will use to guess directory and file names.

The file `(common.txt)` contains common directory names like admin, login, images etc.

<img width="675" height="366" alt="image" src="https://github.com/user-attachments/assets/e619e50d-5dda-482f-8d9d-89080991141d" />

We have found a hidden file called robots.txt and we can access it by going back to the browser and searching http://10.38.1.112:8080/robots.txt

This takes us to an error page and we can see other sub domains that are under the main domain.

<img width="955" height="889" alt="image" src="https://github.com/user-attachments/assets/51a9f23a-cfa8-4ebc-993d-bc7e19972db7" />

We have found a page called mercury facts and when we go to the page using http://10.38.1.112:8080/mercuryfacts/ we can find some links that we can explore.

<img width="955" height="887" alt="image" src="https://github.com/user-attachments/assets/574fea11-e62b-4098-a8e8-c49a4f0c758f" />

The load a fact link takes you to another tab with a fact about mercury, there are several indexed from 1 to 8.
Using a tool called sqlmap we can scan for any databases on the machine. We can use it a follows;
```
(kali@ kali)-[~]
$ sqlmap -u http://10.38.1.112:8080/mercuryfacts/ -- dbs -- batch
```
#### Explanation
`sqlmap` → A tool for automating SQL injection detection and exploitation.

`-u http://10.38.1.112:8080/mercuryfacts/` → The target URL to test for SQL injection vulnerabilities.

`--dbs` → Tells sqlmap to enumerate all databases if the site is vulnerable.

`--batch` → Runs in non-interactive mode (accepts all default options automatically, no prompts).

<img width="955" height="888" alt="VirtualBox_kali-linux-2025 1c-virtualbox-amd64_29_08_2025_14_18_59" src="https://github.com/user-attachments/assets/a1f15849-7675-490e-9ca6-c77216fd6faa" />

We have found 2 databases ; information_schema and mercury.

We have no use for information_schema since it is a standard database for all websites.
So we will jump right into mercury and try to find what it is all about.
We will still use sqlmap as follows;



