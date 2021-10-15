# C# WPF desktop app to demonstarate usage of GitHub CI/CD Action workflow and CodeQL alerts

1. Cloned sample project from https://github.com/microsoft/github-actions-for-desktop-apps 
2. Generated self-signed signing certificate. Added BASE64_ENCODED_PFX and PFX_KEY to the repository secrets. See below the steps.
3. Added CI action workflow from the sample. The workflow targets multiple platforms and different configurations. It includes the following steps: build, test, package, sign and upload package artifacts.
4. Tested CI workflow with the certificate, excluding the publish step. 
5. From Security tab selected 'Code scanning alerts' and added a default WPF 'CodeQL Analysis' workflow.
6. CodeQL scan with default workflow failed.
7. Modified CodeQL workflow to include custom build steps.

Certificate generating and encoding in Powershell:
1. Generate
New-SelfSignedCertificate -Type CodeSigningCert -Subject "CN=GE CA, 0=GE, C=US" -TextExtension @("2.5.29.19={text}false") -KeyUsage DigitalSignature -KeyLength 2048 -NotAfter (Get-Date).AddMonths(33) -FriendlyName GitHubActionsDemo

2. Encode
$pfx_cert = Get-Content '.\GitHubActionsDemo.pfx' -Encoding Byte 

3. Output to a file as a string
[System.Convert]::ToBase64String($pfx_cert) | Out-File 'SigningCertificate_Encoded.txt'

Open the output file, SigningCertificate_Encoded.txt, and copy the string inside. 
Add the string to the repo as a GitHub secret and name it "Base64_Encoded_Pfx."
Add the signing certificate password to the repo as a secret and name it "Pfx_Key"

