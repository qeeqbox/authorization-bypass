<p align="center"> <img src="https://raw.githubusercontent.com/qeeqbox/authorization-bypass/main/content/authorization-bypass.svg"></p>

## Authorization Bypass
Authorization Bypass is a security vulnerability that occurs when an application fails to properly enforce access controls, allowing a user to access resources or perform actions beyond their assigned permissions.

## Authorization
Authorization determines what authenticated users are permitted to do within an application. When authorization controls are improperly implemented, attackers may gain access to restricted data, execute unauthorized actions, or escalate their privileges.

## How Authorization Bypass Works
1. Identifying Access Control Weaknesses: Attackers examine the application to discover areas where authorization checks are either missing or incorrectly implemented. 
2. Manipulating Requests: Attackers modify requests to access resources they should not have permission to access. Examples include:
3. Exploiting Missing Authorization Checks: If the application only verifies that a user is logged in without validating their permissions, the attacker may gain unauthorized access.

## Common Authorization Bypass Techniques
- Parameter Manipulation: Attackers modify parameters within requests to access unauthorized resources. If the application does not verify ownership, attackers may access another user's account.
- Insecure Direct Object Reference (IDOR): Applications may expose internal object identifiers without properly checking for authorization, enabling unauthorized access.
- Weak Access Control Logic: Poorly implemented authorization checks can allow users to perform actions outside their designated permissions.
- Privilege Escalation: Attackers may gain higher privileges than intended. For instance, if an application hides administrative features in the interface but does not restrict direct access, unauthorized users could access them. For example:
- A user could directly access the page at `https://qeeqbox.com/admin` if the server fails to enforce authorization.

## Authorization Bypass Impact
- Unauthorized access to sensitive data
- Compromise of user accounts
- Privilege escalation
- Unauthorized transactions
- Modification or deletion of data
- Exposure of confidential information
- Regulatory and financial consequences

## Authorization Bypass Mitigation
- Implement Server-Side Authorization Checks: Always perform validation on the server side; never rely solely on client-side validation.
- Use Role-Based Access Control (RBAC): Clearly define roles and associated permissions.
- Apply the Principle of Least Privilege: Users should only receive the minimum permissions required to perform their tasks.
- Validate Resource Ownership: For every object request, confirm that the user is authorized to access that specific resource.
- Secure API Endpoints: APIs should enforce authorization independently and should not assume that clients will behave correctly.
- Perform Security Testing
   - Conduct the following types of testing:
     - Access control testing
     - Code reviews
     - Penetration testing
     - Automated security scanning

## Authorization Bypass Example
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
 
