## Description

The application’s API allows cross-origin requests from any origin that contains the substring **`<Host-Name>`**. This misconfiguration occurs when the server is too permissive in handling the Origin header, allowing access-control responses to any domain that includes `<Host-Name>` in its name. Consequently, an attacker can create a subdomain (e.g., `https://attacker-<Host-Name>.com`) or manipulate the Origin header to gain unauthorized access to the API.

This vulnerability allows attackers to bypass the same-origin policy, a critical security measure designed to prevent malicious domains from interacting with resources belonging to another origin. As a result, sensitive data may be exposed, and unauthorized actions could be performed by the attacker’s origin. 


**Vulnerability Location**

- Subdirectories of URL: `https://<Host-Name>:444/Api`

## Proof of Concept

**Step 1**: Create a subdomain or spoof an Origin header with any URL containing `<Host-Name>`. For instance:

• `https://attacker-<Host-Name>.com`
• `https://<Host-Name>.attacker.com`

**Step 2**: Send a GET request to the API endpoint with the manipulated Origin header

![](./images/Pasted%20image%2020250809014510.png)

**Step 3**: Observe the server’s response. If the CORS misconfiguration is present, the server will respond with Access-Control-Allow-Origin set to the attacker-controlled origin, allowing cross-origin requests from the malicious domain.

![](./images/Pasted%20image%2020250809014518.png)