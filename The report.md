# 1.0	High-Level Summary

GoodSecurity was tasked with performing an internal penetration test on GoodCorp’s CEO, Hans Gruber.
An internal penetration test is a dedicated attack against internally connected systems. 
The focus of this test is to perform attacks, similar to those of a hacker and attempt to infiltrate Hans’ computer and determine if it is at risk. 
GoodSecurity’s overall objective was to exploit any vulnerable software and find the secret recipe file on Hans’ computer, while reporting the findings back to GoodCorp.
When performing the internal penetration test, there were several alarming vulnerabilities that were identified on Hans’ desktop. When performing the attacks, GoodSecurity was able to gain access to his machine and find the secret recipe file by exploit two programs that had major vulnerabilities. The details of the attack can be found in the ‘Findings’ category.

# 2.0	Findings

 * Machine IP: 192.168.0.20
 * Hostname: Windows machine
 * Vulnerability Exploited: Icecast header overwrite
### Vulnerability Explanation: 
* This module exploits a buffer overflow in the header parsing of icecast versions 2.0.1 and earlier, discovered by Luigi Auriemma.  A remote user can execute arbitrary code on the target system with the privileges of the target Icecast service, and or a code via an HTTP request with many headers.  It is reported that this vulnerability is only exploitable to execute remote code on Microsoft Windows platforms. This exploit uses ExitThread(), this will leave icecast thinking the thread is still in use, and the thread counter won't be decremented. This means for each time your payload exits, the counter will be left incremented, and eventually the threadpool limit will be maxed. 
### Severity: 
* I consider the severity of this attack as high. Because there is considerable informational disclosure meaning it has impact on confidentiality and it also does have impact on integrity in that modification of some system files or information is possible, but the attacker does not have control over what can be modified, or the scope of what the attacker can affect is limited. According to The Common Vulnerability Scoring System (CVSS score) it is rated 7.5, and that is considered high. 

   ![image](https://user-images.githubusercontent.com/72705930/124705607-ef513e00-dec3-11eb-8c04-d614ba88dce4.png)


# Proof of Concept:

The following steps were taken during the internal penetration test exercise. 
 - A service and version scan were performed using Nmap command nmap -sV 192.168.0.20. where sV indicates service and version respectively and the latter is the IP address of the target machine. By running this scan, I was aware that the Icecast server was running. 
  
   ![image](https://user-images.githubusercontent.com/72705930/124705760-27f11780-dec4-11eb-982d-51b4077fce20.png)

Metasploit was then used to search and identify the exploit that can be used to against the icecast server. Icecast header is a module that exploits a buffer overflow in the header parsing of icecast versions 2.0.1 and earlier.  
#### Hence the exploit used is - exploit/windows/http/icecast_header.  and the RHOST was set as the target IP’s i.e., 192.168.0.20.  

   ![image](https://user-images.githubusercontent.com/72705930/124705880-5c64d380-dec4-11eb-98e8-4fb2f1bda12c.png)

This exploit was successful, and I was able to access Mr. Hans machine. 

   ![image](https://user-images.githubusercontent.com/72705930/124705909-68e92c00-dec4-11eb-83d5-3f9ed87a83be.png)

Since the overall objective was to exploit any vulnerable software and find the secret recipe file on Hans’ computer, a search was performed using the command search -f *recipe*txt and it output the recipe file location in the directory file path – C:\Users\IEUser\Documents\Drinks.recipe.txt.

   ![image](https://user-images.githubusercontent.com/72705930/124705926-730b2a80-dec4-11eb-96e4-c8654c4eaf02.png)


Further digging was performed to see if we could find other file text within Mr Han’s machine and a secret file text was also discovered using the search -f *secretfile* syntax. The file path is as seen in the screenshot below. 
 
   ![image](https://user-images.githubusercontent.com/72705930/124705960-83230a00-dec4-11eb-9bad-6b01af728ef7.png)
 

# 3.0	Recommendations

  ### Patching. 
  I recommend an upgrade of the version presently being used by GoodCorp. Because research shows that the vendor has released version 2.0.2 of Icecast to resolve the vulnerability.
