## Description

The "Output File Templates" functionality can be exploited to perform arbitrary file writes on the target system. By leveraging this feature, an attacker can specify arbitrary file paths and extensions, including ASP or web.config files, potentially leading to remote code execution or system compromise. This vulnerability allows an attacker not only to create new files (e.g., .asp, web.config) but also to append malicious content to existing files, such as log files, batch scripts, or other configuration files, which could lead to further code execution or configuration tampering.

## Vulnerability Location 
 
•	`https://<Host-Name>/#/Administration/TemplateManager`

## Proof of Concept

We were able to create file write template following the Entrust guide at Reference: `https://<HostName>/Help/default/Default.htm#OutputFileTemplates.htm` and changing the file path to any file with absolute system path like for example a place we can verify the file write from like the webroot directory at `C:\inetpub\wwwroot\` .

![](./Pasted%20image%2020250809015339.png)

Login with admin account and go to `https://<Host-Name>/#/Administration/TemplateManager`

Create file template with the path of file you would like to append data into, or creating a new file.

![](./Pasted%20image%2020250809015351.png)

Go to `https://<Host-Name>/#/Administration/CardFormatLinks` and Assign format to a card template:
![](./Pasted%20image%2020250809015402.png)

Go to `https://<Host-Name>/#/CardActions/PrintSamplePINMailer` and Print card PIN Mailer:

![](./Pasted%20image%2020250809015412.png)

Go to `http://<server-ip>/hello.txt` and Verify that the file is written on the server and the content is appended to it, notice that for every time the card format got printed the data appended to the file. we were also able to append data to any system file on the server.

![](./Pasted%20image%2020250809015427.png)
