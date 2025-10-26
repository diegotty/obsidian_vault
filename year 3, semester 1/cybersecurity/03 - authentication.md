---
related to:
created: 2025-03-02T17:41
updated: 2025-10-26T13:43
completed: false
---
# introduction
>[!def] authentication
the process of establishing confidence in user identities that are presented electronically to an information system

there are many identification requirements to protect data:
- *uniquely* identify and authenticate system *users* , and associate that unique identification with processes acting on behalf of those users
- *uniquely* identify and authenticate *devices* before establishing a system connection
- implement *replay-resistant* authentication mechanisms for acces to priviledged and non-priviledged accounts
- obscure *feedback of authentication* information, during the authentication process
>[!info] replay attacks
a replay attack consists in capturing a valid message (/authentication data) and *resend* the same message later to the receiver, to illicitly repeat the original action (or in this case, gain access )
>- because the message is valid, the receiver may accept it as genuine and perform the requested action

so three things must be managed:
- *identifiers*
	- select and assign an indentifier that identifies an individual group, role, service or device
	- prevent the use of identifiers for a defined period of time
- *passwords*
	- maintain a list of commonly used, expected or compromised password (and frequently update the list), and verify that those passwords are not used when users create passwords
	- transmit passwords only over cryptographically protected channels
	- store passwords in a cryptographically protected form
	- select a new password upon first use after account recovery
	- enforcce composition and complexity rules for passwords
- *authenticator* (the element that allows to perform the authentication)
	- verify the identity of the individual receiving the authenticator 
	- didnt understand ngl TODO 
	- change or refresh authenticators frequently or when relevant events occur
	- protect authenticator content from unauthorized disclosure or modification
>[!info] digital identity guidelines architecture model
TODO appendi foto
## means of authentication
the four means of authenticating user identity are based on:
- *password*: something the individual knows (PIN, secret answers)
- *token*: something the individual possesses (smart card, physical key)
- *biometrics*: something the individual is (fingerprint, iris, face)
- *dynamic biometrics*: something the individual does (voice pattern, handwriting, typing rhythm (*keystroke dynamics*), walking style (*gait recognition*))
*multifactor authentication* (the use of more than one authentication means) is advised 
## assurance leves for user authentication
an organization can choose from a range of authentication technologies, based on the *degree of condifence in identity proofing* and authentication processes
the three levels of *identity assurane levels* (*IAL*), or how strong you prove who you are, when getting an account/credentials (and so how verified is the identity of the account), are:
- *IAL 1*: no link from applicant to a specific real-life identity (no confidence or minimal confidence)
- *IAL 2*: idenity is verified using either remote or physically-present identity proofing (moderate to high-confidence)
- *IAL 3*: identity is verified  mandatorily through physical presence (high-confidence)

*authenticator assurance levels* (*AAL*) define how strong an authentication method is (this also works for multifactor authentication). they are options an organization can select,based on the need for assurance (which is based on their risk assessment and the potential harm caused by an attacker taking control of an authenticator)
- *AAL 1*: provides some assurance of authentication via user-supplied ID and password
- *AAL 2*: provides high confidence of authentication via proof of possession and control of two authentication factors
- *AAL 3*: provides very high confidence of authentication via proof of possession and control of two authentication factors
# passwords
password-based authentication has many vulnerabilities !
- also , passwords’ weakness is that they have to be stored in human memory! so they tend to be easy

>[!info]
 storing a password, UNIX-style
 - *legacy*
	 - up to 8 printable characters in length
	 - 12bit salt used to modify DES encryption into a one-way hash function
	- zero value repeatedly encrypted 25 times
	- output translated to a 11 character sequence
- *today*	
	- 

### password strength
what makes a password strong ?
- UPPER/lower case characters
- special numbers
- numbers
- unpredictibility
- length


attacks include:
- *social engineering*
- *dictionary attacks*
- *rainbow table attacks*