

## Onboarding Checklist

### General Hints

This document describes the Transitive Trust onboarding process.

**Note:** In the embedded image the following relabels apply:
* DCCG -> TNG
* CSCA -> SCA
* DCC -> VDHC (Verifiable Digital Health Certificate)
* NB -> TNP




## Procedure
In the following description the required steps are divided into three sections:
**[1]** - **[29]** - Application and Verification
**[30]** - **[59]** Setup
**[60]** - **[End]** Participant Onboarding



### Application and Verification

**[1]** The eligible Trust Network Participant (TNP) has to submit its Transitive Trust (TT) Statement of Interest to  tng-support@who.int .

**[2]** WHO validates all provided data and verify that such a data follows WHO compliance with Trusted Network Terms of Participation (TOP 0 – 3) https://worldhealthorganization.github.io/smart-trust/concepts.htmlas well as: Participant´s eligibility criteria, governmental entity/health agency, contact details of approvers and individuals. 
**[3]** WHO to review that certificates: SCA, TLS and UP  are valid at least until 29th  February 2024. 
**[4]** In case no compliance with WHO governance rules is given, the application is rejected. 
**[5]** In case of any of 3 previous validations is not Correct, the application is rejected and the rejection is communicated to the TNP by email.
**[6 - 8]** TNP needs to modify the data they shared with WHO and re-apply it.
**[9 - 10]** If the WHO’s technical team gives a positive opinion, the eligible Trust Network Participant will be informed that Transitive Trust has been started. 
**[11]** Confirmation received by service provider and triggers the onboarding process.

**[12]** Service Provider to validate Certificates (Private/Public CA)



**[14 – 15]** Key Master information is shared from Participant to WHO, such an information is data referred as :

•	Certificates : TLS, UP, SCA
•	GPG Keys
 



**[23]** WHO Collects and reviews the information to be provided to the operations team (OPS)
  - GPG keys of all confirmed people and for all requested environments (UAT, PROD and optional DEV)
- the URL's of the applicant private GitHub repositories
- 3-digit ISO participant code of the TNP
- the confirmation  of GitHub users.

### Setup



**[33]** OPS Team receive the confirmation and information from WHO, and configure the Participant Code in Secrets and Flag for TT.
**[34]** OPS to create CronJob for Onboarding Material for PR.

**[35]** Based on EU-DCC ZIP file for PR. UAT signing included. Verify if data is at  PROD stage, IF PROD got to Step **[ 36  ]** , if not in PROD, go to stage **[ 38 ]**.



**[36 -37]** For PROD environment, the PR is verified by WHO. After successful verification, the air-gapped signing process is performed to sign the keys. Follow in Step [46].

**[38]** For UAT (and optional DEV), an auto signing process signs the keys.

**[39]** The service provider checks the PR as well.


**[42]** In case the verification was successful the whitelisting of the certificates is initiated.

**[43 - 45]** The participant will be informed that the preparation has been completed successfully.

**[46]** Service provider to review the Pull Request for PROD environment

**[47-48]** In case tests don’t pass, the service provider will inform the participant; it is expected that participant carries out the corrective actions needed and inform the service provider.



### Participant Onboarding

**[60]** The participant is connecting to UAT environment and the following steps has to be performed to check the connection:

<details> 

<summary> 1) check the connectivity with the following command: </summary> 
curl -v https://tng-uat.who.int/trustList --cert TLS.pem --key TLS_key.pem
You should see an output like:

\`\`\`
[
{
    "kid": "+jrpHSqdqZY=",
    "timestamp": "2023-05-25T07:55:21Z",
    "country": "XC",
    "certificateType": "UPLOAD",
    "thumbprint": "fa3ae91d...",
    "signature": "MIAGCSqGSIb3D...",
    "rawData": "MIIErTCCA5WgAwIBAgII..."
}
]
\`\`\`

</details> 

2) Test the other Trustlist Routes in the same style (e. g. with DSC/SCA/Upload/Authentication…) 
https://worldhealthorganization.github.io/smart-trust-network-gateway/#/Trust%20Lists/downloadTrustListFilteredByType



**[61]** In case the connection fails, some corrective actions must be performed to analyze and solve the issue(s). The operations team is supporting the TNP.

**[62]** After the connection to the TNG is established the participant can start the dry run test on UAT.
This test includes the listed steps:
  a) Upload one or more DSCs to the TNG 
  b) Delete at least one DSC again (revocation of a DSC)
  c) Optional: Upload it again (if it is required for further testing)
  d) Download the trust list from the TNG gateway
  e) Provide sample VDHC s to be verified by the service provider
f) Validate some sample VDHC 's to ensure the validation implementation is working fine

**[63]** After executing all required steps, the participant must provide the test results to the onboarding team.

**[64 - 65]** The onboarding team checks the results and provides feedback to the participant.

**[66]** In case of any issues additional corrective actions are bespoken and solved with the participant.

**[67]**   If the Dry Run Test has passed successfully, the whitelisting of the certificates in the gateway  (PROD environment) continues.
 
**[68 - 69]** The Onboarding team informs the TNP to be ready to connect to TNG PROD environment.

**[70]**  The participant is allowed and enabled to connect to TNG PROD environment (for detailed information see **[50]**).

**[71]** In case the connection fails, some corrective actions must be performed to analyze and solve the issue(s). The operations team is supporting the TNP.

**[72]** After the connection to the TNG PROD is established the Production Readiness Test can start.

<details> 
<summary> This test includes the listed steps: </summary> 
a) Create a Document Signer Certificate (DSC) and sign it by the SCA 
b) Create an CMS package with the following command:
      openssl x509 -outform der -in cert.pem -out cert.der
      openssl cms -sign -nodetach -in cert.der -signer signing.crt -inkey signing.key -out signed.der -outform DER -binary
      openssl base64 -in signed.der -out cms.b64 -e -A 
Note: cert.der is your DSC, signing.crt is the TNPUP
c) Upload the CMS package to the gateway
curl -v -X POST -H "Content-Type: application/cms" --cert TLS.pem --key TLS_key.pem --data @cms.b64 https://tng-uat.who.int/signerCertificate
d) Download the trustlist again and check if your DSC is available.
Note: Some versions of curl don’t attach the client certificates automatically. This can be checked via curl –version. Ensure that the used version is linked to OpenSSL. Especially under Windows (https://curl.se/windows/):

OpenSSL Test Example (working)
  e) Delete at least one DSC again (Revocation)
  f) Upload it again in case the DSC is required

</details> 

**[73 - 74]** After executing all required steps, the participant must provide the test results to the service provider by mail.

**[75]** The service provider checks the results.

**[76]** and in case of any issues additional corrective actions are bespoken and solved with the participant.




**[77–78]** After passing the Production Readiness Test, the participant is allowed to use the TNG PROD environment. A confirmation email is sent to the participant and to WHO with the confirmation about the successfully passed test and the completion of the onboarding process.

**[80]** Before the 31st of December 2023, the service Provider to send a remainder email to each TNP about tasks that need to be implemented, after this date will not be possible to use EU-DCC to renew the certificates.

**[90]** Create certificates per environment. Details can be found here: https://worldhealthorganization.github.io/smart-trust/concepts_CertificatePreperation.html
1) Certificates must be prepared for all environments (self-signed allowed) following the requirements in Certificate Governance - Authentication: TNPTLS - Upload: TNPUP - SCA(s): TNPSCA  . (Only if certificates are expired)
2) Prepare public keys in PEM format in your private GitHub repository (see [11]) dedicated to the used environment keys. 
3) The prepared public keys must be tagged by the generated GPG keys:
a)	Tag the version of your latest information by using git tag + signing commands either from terminal or developer IDE. Please note that an update in GitHub web desktop itself is not working, because the platform will use an intermediate key.
b)	The bot user clones the latest tag of your private repo and verifies the signature of the tag against the onboarded GPG keys
c)	After verification of the provisioned material,  the content will be taken over for the participant

**[91]** Create the GPG keys per environment and per each user needed.
Follow the instructions to create a key: https://docs.github.com/en/authentication/managing-commit-signature-verification/generating-a-new-gpg-key
Use Algorithm RSA or EC with minimum key length of 4096 bit (RSA) or 256 bit (EC).

**[92]** The TNP creates the private GitHub repositories per environment
1.	Create a private git repository on GitHub.
2.	Prepare the following information to be provided in the onboarding request:
o	Environment repository URL’s (all private to hide uploader's identity) (DEV (optional), UAT, PROD)
o	Invite the WHO bot user to the private repository (with read rights). The bot user is: tng-bot for PROD and tng-bot-dev for DEV (optional) and UAT environments.

**[93]** Upon GitHub repository creation and invitation to GitHub bot user to WHO is sent, validity for accepting this invitation is 7 days by default. If no action is taken it loses validity.

**[94]** Invitation for GitHub bot user must be accepted by WHO within 7 days since invitation was emitted.

**[97 – 98]** The verification and confirmation of the named people that are allowed to provide the key material for the TNP takes place through either of different channels:
a.	Through a Face to Face meeting
b.	Through Diplomat Channel signed confirmation
When the Letter of Application is received, identification of individuals take place with passport, finger printing or similar.  

**[99]** WHO is checking for each provided GPG key, if the given email address is included in the GPG key.


**[100]** Validate that GPG key matches cryptographic/governance criteria
- Key length (min. 3072 bit for RSA and min. 256 bit for ECDSA)
- mail address in GPG key must match with the provided mail address in the Letter of Application

**[101]** WHO is collecting the required information to be provided to the operations team (OPS):
  - GPG keys of all confirmed people and for all requested environments (UAT, PROD and optional DEV)
- the URL's of the applicant private GitHub repositories
- 3-digit ISO participant code of the TNP
- the confirmation that the invitation for the GitHub bot users has been accepted


**[102]** The collected onboarding information is sent to the operations (OPS) team. The onboarding (OB) team is informed as well. 


