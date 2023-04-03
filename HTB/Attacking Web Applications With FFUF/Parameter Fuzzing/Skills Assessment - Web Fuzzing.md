You are given an online academy's IP address but have no further information about their website. As the first step of conducting a Penetration Test, you are expected to locate all pages and domains linked to their IP to enumerate the IP and domains properly.

Finally, you should do some fuzzing on pages you identify to see if any of them has any parameters that can be interacted with. If you do find active parameters, see if you can retrieve any data from them.

Run a sub-domain/vhost fuzzing scan on '*.academy.htb' for the IP shown above. What are all the sub-domains you can identify? (Only write the sub-domain name)

`test faculty archive`
* run `sudo nano /etc/hosts` command
* on `others` section add `<ip given> academy.htb` and save
* run command `ffuf -w /usr/share/seclists/Discovery/DNS/subdomains-top1million-5000.txt:FUZZ -u http://academy.htb:<port given>/ -H 'Host: FUZZ.academy.htb'`, you will receive a bunch of responses which will need to be filtered by size, on my case its 985
* run command again as `ffuf -w /usr/share/seclists/Discovery/DNS/subdomains-top1million-5000.txt:FUZZ -u http://academy.htb:<port given>/ -H 'Host: FUZZ.academy.htb' -fs 985`, the subdomains found are test, faculty and archive

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

`http://faculty.academy.htb:PORT/courses/linux-security.php7`
* run command `ffuf -w /usr/share/seclists/Discovery/Web-Content/directory-list-lowercase-2.3-small.txt:FUZZ -u http://faculty.academy.htb:<port given>/FUZZ -recursion -recursion-depth 1 -e .php,.phps,.php7 -v` , make sure the extensions are all separated by a comma, if not then the script will look for matches only with the first extension written
* as the fuzzing might be exstensive, at least we got a hint of the subdomain `faculty` having an additional web extension `php7`, so look for the page on the courses folder under `faculty`
* running the command again on courses will display a lot of matches, try filtering the pages by the size again as previously done
* run command `ffuf -w /usr/share/seclists/Discovery/Web-Content/directory-list-lowercase-2.3-small.txt:FUZZ -u http://faculty.academy.htb:<port given>/courses/FUZZ -recursion -recursion-depth 1 -e .php,.phps,.php7 -v`, patienly wait for the proper web to be displayed, the new match is  `linux-security.php7` so look for it on firefox to get the answer

In the page from the previous question, you should be able to find multiple parameters that are accepted by the page. What are they?

`user username`
* run command `ffuf -w /usr/share/seclists/Discovery/Web-Content/burp-parameter-names.txt:FUZZ -u http://faculty.academy.htb:<port given>/courses/linux-security.php7?FUZZ=key`, this will do a default GET request parameter fuzzing, there will be several results which can be filtered by size again
* run command `ffuf -w /usr/share/seclists/Discovery/Web-Content/burp-parameter-names.txt:FUZZ -u http://faculty.academy.htb:<port given>/courses/linux-security.php7?FUZZ=key -fs <size>`, the result will be `user`
* now run command `ffuf -w /usr/share/seclists/Discovery/Web-Content/burp-parameter-names.txt:FUZZ -u http://faculty.academy.htb:<port given>/courses/linux-security.php7 -X POST -d 'FUZZ=key' -H 'Content-Type: application/x-www-form-urlencoded'`, this time we will get the results from a POST request, there will be several results which can be filtered by size again
* run command `ffuf -w /usr/share/seclists/Discovery/Web-Content/burp-parameter-names.txt:FUZZ -u http://faculty.academy.htb:<port given>/courses/linux-security.php7 -X POST -d 'FUZZ=key' -H 'Content-Type: application/x-www-form-urlencoded' -fs <size>`, the results will be user and username

Try fuzzing the parameters you identified for working values. One of them should return a flag. What is the content of the flag?

`HTB{w3b_fuzz1n6_m4573r}`
* look for a big parameter file with usernames or common file names, in ym case i used `directory-list-lowercase-2.3-big.txt`
* run command `ffuf -w directory-list-lowercase-2.3-big.txt:FUZZ -u http://faculty.academy.htb:<port given>/courses/linux-security.php7 -X POST -d 'username=FUZZ' -H 'Content-Type: application/x-www-form-urlencoded' -fs <size>` making sure the payload is fuzzing on the username parameter, i also added a size filter, the only result will be `harry`
* run command `curl http://faculty.academy.htb:<port given>/courses/linux-security.php7 -X POST -d 'username=harry' -H 'Content-Type: application/x-www-form-urlencoded'` using `harry` as the value on the `username` parameter, the flag will then be retrieved