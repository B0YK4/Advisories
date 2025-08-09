## Description

The application endpoint `/api/OAuthTokens/` allows username enumeration due to differences in error messages returned during authentication attempts. By sending HTTP POST requests with various usernames and monitoring the responses, it’s possible to distinguish between valid and invalid usernames based on the specific error messages returned.

In this scenario, an intruder attack was conducted where various usernames were tested against a static password. Two distinct error messages were observed:

•        **"UserNotFoundException"**: This error indicates that the username does not exist in the application, suggesting an invalid username.

•        **"InvalidUsernameOrPasswordException"**: This message implies that the username exists, but the password is incorrect, thereby confirming the validity of the username.

Such discrepancies allow attackers to enumerate valid usernames, which can be leveraged in subsequent attacks such as brute force or phishing.

## Vulnerability Location

•   Login page: `https://<Host-Name>/#/Login`

•   Authentication API: `https://<Host-Name>:444/api/OAuthTokens/`

## Proof of Concept

**Login:**

Log into the application and intercept the login request with Burp Suite.  
**Set up Burp Suite Intruder:**

1. In Burp Suite, open **Intruder** and set the target URL to `https://<HostName>:444/api/OAuthTokens/`.
2. Define the HTTP **POST** request.
3. Add a **position** on the username field and select the **Sniper** attack type.

![](./Pasted%20image%2020250809014015.png)

**Load Payloads**

- In the **Payloads** tab, load a username wordlist (e.g., `admin`, `test`, `user1`).  
  Example list:  
  https://raw.githubusercontent.com/danielmiessler/SecLists/refs/heads/master/Usernames/cirtdefault-usernames.txt

**Start Attack**

- Start the attack and let Intruder iterate through usernames.

**Analyze Responses**

- Watch for discrepancies in the response body:

- **"UserNotFoundException"** → invalid username.  
![](./Pasted%20image%2020250809014034.png)

- **"InvalidUsernameOrPasswordException"** → valid username with wrong password.  
![](./Pasted%20image%2020250809014052.png)

**Filter Results**

- Use **Filter** to search for `InvalidUsernameOrPasswordException` (or filter by **Content length** `627`) to surface valid usernames.  
![](./Pasted%20image%2020250809014111.png)

**Usernames found:**

![](./Pasted%20image%2020250809014131.png)
