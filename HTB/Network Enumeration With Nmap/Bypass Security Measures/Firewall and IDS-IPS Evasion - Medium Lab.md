After we conducted the first test and submitted our results to our client, the administrators made some changes and improvements to the `IDS/IPS` and firewall. We could hear that the administrators were not satisfied with their previous configurations during the meeting, and they could see that the network traffic could be filtered more strictly.




After the configurations are transferred to the system, our client wants to know if it is possible to find out our target's DNS server version. Submit the DNS server version of the target as the answer.

`HTB{GoTtgUnyze9Psw4vGjcuMpHRp}`
* update your VPN file replacing the old one you were using
* run command `sudo nmap -sUV -p 53 --script dns-nsid <ip given>`. Parameter `-sUV` will attempt a connection with UDP protocol and will retrieve the version of it as well as DNS usually resolves through port 53. Searching through the Nmap scripts webpage tab (link [here](https://nmap.org/nsedoc/scripts/)) we find the `dns-nsid` is helpful towards finding the DNS nameserver ID (or `nsid`)  which in our case does not contain it but it contains the HTB flag instead



