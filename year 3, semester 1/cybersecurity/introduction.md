---
related to:
created: 2025-03-02T17:41
updated: 2025-09-29T22:36
completed: false
---
>[!def] cybersecurity
>prevention of damage to, protection of, and restoration of computers, electronic communicaiton systems, electronic communications services, wire communication, and electronic communication, including information contained therein, to ensure its **availability**, **integrity**, **authentication**, **confidentiality**, and **nonrepudiation**

>[!info] the key concepts of security and their relationships
![[Pasted image 20250929223323.png]]
>- **threat agent**: who conducts or has the intention to conduct detrimental activities

## C.I.A.
the acronym **C.I.A.** stands for: 
### confidentiality 
>[!def] definition
>avoiding unauthorized disclosure of information, protecting data and providing access for those who are allowed to see it, while disallowing others from learning anything about its content

the tools used to guarantee confidentiality are:
- **encryption**: the transformation of information using an **encryption key**, so that the transformed information can only be read using the **decryption key** (both keys are secret, and in some cases might be the same key)
- **access control**: rules and policies, that limit access to confidential information to those people that need to know
- **authentication**: the determination of the identity or role that someone has. this can be done in a number of different ways, but is usually a combination of: something the person **has**, **knows** and **is**
- **authorization**: the determination if a person or system is allowed access to resources, based on an **access control** policy
- **physical security**: the establishment of physical barriers to limit access to protected computational resources (faraday cages !!)
### integrity
>[!def] definition
>the property that something has not been altered (modified or destroyed) in an unauthorized way

tools used to guarantee integrity are:
- **backups**: the periodic archiving of data
- **checksums**: the computation of a function that maps the contents of a file to a numerical value. the function depends on the entire content of a file, and is designed in a way that even a small change to the input file is highly likely to result in a different output value
- **data correcting codes**: methods for storing data in such a way that small changes can be easily detected and automatically corrected
### availability
>[!def] definition
>the property that something is accessible and modifiable in a timely fashion by those authorized to do so

tools used to guarantee availability are:
- **physical protections**: infrastructure meant to keep information available even in the event of physical challenges
- **computational redundancies**: computers and storage devices that serve as fallbacks in case of failures
## more security concepts
### authenticity
>[!def] definition
the ability to determine that statements, policies and permissions issued by persons or systems are genuine

the primary tool used for authenticity is **digital signatures**, cryptographic computations that allow a person to commit to the authenticty of their documents in a unique way
### accountability
>[!def] definition
the security goal that generates the requirement for actions of an entity to be **traced uniquely to that entity**

to guarantee accountability, systems must keep records of their activities to permit later forensic analysis to trace security breaches or to aid in transaction disputes
### anonimity
>[!def] definition
the property that certain records or transactions not to be attributable to any individual

the tools used to guarantee anonimity are:
- **aggregation**: combining dat from many individuals, so that disclosed sums or averages cannot be tied to any individual
- **mixing**: intertwining of transactions, information or communications in a way that cannot be traced to any individual
- **proxies**: trusted agents that are willing to engage in actions for an individual in a way that cannot be traced back to that person
- **pseudonyms**: fictional identities, that can fill in for real identities in communication, but known only to a trusted entity

>[!info] levels of impact
depending on how much the systems is compromised, there are three levels of impact:
>- **low**: minimal ad