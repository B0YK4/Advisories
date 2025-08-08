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

Login into the application and intercept the login request with burpsuite  **Set up Burp Suite Intruder**:

1.     In Burp Suite, navigate to the Intruder tool and set the target URL to `https://<HostName>:444/api/OAuthTokens/`.

2.     Define the HTTP POST request and add position to brute force the username value 3. Select the **Sniper** attack type.

![[Pasted image 20250809014015.png]] 

**Load Payloads**:

•   Under the "Payloads" tab, load a wordlist of common usernames (e.g., admin, test, user1). You can use this  list:

https://raw.githubusercontent.com/danielmiessler/SecLists/refs/heads/master/Usernames/cirtdefault-usernames.txt  

**Start Attack:**

- Begin the attack and allow Burp Suite to iterate through the usernames.

**Analyze Responses**:

- Observe the responses returned for each username attempt.

- Look for discrepancies in the response body, specifically:

- **"UserNotFoundException"** indicates an invalid username.  

![[Pasted image 20250809014034.png]]

o   **"InvalidUsernameOrPasswordException"** suggests a valid username with an incorrect password.

![[Pasted image 20250809014052.png]]

**Filter Results**:

•        Use the filter option in Burp Suite to search for **InvalidUsernameOrPasswordException** in the responses to identify valid usernames. Or you can filter by content length: **627**

![[Pasted image 20250809014111.png]]

Usernames found:

![[Pasted image 20250809014131.png]]