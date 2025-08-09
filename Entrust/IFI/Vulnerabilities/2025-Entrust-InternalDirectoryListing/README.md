## Description

The API endpoint **/Api/DataDirectory?path=** allows directory listing and arbitrary directory creation within the system directory `C:\DCGDATA\<CLIENT>\`. By providing this path, attackers can retrieve a list of all directories and files within `C:\DCGDATA\<CLIENT>\`, including sensitive directories such as DatabaseBackup, sqlscripts, and archivedlogs. Additionally, if a non-existent directory path is supplied, the API will create that directory within `C:\DCGDATA\<CLIENT>\`, allowing attackers to establish directories on the server. This behavior can be exploited for reconnaissance, unauthorized access to sensitive files, or the creation of custom directories that may later be abused for storing malicious files or configurations.

## Vulnerability Location

- URL: `https://<Host-Name>:444/Api/DataDirectory`
- Parameter: `path`
## Proof of Concept

Authenticate with admin credentials to get a JWT token.

Make a GET request to this API endpoint `https://<Host-Name>:444/Api/DataDirectory` with the path of the directory to list content:

![](./Pasted%20image%2020250809013358.png)

![](./Pasted%20image%2020250809013412.png)

When providing a non-existent directory it gets created.

![](./Pasted%20image%2020250809013436.png)
