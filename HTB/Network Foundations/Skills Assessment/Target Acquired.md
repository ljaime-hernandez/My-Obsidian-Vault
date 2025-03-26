For our final chapter, we will be focusing on the target machine's port `21` and port `80` - used for `FTP (File Transfer Protocol)` and `HTTP (Hyper-Text Transport Protocol.)` Let's perform another `nmap` scan, this time focused only on the two aforementioned ports.

Code: shell

```shell
nmap -p21,80 -sC -sV <target ip>
```

By adding the `-sC` and `-sV` options to our scan, this will allow nmap to determine the version of whatever program is listening on a given port, as well as additional information (such as how a particular service might be configured.

Target Acquired

```shell-session
┌─[eu-academy-1]─[10.10.14.21]─[htb-ac-594497@htb-5mix2gkv1a]─[~]
└──╼ [★]$ nmap -p21,80 -sC -sV 10.129.233.197
 
Starting Nmap 7.94SVN ( https://nmap.org ) at 2025-02-08 19:17 CST
Nmap scan report for 10.129.233.197
Host is up (0.071s latency).

PORT   STATE SERVICE VERSION
21/tcp open  ftp     Microsoft ftpd
| ftp-syst: 
|_  SYST: Windows_NT
|_ftp-anon: Anonymous FTP login allowed (FTP code 230) 
80/tcp open  http    Microsoft HTTPAPI httpd 2.0 (SSDP/UPnP)
Service Info: OS: Windows; CPE: cpe:/o:microsoft:windows

Service detection performed. Please report any incorrect results at https://nmap.org/submit/ .
Nmap done: 1 IP address (1 host up) scanned in 11.05 seconds
```

Nmap tells us that the FTP service has `Anonymous FTP` enabled, meaning that anyone is able to connect to the FTP service (typically by providing the username `anonymous`). However, the HTTP service on port 80 does not return as much information. Generally, nmap will be able to fingerprint the webserver software in place (such as `Apache`, `Nginx`, or `IIS`). Our initial nmap scan operates at layer 4 of the OSI model --- that is, determining if the port is open or closed at the TCP/UDP level. For things such as the version scan (-sV) or default scripts (-sC), nmap uses protocol-specific layer 7 packets. Because port 80 showed no additional information after our second scan, we can hypothesize that there is some sort of `request filtering` in place. We will touch on this more in a bit.

IMPORTANT: While it is not illegal, it is considered unethical to use nmap (or any port scanning utility) against a device that you do not either own, or have explicit permission to be scanning.

Let's take a look at the FTP service running on port 21. To connect, we will use [netcat](https://en.wikipedia.org/wiki/Netcat), a utility for making raw TCP/UDP connections. After we connect with `netcat`, whatever we characters we type in will be transmitted to the port in which we are connected. Rather than using a specific `FTP client` utility, we will use netcat to better understand how the FTP protocol works. On the flipside, for the HTTP service, software such as a web browser (or the `curl` and `wget` command line tools) formulate and send the protocol specific data for us. Whenever you are ready, enter the following commend into your terminal and press enter.

Code: shell

```shell
nc <target ip> 21
```

Here, we are telling netcat to connect to port 21 on the target machine. Once connected, we are greeted with a banner from the FTP service, indicating we have successfully connected.

Target Acquired

```shell-session
┌─[eu-academy-1]─[10.10.14.21]─[htb-ac-594497@htb-5mix2gkv1a]─[~]
└──╼ [★]$ nc 10.129.233.197 21
  
220 Microsoft FTP Service
```

Next, we will login as an `anonymous user` with the commands shown below, and then specify we will be using [Passive Mode](https://www.geeksforgeeks.org/difference-between-active-and-passive-ftp/):

Code: shell

```shell
USER anonymous[Ctrl+V][Enter][Enter]
PASS anything[Ctrl+V][Enter][Enter]
PASV[Ctrl+V][Enter][Enter]
```

The reason we provided the `[Ctrl + V] [Enter] [Enter]` is because FTP requires a `return character and new-line character (\r\n)` for its commands. When we press Enter on our keyboard, Netcat only sends the `\n`.

Target Acquired

```shell-session
┌─[eu-academy-1]─[10.10.14.21]─[htb-ac-594497@htb-5mix2gkv1a]─[~]
└──╼ [★]$ nc 10.129.233.197 21
  
220 Microsoft FTP Service
USER anonymous^M
331 Anonymous access allowed, send identity (e-mail name) as password.
PASS anything^M
230 User logged in.
PASV^M
227 Entering Passive Mode (10,129,233,197,194,40).
```

FTP (File Transfer Protocol) uses two separate channels to function.

| **Channel**     | **Purpose**                                       | **Port**                                          |
| --------------- | ------------------------------------------------- | ------------------------------------------------- |
| Control Channel | Sends FTP commands (USER, PASS, LIST, RETR, etc.) | Port 21                                           |
| Data Channel    | Transfers files and directory listings            | Dynamic Port (Varies by mode: Active or Passive). |

Here, we have selected passive mode, and subsequently need to re-connect to the FTP server on another port. To determine the port number, we need to do some calculations. This is mainly due to limitations in the FTP protocol, as the full port cannot be displayed at once, so it is instead split to 2 small number 'p1 and p2; the last 2 numbers in the above output'. Then the real port is calculated as 'p1*256 + p2'. See [this documentation](http://www.faqs.org/rfcs/rfc959.html) for more info.

So, to calculate the port number, we will use the last two numbers shown (in the example above, they are `194` and `40`). We will take the first number and multiply it by `256`, then add the second number.

`194` * `256` + `40` = `49704`

Let's open a new `Parrot Terminal` by clicking `File` (located in the top left hand corner of your current Parrot Terminal), then select `Open Terminal` -> `Bash`. Then, in our new Terminal, we will use netcat to connect to the `data channel`.

Code: shell

```shell
nc -v <target ip> <dynamic port>
```

  Target Acquired

```shell-session
┌─[eu-academy-1]─[10.10.14.21]─[htb-ac-594497@htb-5mix2gkv1a]─[~]
└──╼ [★]$ nc -v 10.129.233.197 49704
  
10.129.233.197: inverse host lookup failed: Unknown host
(UNKNOWN) [10.129.233.197] 49704 (?) open
```

Now, let's return to our first Terminal and use the `connection channel` to list the available files in the FTP share. Enter the following command:

Code: shell

```shell
LIST[Ctrl+V][Enter][Enter]
```

We should see a message indicating that a data connection is already open, and therefore a transfer is starting.

  Target Acquired

```shell-session
┌─[eu-academy-1]─[10.10.14.21]─[htb-ac-594497@htb-5mix2gkv1a]─[~]
└──╼ [★]$ nc 10.129.233.197 21
  
220 Microsoft FTP Service
USER anonymous^M
331 Anonymous access allowed, send identity (e-mail name) as password.
PASS anything^M
230 User logged in.
PASV^M
227 Entering Passive Mode (10,129,233,197,194,40).
LIST^M
125 Data connection already open; Transfer starting.
226 Transfer complete.
```

When we check back on our other Terminal, we will see a list of the files available in the share!

  Target Acquired

```shell-session
┌─[eu-academy-1]─[10.10.14.21]─[htb-ac-594497@htb-5mix2gkv1a]─[~]
└──╼ [★]$ nc -v 10.129.233.197 49704
  
10.129.233.197: inverse host lookup failed: Unknown host
(UNKNOWN) [10.129.233.197] 49704 (?) open
02-08-25  08:37PM                  438 Note-From-IT.txt
```

We see that there is a `Note-From-IT.txt` text file available for us to read. To retrieve the file, we must once again enter the following command in our `connection channel`, then use netcat to establish connection to a new `data channel`.

Code: shell

```shell
RETR[Ctrl + V][Enter][Enter]
```

The output will be similar to when we ran the command the first time.

Target Acquired

```shell-session
┌─[eu-academy-1]─[10.10.14.21]─[htb-ac-594497@htb-5mix2gkv1a]─[~]
└──╼ [★]$ nc 10.129.233.197 21
  
220 Microsoft FTP Service
USER anonymous^M
331 Anonymous access allowed, send identity (e-mail name) as password.
PASS anything^M
230 User logged in.
PASV^M
227 Entering Passive Mode (10,129,233,197,194,40).
LIST^M
125 Data connection already open; Transfer starting.
226 Transfer complete.
PASV^M
227 Entering Passive Mode (10,129,233,197,194,50).
```

Again, we must calculate the port number, and use `netcat` to make the connection.

`194` * `256` + `50` = `49714`

  Target Acquired

```shell-session
┌─[eu-academy-1]─[10.10.14.21]─[htb-ac-594497@htb-5mix2gkv1a]─[~]
└──╼ [★]$ nc -v 10.129.233.197 49714
  
10.129.233.197: inverse host lookup failed: Unknown host
(UNKNOWN) [10.129.233.197] 49714 (?) open
```

Sending one final command, we can retrieve the `Note-From-IT.txt` text file.

Code: shell

```shell
RETR Note-From-IT.txt[Ctrl+V][Enter][Enter]
```

  Target Acquired

```shell-session
┌─[eu-academy-1]─[10.10.14.21]─[htb-ac-594497@htb-5mix2gkv1a]─[~]
└──╼ [★]$ nc 10.129.233.197 21
  
220 Microsoft FTP Service
USER anonymous^M
331 Anonymous access allowed, send identity (e-mail name) as password.
PASS anything^M
230 User logged in.
PASV^M
227 Entering Passive Mode (10,129,233,197,194,40).
LIST^M
125 Data connection already open; Transfer starting.
226 Transfer complete.
PASV^M
227 Entering Passive Mode (10,129,233,197,194,50).
RETR Note-From-IT.txt^M
125 Data connection already open; Transfer starting.
226 Transfer complete.
```

When we check the data channel, we are greeted with the contents of the note.

  Target Acquired

```shell-session
┌─[eu-academy-1]─[10.10.14.21]─[htb-ac-594497@htb-5mix2gkv1a]─[~]
└──╼ [★]$ nc -v 10.129.233.197 49714
  
10.129.233.197: inverse host lookup failed: Unknown host
(UNKNOWN) [10.129.233.197] 49714 (?) open
Bertolis,

The website is still under construction. To stop users from poking their nose where it doesn't belong, I've configured IIS to only allow requests containing a specific user-agent header. If you'd like to test it out, please provide the following header to your HTTP request.

User-Agent: Server Administrator

The site should be finished within the next couple of weeks. I'll keep you posted.

Cheers,
jarednexgent
```

The note from the IT team has some revealing information. It turns out, the web server is configured to only allow requests that provide a specific user-agent header. We have seen how the FTP protocol recognizes commands such as `USER`, `PASS`, `PASV`, `LIST`, and `RETR`. The HTTP protocol, on the other hand, utilizes a system of requests and responses consisting of [HTTP headers](https://en.wikipedia.org/wiki/List_of_HTTP_header_fields). By providing the header `User-Agent: Server Administrator`, we should be able to view the website that is being served via port 80.

Let us make our initial TCP connection with netcat using the command shown below:

Code: shell

```shell
nc -v <target ip> 80
```

We are met with a message indicating that the port is open.

  Target Acquired

```shell-session
┌─[eu-academy-1]─[10.10.14.21]─[htb-ac-594497@htb-5mix2gkv1a]─[~]
└──╼ [★]$ nc -v 10.129.233.197 80
  
10.129.233.197: inverse host lookup failed: Unknown host
(UNKNOWN) [10.129.233.197] 80 (http) open
```

Now, we can formulate our request. Enter the following into your netcat session:

Code: shell

```shell
GET / HTTP/1.1[enter]
Host: <target ip>[enter]
User-Agent: Server Administrator[enter][enter]
```

The web server responds! Unlike when we use our web browser (which makes the HTTP request for us automatically, then renders the HTML into an aesthetic, readable website), with netcat the contents of the webpage are printed to our terminal. We can also find a flag hidden in the comments of the HTML, which would not be visibile if we were to use our web browser.

  Target Acquired

```shell-session
┌─[eu-academy-1]─[10.10.14.21]─[htb-ac-594497@htb-5mix2gkv1a]─[~]
└──╼ [★]$ nc -v 10.129.233.197 80
 
10.129.233.197: inverse host lookup failed: Unknown host
(UNKNOWN) [10.129.233.197] 80 (http) open
GET / HTTP/1.1
Host: 10.129.233.197
User-Agent: Server Administrator

HTTP/1.1 200 OK
Content-Type: text/html
Accept-Ranges: bytes
ETag: "5acd7854a179db1:0"
Server: Microsoft-IIS/10.0
Date: Tue, 5 Feb 2025 00:44:43 GMT
Content-Length: 746

<!DOCTYPE html PUBLIC "-//W3C//DTD XHTML 1.0 Strict//EN" "http://www.w3.org/TR/xhtml1/DTD/xhtml1-strict.dtd">
<html xmlns="http://www.w3.org/1999/xhtml">
<head>
<meta http-equiv="Content-Type" content="text/html; charset=iso-8859-1" />
<title>IIS Windows Server</title>
<style type="text/css">
<!--
body {
	color:#000000;
	background-color:#0072C6;
	margin:0;
}

#container {
	margin-left:auto;
	margin-right:auto;
	text-align:center;
	}

a img {
	border:none;
}

-->
</style>
</head>
<body>
<div id="container">
<a href="http://go.microsoft.com/fwlink/?linkid=66138&amp;clcid=0x409"><img src="iisstart.png" alt="IIS" width="960" height="600" /></a>
</div>
</body>
</html>
<!-- HTB{REDACTED} -->
```

`GET /` is an HTTP request that tells a web server "Give me the homepage (root directory) of this website." If there were a login page we wanted to access, our request might look like `GET /login.php`. The `Host` header tells the server which host we are requesting (it is possible for a server to host multiple, unique webpages all on the same server). The `User Agent` header is used to indicate the agent making the web request --- for example, if it's a browser making the request, the user agent will typically be the name and version of the browser.

The server, in turn, replies with it's own headers. For example, the `Content-Type` header tells us what type of data the server is replying with. The `Accept` header tells us what type of data it is able to receive, and the `Server` header tells us that the web server software in place is Microsoft IIS. Below these headers is the HTML of the site we are attempting to access.

We have now seen first hand how the FTP and HTTP protocols work. By sending specific packets of data, we adhere to the `protocol` (or "language") that a particular service speaks. We have seen how FTP utilizes both a data channel and connection channel, and how it's protocol requires new-line and return characters be submitted. Conversely, we have seen the HTTP protocol be more forgiving, while at the same time displaying merciless intolerance for requests that do not adhere to the specifications put in place (i.e., the `user-agent` header request filtering).

With that, we conclude chapter three. You should now be able to complete all of the challenge questions and finish the assessment.



What IPv4 address is used when a host wants to send and receive network traffic to itself?

+ 127.0.0.1

What is the the name of the Program listening on localhost:5901 of the Pwnbox?

+ Xtigervnc

Which network interface allows us to interact with target machines in the HTB lab environment?

+ tun0

What is the FTP command used to retrieve a file? (Format: XXXX)

+ RETR

Bypass the request filtering found on the target machine's HTTP service, and submit the flag found in the response. The flag will be in the format: HTB{...}

+ HTB{S00n_2_B_N3tw0rk1ng_GURU!}


