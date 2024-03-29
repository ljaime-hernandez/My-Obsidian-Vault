Now let's get practical. A company hired us to test their IT security defenses, including their `IDS` and `IPS` systems. Our client wants to increase their IT security and will, therefore, make specific improvements to their `IDS/IPS` systems after each successful test. We do not know, however, according to which guidelines these changes will be made. Our goal is to find out specific information from the given situations.

We are only ever provided with a machine protected by `IDS/IPS` systems and can be tested. For learning purposes and to get a feel for how `IDS/IPS` can behave, we have access to a status web page at:

`http://<target>/status.php`

This page shows us the number of alerts. We know that if we receive a specific amount of alerts, we will be `banned`. Therefore we have to test the target system as `quietly` as possible.



Our client wants to know if we can identify which operating system their provided machine is running on. Submit the OS name as the answer.

`Ubuntu`
* run command `sudo nmap <ip given> -sV` to get service versions, the answer can be found at `port 22` 
