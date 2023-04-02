You are given an online academy's IP address but have no further information about their website. As the first step of conducting a Penetration Test, you are expected to locate all pages and domains linked to their IP to enumerate the IP and domains properly.

Finally, you should do some fuzzing on pages you identify to see if any of them has any parameters that can be interacted with. If you do find active parameters, see if you can retrieve any data from them.

Run a sub-domain/vhost fuzzing scan on '*.academy.htb' for the IP shown above. What are all the sub-domains you can identify? (Only write the sub-domain name)

`test faculty archive`
* run `sudo nano /etc/hosts` command
* on `others` section add `<ip given> academy.htb` and save
* run command `ffuf -w /usr/share/seclists/Discovery/DNS/subdomains-top1million-5000.txt:FUZZ -u http://academy.htb:<port given>/ -H 'Host: FUZZ.academy.htb'`, you will receive a bunch of responses which will need to be filtered by size, on my case its 985
* run command again as `ffuf -w /usr/share/seclists/Discovery/DNS/subdomains-top1million-5000.txt:FUZZ -u http://academy.htb:31166/ -H 'Host: FUZZ.academy.htb' -fs 985`, the subdomains found are test, faculty and archive

Before you run your page fuzzing scan, you should first run an extension fuzzing scan. What are the different extensions accepted by the domains?

`.php .phps .php7`
* run `sudo nano /etc/hosts` command to add the sub domains previously aquired on the question above
* on `others` section add `<ip given> test.academy.htb`
* on `others` section add `<ip given> archive.academy.htb`
* on `others` section add `<ip given> faculty.academy.htb` and save
* run command  `ffuf -w /usr/share/seclists/Discovery/Web-Content/web-extensions.txt:FUZZ -u http://test.academy.htb:<port given>/indexFUZZ`, to retrieve .php and .phps extensions
* run command  `ffuf -w /usr/share/seclists/Discovery/Web-Content/web-extensions.txt:FUZZ -u http://archive.academy.htb:<port given>/indexFUZZ` to retrieve .php and .phps extensions
* run command  `ffuf -w /usr/share/seclists/Discovery/Web-Content/web-extensions.txt:FUZZ -u http://faculty.academy.htb:<port given>/indexFUZZ` to retrieve .php, .phps and php7 extensions

One of the pages you will identify should say 'You don't have access!'. What is the full page URL?



In the page from the previous question, you should be able to find multiple parameters that are accepted by the page. What are they?



Try fuzzing the parameters you identified for working values. One of them should return a flag. What is the content of the flag?

