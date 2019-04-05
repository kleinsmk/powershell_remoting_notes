# powershell_remoting_notes
Powershell Remoting Cross Domain using SSL

####  Step 1: Check Current Winrm Setup
```bash
    winrm get winrm/config/client
```
```bash
Client
    NetworkDelayms = 5000
    URLPrefix = wsman
    AllowUnencrypted = false
    Auth
        Basic = true
        Digest = true
        Kerberos = true
        Negotiate = true
        Certificate = true
        CredSSP = false
    DefaultPorts
        HTTP = 5985
        HTTPS = 5986
    TrustedHosts
```
####  Step 2: Turn Basic, Digest off

```bash
winrm set winrm/config/service/auth @{Basic="false"}
```
####  Step 3: Remove HTTP listener to prevent HTTP using powershell
```bash

Get-ChildItem WSMan:\Localhost\listener | Where -Property Keys -eq "Transport=HTTP" | Remove-Item -Recurse
```
####  Step 4: Get Certifcate Thumbprint
```bash
Get-ChildItem -Path Cert:\LocalMachine\My
```
#### Step 5: Add winrm Listener for Incomming HTTPS
```bash
New-Item -Path WSMan:\LocalHost\Listener -Transport HTTPS -Address * -CertificateThumbPrint  "thumprint here"–Force
```
#### Step 6 Add FW Rule for inbound HTTPS
```bash
	
New-NetFirewallRule -DisplayName "Windows Remote Management (HTTPS-In)" -Name "Windows Remote Management (HTTPS-In)" -Profile Any -LocalPort 5986 -Protocol TCP
```
