<p align="center"> <img src="https://raw.githubusercontent.com/qeeqbox/authorization-bypass/main/content/authorization-bypass.svg"></p>

An application fails to properly verify a user's permissions before granting access.  A threat actor can exploit this vulnerability and carry out unauthorized actions or functionality. As a result, this could lead to privilege escalation or the unauthorized disclosure or modification of information.

Clone this current repo recursively
```sh
git clone --recurse-submodules https://github.com/qeeqbox/authorization-bypass
```
Run the webapp using Python
```sh
python3 authorization-bypass/vulnerable-web-app/webapp.py
```
Open the webapp in your browser 127.0.0.1:5142
<p align="center"> <img src="https://raw.githubusercontent.com/qeeqbox/authorization-bypass/main/content/1.png"></p>
Login as John (username: john and password: john)
<p align="center"> <img src="https://raw.githubusercontent.com/qeeqbox/authorization-bypass/main/content/2.png"></p>
John has access to the tickets only
<p align="center"> <img src="https://raw.githubusercontent.com/qeeqbox/authorization-bypass/main/content/3.png"></p>
Open the Storage tab in the developer tools to examine the request cookies, 
<p align="center"> <img src="https://raw.githubusercontent.com/qeeqbox/authorization-bypass/main/content/4.png"></p>
Add sysinfo to the access variable
<p align="center"> <img src="https://raw.githubusercontent.com/qeeqbox/authorization-bypass/main/content/5.png"></p>
Refresh the page, John now has access to tickets and sysinfo
<p align="center"> <img src="https://raw.githubusercontent.com/qeeqbox/authorization-bypass/main/content/6.png"></p>

## Code
This logic utilizes user-controlled cookies to manage authorization. A threat actor can alter these cookies to achieve unauthorized access or elevated privileges.
```py
@logged_in
def render_home_page(self):
    content = b""
    cookies = SimpleCookie(self.headers.get('Cookie'))
    if "access" in cookies:
        for access in cookies["access"].value.split(","):
            content += getattr(self, f"{access}_section" , None)()
    return BASE_TEMPLATE.replace(b"{{body}}",content)
```
 
