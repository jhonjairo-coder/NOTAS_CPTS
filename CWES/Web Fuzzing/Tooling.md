1. Open a terminal and update your package lists to ensure you have the latest information on the newest versions of packages and their dependencies.

    ```shell-session
    jhonmorales@htb[/htb]$ sudo apt update
    ```
    
2. Use the following command to install Go:
    
    ```shell-session
    jhonmorales@htb[/htb]$ sudo apt install -y golang
    ```
    
3. Use the following command to install Python:
    
    ```shell-session
    jhonmorales@htb[/htb]$ sudo apt install -y python3 python3-pip
    ```
    
4. Use the following command to install and configure pipx:
    
    ```shell-session
    jhonmorales@htb[/htb]$ sudo apt install pipx
    jhonmorales@htb[/htb]$ pipx ensurepath
    jhonmorales@htb[/htb]$ sudo pipx ensurepath --global
    ```
    
5. To ensure that Go and Python are installed correctly, you can check their versions:

    ```shell-session
    jhonmorales@htb[/htb]$ go version
    jhonmorales@htb[/htb]$ python3 --version
    ```

## FFUF

```shell-session
jhonmorales@htb[/htb]$ go install github.com/ffuf/ffuf/v2@latest
```

| Use Case                         | Description                                                                               |
| -------------------------------- | ----------------------------------------------------------------------------------------- |
| `Directory and File Enumeration` | Quickly identify hidden directories and files on a web server.                            |
| `Parameter Discovery`            | Find and test parameters within web applications.                                         |
| `Brute-Force Attack`             | Perform brute-force attacks to discover login credentials or other sensitive information. |

## Gobuster

```shell-session
jhonmorales@htb[/htb]$ go install github.com/OJ/gobuster/v3@latest
```

| Use Case                      | Description                                                                             |
| ----------------------------- | --------------------------------------------------------------------------------------- |
| `Content Discovery`           | Quickly scan and find hidden web content such as directories, files, and virtual hosts. |
| `DNS Subdomain Enumeration`   | Identify subdomains of a target domain.                                                 |
| `WordPress Content Detection` | Use specific wordlists to find WordPress-related content.                               |

## FeroxBuster

```shell-session
jhonmorales@htb[/htb]$ curl -sL https://raw.githubusercontent.com/epi052/feroxbuster/main/install-nix.sh | sudo bash -s $HOME/.local/bin
```

| Use Case                     | Description                                                              |
| ---------------------------- | ------------------------------------------------------------------------ |
| `Recursive Scanning`         | Perform recursive scans to discover nested directories and files.        |
| `Unlinked Content Discovery` | Identify content that is not linked within the web application.          |
| `High-Performance Scans`     | Benefit from Rust's performance to conduct high-speed content discovery. |

## wfuzz/wenum

```shell-session
jhonmorales@htb[/htb]$ pipx install git+https://github.com/WebFuzzForge/wenum
jhonmorales@htb[/htb]$ pipx runpip wenum install setuptools
```

|Use Case|Description|
|---|---|
|`Directory and File Enumeration`|Quickly identify hidden directories and files on a web server.|
|`Parameter Discovery`|Find and test parameters within web applications.|
|`Brute-Force Attack`|Perform brute-force attacks to discover login credentials or other sensitive information.|


