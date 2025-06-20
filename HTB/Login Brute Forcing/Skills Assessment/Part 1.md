The first part of the skills assessment will require you to brute-force the the target instance. Successfully finding the correct login will provide you with the username you will need to start Skills Assessment Part 2.

You might find the following wordlists helpful in this engagement: [usernames.txt](https://github.com/danielmiessler/SecLists/blob/master/Usernames/top-usernames-shortlist.txt) and [passwords.txt](https://github.com/danielmiessler/SecLists/blob/master/Passwords/Common-Credentials/2023-200_most_used_passwords.txt)




#### Questions

What is the password for the basic auth login?

* Run command " hydra -L top-usernames-shortlist.txt -P 2023-200_most_used_passwords.txt \<ip given> -s \<port given> -f http-get ", the result will give us " admin " as the username and " Admin123 " as the password
* Admin123

After successfully brute forcing the login, what is the username you have been given for the next part of the skills assessment?

* Access the webpage given by HTB and submit both username and password found on the previous question
* satwossh
