### GET Parameters: Openly Sharing Information

You'll often spot `GET` parameters right in the URL, following a question mark (`?`). Multiple parameters are strung together using ampersands (`&`). For example:

``` shell
https://example.com/search?query=fuzzing&category=security
```

In this URL:

- `query` is a parameter with the value "fuzzing"
- `category` is another parameter with the value "security"

## POST Parameters: Behind-the-Scenes Communication

1. `Data Collection`: The information entered into the form fields is gathered and prepared for transmission.
2. `Encoding`: This data is encoded into a specific format, typically `application/x-www-form-urlencoded` or `multipart/form-data`:
    - `application/x-www-form-urlencoded`: This format encodes the data as key-value pairs separated by ampersands (`&`), similar to GET parameters but placed within the request body instead of the URL.
    - `multipart/form-data`: This format is used when submitting files along with other data. It divides the request body into multiple parts, each containing a specific piece of data or a file.
3. `HTTP Request`: The encoded data is placed within the body of an HTTP POST request and sent to the web server.
4. `Server-Side Processing`: The server receives the POST request, decodes the data, and processes it according to the application's logic.

``` shell
POST /login HTTP/1.1 
Host: example.com 
Content-Type: application/x-www-form-urlencoded 

username=your_username&password=your_password
```

- `POST`: Indicates the HTTP method (POST).
- `/login`: Specifies the URL path where the form data is sent.
- `Content-Type`: Specifies how the data in the request body is encoded (`application/x-www-form-urlencoded` in this case).
- `Request Body`: Contains the encoded form data as key-value pairs (`username` and `password`).



## wenum

In this section, we'll leverage `wenum` to explore both GET and POST parameters within our target web application, ultimately aiming to uncover hidden values that trigger unique responses, potentially revealing vulnerabilities.

To follow along, start the target system via the question section at the bottom of the page, replacing the uses of IP:PORT with the `IP`:`PORT` for your spawned instance. We will be using the `/usr/share/seclists/Discovery/Web-Content/common.txt` wordlists for these fuzzing tasks.

Let's first ready our tools by installing `wenum` to our attack host:

``` shell
jhonmorales@htb[/htb]$ pipx install git+https://github.com/WebFuzzForge/wenum jhonmorales@htb[/htb]$ pipx runpip wenum install setuptools
```


Let's use `wenum` to fuzz the "`x`" parameter's value, starting with the `common.txt` wordlist from SecLists:

``` shell
jhonmorales@htb[/htb]$ wenum -w /usr/share/seclists/Discovery/Web-Content/common.txt --hc 404 -u "http://IP:PORT/get.php?x=FUZZ" ...  Code    Lines     Words        Size  Method   URL  ...  200       1 L       1 W        25 B  GET      http://IP:PORT/get.php?x=OA...  Total time: 0:00:02 Processed Requests: 4731 Filtered Requests: 4730 Requests/s: 1681`
````

- `-w`: Path to your wordlist.
- `--hc 404`: Hides responses with the 404 status code (Not Found), since `wenum` by default will log every request it makes.
- `http://IP:PORT/get.php?x=FUZZ`: This is the target URL. `wenum` will replace the parameter value `FUZZ` with words from the wordlist.
### POST

``` shell
jhonmorales@htb[/htb]$ ffuf -u http://IP:PORT/post.php -X POST -H "Content-Type: application/x-www-form-urlencoded" -d "y=FUZZ" -w /usr/share/seclists/Discovery/Web-Content/common.txt -mc 200 -v

        /'___\  /'___\           /'___\       
       /\ \__/ /\ \__/  __  __  /\ \__/       
       \ \ ,__\\ \ ,__\/\ \/\ \ \ \ ,__\      
        \ \ \_/ \ \ \_/\ \ \_\ \ \ \ \_/      
         \ \_\   \ \_\  \ \____/  \ \_\       
          \/_/    \/_/   \/___/    \/_/       

       v2.1.0-dev
________________________________________________

 :: Method           : POST
 :: URL              : http://IP:PORT/post.php
 :: Wordlist         : FUZZ: /usr/share/seclists/Discovery/Web-Content/common.txt
 :: Header           : Content-Type: application/x-www-form-urlencoded
 :: Data             : y=FUZZ
 :: Follow redirects : false
 :: Calibration      : false
 :: Timeout          : 10
 :: Threads          : 40
 :: Matcher          : Response status: 200
________________________________________________

[Status: 200, Size: 26, Words: 1, Lines: 2, Duration: 7ms]
| URL | http://IP:PORT/post.php
    * FUZZ: SU...

:: Progress: [4730/4730] :: Job [1/1] :: 5555 req/sec :: Duration: [0:00:01] :: Errors: 0 :
```

The main difference here is the use of the `-d` flag, which tells `ffuf` that the payload ("`y=FUZZ`") should be sent in the request body as `POST` data.

