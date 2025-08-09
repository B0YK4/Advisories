## Description

This vulnerability allows an attacker to read arbitrary files on the target system by abusing the image read endpoint **(/Api/DataDirectory/Image?path=**). Although intended to serve image files, this endpoint does not properly validate the file type or restrict paths, allowing an attacker to specify any file path within the **`C:\DCGData\\<CLIENT>\\`**  directory. Sensitive files, such as database backups, SQL files, configuration files, and log files, could be accessed and exposed, potentially revealing sensitive data, database credentials, and system configuration information.

## Vulnerability Location

- URL: `https://<Host-Name>:444/Api/DataDirectory/Image`  
- Parameter: `path`

## Proof of Concept

**Step 1:** Authenticate to this API endpoint `https://<Host-Name>:444/api/OAuthTokens/` with a banker or admin account to get a JWT token.

**Step 2:** Make a request to this API endpoint `https://<Host-Name>:444/Api/DataDirectory/Image?path=` with the path of a file you want to read and add the custom header `Jwt: <token>`.

![](./Pasted%20image%2020250808231854.png)

We were able to read the database backup and restore it.

![](./Pasted%20image%2020250808232518.png)
