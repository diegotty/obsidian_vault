---
related to:
created: 2025-11-03, 18:33
updated: 2025-11-03T18:54
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
- *discretionary access control* (*DAC*): controls access based on the identity of the requestor, and on access rules stating what thre requestors are allowed to do
## MAC
*mandatory access control* (*MAC*): control access based on comparing security labels with security clearances
	- often provided using an *access matrix* or an *access control list* (*ACL*)
>[!info] access matrix
![[Pasted image 20251103185039.png]]

>[!info] access control list
![[Pasted image 20251103185204.png]]
it defines, for each object, a list of subjects that have access rights to the object, and the access rights for each subject to that object
>- this approach takes a subject-centered approact to access control

- *role-based access control* (*RBAC*): controls access based on the roles that users have within the systems, and on rules stating what accesses are allowed to users in given roles
- *attribute-based access control* (*ABAC*): controls access based on attributes of the user, the resource to be accessed, and *current enviromental conditions*
