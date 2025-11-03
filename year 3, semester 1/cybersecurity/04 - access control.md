---
related to:
created: 2025-11-03, 18:33
updated: 2025-11-03T19:07
completed: false
---
# access control
the goal of *access control* is to protect confidentiality and integrity of information, by controlling what a subject can do, to prevent damage to the system
- in practice, we regulate the operations that can be executed by subjects on data and resources
- access control is typically provided as part of OS and DBMS
>[!def] RFC 4949
a process by which use of system resources is regulated according to a *security policy* and is permitted only by *authorized entities* (users, programs, processes, or other systems) according to that policy

>[!info] the flow
![[Pasted image 20251103184625.png]]
## access control models
### DAC
*discretionary access control* (*DAC*) controls access based on the identity of the requestor, and on access rules stating what thre requestors are allowed to do
such rules are often provided using an *access matrix* or an *access control list* (*ACL*) or an *extended access control matrix*
>[!info] access matrix
![[Pasted image 20251103185039.png]]

>[!info] access control list
![[Pasted image 20251103185204.png]]
it defines, for each object, a list of subjects that have access rights to the object, and the access rights for each subject to that object
>- this approach takes a subject-centered approact to access control, as it can also be seen as this:
![[Pasted image 20251103185655.png]]

>[!info] extended access control matrix
![[Pasted image 20251103185742.png]]
it defines, for each subject, its abilites in regards to all other subjects, files, processes and drives
>
> these permissions in the matrix translate to the following rules:
![[Pasted image 20251103185927.png]]
i aint reading allat ngl. happy 4 u though
>
>with this approach, every access by a subject to an object is mediated by the *controller* for that object. its decision is based on the current content of the matrix
and as we already said, some subjects have the authority to make *specific changes* to the access matrix

many modern UNIX systems support ACLs, like freeBSD, openBSD, linux and solaris
### MAC
*mandatory access control* (*MAC*) controls access based on comparing *security labels* with *security clearances*
each subject and each object is assigned a security class, and in the simplest formulation, security classes form a *strict hierarchy* and are referred to as *security levels*
- es: top secret < secrete < confidential < restricted < unclassified
a subject is said to have a *security clearance* of a given level, and an object is said to have a *security classification* of a given level

### RBAC
*role-based access control* (*RBAC*) controls access based on the roles that users have within the systems, and on rules stating what accesses are allowed to users in given roles

### ABAC
*attribute-based access control* (*ABAC*) controls access based on attributes of the user, the resource to be accessed, and *current enviromental conditions*
