```shell-session
jhonmorales@htb[/htb]$ ffuf -w /usr/share/seclists/Discovery/Web-Content/directory-list-2.3-medium.txt -ic -v -u http://IP:PORT/FUZZ -e .html -recursion 

        /'___\  /'___\           /'___\       
       /\ \__/ /\ \__/  __  __  /\ \__/       
       \ \ ,__\\ \ ,__\/\ \/\ \ \ \ ,__\      
        \ \ \_/ \ \ \_/\ \ \_\ \ \ \ \_/      
         \ \_\   \ \_\  \ \____/  \ \_\       
          \/_/    \/_/   \/___/    \/_/       

       v2.1.0-dev
________________________________________________

 :: Method           : GET
 :: URL              : http://IP:PORT/FUZZ
 :: Wordlist         : FUZZ: /usr/share/seclists/Discovery/Web-Content/directory-list-2.3-medium.txt
 :: Extensions       : .html 
 :: Follow redirects : false
 :: Calibration      : false
 :: Timeout          : 10
 :: Threads          : 40
 :: Matcher          : Response status: 200-299,301,302,307,401,403,405,500
________________________________________________

[Status: 301, Size: 0, Words: 1, Lines: 1, Duration: 0ms]
| URL | http://IP:PORT/level1
| --> | /level1/
    * FUZZ: level1

[INFO] Adding a new job to the queue: http://IP:PORT/level1/FUZZ

[INFO] Starting queued job on target: http://IP:PORT/level1/FUZZ

[Status: 200, Size: 96, Words: 6, Lines: 2, Duration: 0ms]
| URL | http://IP:PORT/level1/index.html
    * FUZZ: index.html

[Status: 301, Size: 0, Words: 1, Lines: 1, Duration: 0ms]
| URL | http://IP:PORT/level1/level2
| --> | /level1/level2/
    * FUZZ: level2

[INFO] Adding a new job to the queue: http://IP:PORT/level1/level2/FUZZ

[Status: 301, Size: 0, Words: 1, Lines: 1, Duration: 0ms]
| URL | http://IP:PORT/level1/level3
| --> | /level1/level3/
    * FUZZ: level3

[INFO] Adding a new job to the queue: http://IP:PORT/level1/level3/FUZZ

[INFO] Starting queued job on target: http://IP:PORT/level1/level2/FUZZ

[Status: 200, Size: 96, Words: 6, Lines: 2, Duration: 0ms]
| URL | http://IP:PORT/level1/level2/index.html
    * FUZZ: index.html

[INFO] Starting queued job on target: http://IP:PORT/level1/level3/FUZZ

[Status: 200, Size: 126, Words: 8, Lines: 2, Duration: 0ms]
| URL | http://IP:PORT/level1/level3/index.html
    * FUZZ: index.html

:: Progress: [441088/441088] :: Job [4/4] :: 100000 req/sec :: Duration: [0:00:06] :: Errors: 0 ::
```

```shell-session
jhonmorales@htb[/htb]$ ffuf -w /usr/share/seclists/Discovery/Web-Content/directory-list-2.3-medium.txt -ic -u http://IP:PORT/FUZZ -e .html -recursion -recursion-depth 2 -rate 500
```

