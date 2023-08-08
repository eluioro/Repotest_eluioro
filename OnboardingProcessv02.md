\#\# Onboarding Checklist

\#\#\# General Hints

This document describes the standard onboarding process (so called Future Onboarding Process - FOP).

\*\*Note:\*\* In the embedded image the following relabels apply:

\* DCCG -\> TNG

\* CSCA -\> SCA

\* DCC -\> VDHC (Verifiable Digital Health Certificate)

\* NB -\> TNP

It is highly recommended:

\- \*\*to use certificates issued from a public CA which follows the CAB Forum Rules\*\*

\- \*\*not to reuse any certificates across the different staging environments\*\*

\#\# Procedure

In the following description the required steps are divided into three sections:

[1] - [20] - Application and Verification  
[30] - [45] Setup  
[50] - [End] Country Onboarding

\#\#\# Application and Verification

[1] The eligible Trust Network Participant (TNP) has to submit its Trust Network Technical Evaluation Form [link to the form] to [tng-support@who.int](mailto:tng-support@who.int) .

[2 – 3] WHO validates all provided data and verify that such a data follows WHO compliance with Trusted Network Terms of Participation (TOP 0 – 3)as well as: country´s eligibility criteria, governmental entity/health agency, contact details of approvers and individuals.

\< see chapter 9.5 in [TOP details](https://worldhealthorganization.github.io/smart-trust/concepts.html) \>

[4 - 5] In case no compliance with WHO governance rules is given, the application is rejected. The rejection is communicated to the TNP by email.

[6] TNP needs to modify the data they shared with WHO and re-apply it.

[7] If the WHO’s technical team gives a positive opinion, the eligible Trust Network Participant will be invited to start the onboarding process. The following information is included:

1.  The necessary technical specifications and configuration information to connect the local backend systems to the WHO TNG

    1.  The request to provide the necessary information to be onboarded to the different environments (UAT, PROD, DEV (optional))

    2.  The request to comply with the Trust Network Terms of Participation

[8] The TNP receives the confirmation and necessary technical information to connect to the systems and register the certificates.

[9] TNP creates the GitHub repositories per environment

1.  Create a private git repository on GitHub.

2.  Prepare the following information to be provided in the onboarding request:

    -   Environment Repository (all private to hide uploader's identity) (DEV (optional), UAT, PROD)

    -   Invite WHO bot user to the private repository (with read rights). The Bot User is: [tng-bot](https://github.com/tng-bot) for production and [tng-bot-dev](https://github.com/tng-bot-dev) for Development (optional) and User Acceptance Test environments.

[10] Create GPG keys per environment and per each user needed.

For Linux environment:

### Supported GPG key algorithms

GitHub supports several GPG key algorithms. If you try to add a key generated with an unsupported algorithm, you may encounter an error.

-   RSA

-   ElGamal

-   DSA

-   ECDH

-   ECDSA

-   EdDSA

## [Generating a GPG key](https://docs.github.com/en/authentication/managing-commit-signature-verification/generating-a-new-gpg-key#generating-a-gpg-key)

**Note:** Before generating a new GPG key, make sure you've verified your email address. If you haven't verified your email address, you won't be able to sign commits and tags with GPG. For more information, see "[Verifying your email address](https://docs.github.com/en/get-started/signing-up-for-github/verifying-your-email-address)“.

1.  Download and install the GPG command line tools for your operating system. We generally recommend installing the latest version for your operating system.

2.  Open Git Bash.

3.  Generate a GPG key pair. Since there are multiple versions of GPG, you may need to consult the relevant *man page* to find the appropriate key generation command.

    -   If you are on version 2.1.17 or greater, paste the text below to generate a GPG key pair.

        Shell

        gpg --full-generate-key

    -   If you are not on version 2.1.17 or greater, the gpg --full-generate-key command doesn't work. Paste the text below and skip to step 6.

        Shell

        gpg --default-new-key-algo rsa4096 --gen-key

4.  At the prompt, specify the kind of key you want, or press Enter to accept the default.

5.  At the prompt, specify the key size you want, or press Enter to accept the default.

6.  Enter the length of time the key should be valid. Press Enter to specify the default selection, indicating that the key doesn't expire. Unless you require an expiration date, we recommend accepting this default.

7.  Verify that your selections are correct.

8.  Enter your user ID information.

    **Note:** When asked to enter your email address, ensure that you enter the verified email address for your GitHub account. To keep your email address private, use your GitHub-provided no-reply email address. For more information, see "Verifying your email address" and "Setting your commit email address."

9.  Type a secure passphrase.

10. Use the gpg --list-secret-keys --keyid-format=long command to list the long form of the GPG keys for which you have both a public and private key. A private key is required for signing commits or tags.

    Shell

    gpg --list-secret-keys --keyid-format=long

    **Note:** Some GPG installations on Linux may require you to use gpg2 --list-keys --keyid-format LONG to view a list of your existing keys instead. In this case you will also need to configure Git to use gpg2 by running git config --global gpg.program gpg2.

11. From the list of GPG keys, copy the long form of the GPG key ID you'd like to use. In this example, the GPG key ID is 3AA5C34371567BD2:

    Shell

    \$ gpg --list-secret-keys --keyid-format=long

    /Users/hubot/.gnupg/secring.gpg

    \------------------------------------

    sec 4096R/3AA5C34371567BD2 2016-03-10 [expires: 2017-03-10]

    uid Hubot \<hubot@example.com\>

    ssb 4096R/4BB6D45482678BE3 2016-03-10

12. Paste the text below, substituting in the GPG key ID you'd like to use. In this example, the GPG key ID is 3AA5C34371567BD2:

13. gpg --armor --export 3AA5C34371567BD2

14. \# Prints the GPG key ID, in ASCII armor format

15. Copy your GPG key, beginning with -----BEGIN PGP PUBLIC KEY BLOCK----- and ending with -----END PGP PUBLIC KEY BLOCK-----.

16. Add the GPG key to your GitHub account.

17. Export the GPG keys by using:  
    gpg --amor --export [key-id]  
      
    Keys can be listed by:  
    gpg -k

[11] Create certificates per environment

For a successful connection to the gateway there are several steps to prepare:

1) Certificates must be prepared for all environments (self signed allowed) following the requirements in Certificate Governance - Authentication: TNPTLS - Upload: TNPUP - SCA(s): TNPSCA

2) Prepare public keys in PEM format in your private GitHub repository dedicated to acceptance environment keys. Follow the procedure described in this GitHub repository: https://github.com/WorldHealthOrganization/tng-participant-template (for support contact the tng-support@who.int functional mailbox).

3) The prepared public keys have to be tagged by the generated GPG keys:

1.  Tag the version of your latest information by using git tag + signing commands either from terminal or developer IDE. Please Note that an update in GitHub web desktop itself is not working, because the platform will use an intermediate key.

2.  The bot user clones the latest tag of your private repo and verifies the signature of the tag against the onboarded GPG keys

3.  After verification the content will be taken over for your country

[1] TNP is ready for onboarding according to WHO requirements and governance and has collected the following information in the mail:

\- the URL of the private GitHub repositories for each environment (UAT, PROD, DEV (optional))

\- the GPG keys per environment and authorized/responsible person

\- ISO country code

TNP to .

[1] The is received. In case the TNP hasn’t requested the Transitive Trust (only EU DCC members) or the requirements are NOT fulfilled, then await confirmation through either of channels; otherwise if Transitive Trust requirement is fulfilled and Transitive Trust is requested by a EU DCC member follow to step [30].

Find detailed information about the Transitive Trust here.

[1 - 1] The validation and confirmation of the provided GPG keys takes place through either of channels:

1.  Through a Face to Face meeting

2.  Through Diplomat Channel signed confirmation

3.  

[1] WHO acknowledges the and proceeds to GPG key and email addresses.

[] WHO is the given email address is included in the provided GPG key.

[] Validate that GPG key matches cryptographic/governance criteria

\- Key length (min. 3072 bit for RSA and min. 256 bit for ECDSA)

\- mail address in GPG key must match with the provided mail address in the Letter of Application

[2] Collect information to be provided to the operations (OPS) team:

\- GPG keys of all confirmed people and for all requested environments (UAT, PROD and optional DEV)

\- the URL's of the applicant private GitHub repositories

\- 3-digit ISO country code

\#\#\# Setup

[30] The collected onboarding information is sent to the Operations (OPS) team. The onboarding (OB) team is informed as well.

[31 - 35] The onboarding data is taken over to the configuration files by the service provider to automate the GitHub processes. As a result, a Pull Request (PR) is created. Dependent on the environment, the subsequent steps differ.

[38] For UAT (and optional DEV), an auto signing process signs the keys.

[36 -37] For PROD environment, the PR is verified by WHO. After successful verification, the air-gapped signing process is performed to sign the keys.

[39] The service provider checks the PR as well.

[40 - 41] In case the verification failed, WHO is informed about the outcome.

[42] In case the verification was successful the whitelisting of the certificates is initiated.

[43 - 45] and the participant will be informed that the preparation has been completed successfully.

\#\#\# Country Onboarding

[50] The participant is connecting to UAT environment and the following steps has to be performed to check the connection:

1) check the connectivity with the following command:  
curl -v https://tng-uat.who.int/trustList --cert TLS.pem --key TLS_key.pem  
You should see a output like:

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

2) Test the other Trustlist Routes in the same style (e. g. with DSC/SCA/Upload/Authentication…)

[51] In case the connection fails, some corrective actions must be performed to analyze and solve the issue(s). The operations team is supporting the TNP.

[52] After the connection to the TNG is established the participant can start the dry run test on UAT.

This test includes the listed steps:

\- Upload one or more DSC's to the TNG (Details: xyyy)

\- Delete at least one DSC again (Details: xyyy)

\- Upload it again if it is required for further testing

\- Download the trust list from gateway

\- Provide sample DCC's to be verified by the service provider

\- Validate some sample DCC's to ensure the validation implementation is working fine

[53] After executing all required steps, the participant must provide the test results to the service provider.

[54 - 55] The service provider checks the results

[56] and in case of any issues additional corrective actions are bespoken and solved with the participant.

[] the participant is allowed to connect to TNG PROD environment (for detailed information see [50]).

[] In case the connection fails, some corrective actions must be performed to analyze and solve the issue(s). The operations team is supporting the TNP.

[] After the connection to the TNG PROD is established the Production Readiness Test can start.

This test includes the listed steps:

a) Create a Document Signer Certificate (DSC) and sign it by the SCA

b) Create an CMS package with the following command:

openssl x509 -outform der -in cert.pem -out cert.der

openssl cms -sign -nodetach -in cert.der -signer signing.crt -inkey signing.key -out signed.der -outform DER -binary

openssl base64 -in signed.der -out cms.b64 -e -A

Note: cert.der is your DSC, signing.crt is the TNPUP

c) Upload the CMS package to the gateway  
curl -v -X POST -H "Content-Type: application/cms" --cert TLS.pem --key TLS_key.pem --data @cms.b64 https://tng-uat.who.int/signerCertificate  
d) Download the trustlist again and check if your DSC is available.

**Note**: Some versions of curl don’t attach the client certificates automatically. This can be checked via curl –version. Ensure that the used version is linked to OpenSSL. Especially under Windows (https://curl.se/windows/):  
  
OpenSSL Test Example (working)

e) Delete at least one DSC again [Details to be entered]

f) Upload it again in case the DSC is required

[6 - 6] After executing all required steps, the participant must provide the test results to the service provider by mail.

[6] The service provider checks the results.

[6] and in case of any issues additional corrective actions are bespoken and solved with the participant.

[6 - ] After passing the Production Readiness Test, the participant is allowed to use the TNG PROD environment. A confirmation email is sent to the participant and to WHO with the confirmation about the successfully passed test and the completion of the onboarding process.

\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\* older content follows \*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*

\#\#\# [optional] Onboarding to the Development Environment (DEV) [do we need this part of the description for the DEV?]

Onboarding to the DEV environment isn't a must and is only performed on request from the participant.

Please follow the instructions according to the UAT onboarding process describe below.

1) Prepare public keys in PEM format in a private Github repository dedicated to development environment keys. Follow the procedure described in this Github repository: https://github.com/WorldHealthOrganization/tng-participant-template.

2) After onboarding succeeded connect your development setup as described below in the next section "Onboarding to the User Acceptance Test Environment (UAT)".
