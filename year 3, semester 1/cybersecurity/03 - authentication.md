---
related to:
created: 2025-03-02T17:41
updated: 2025-10-26T10:45
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
	- 
	- change or refresh authenticators frequently or when relevant events occur
	- protect authenticator content from unauthorized disclosure or modification
## means of authentication