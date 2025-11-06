---
related to:
created: 2025-11-03, 18:33
updated: 2025-11-06T16:16
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
we had a high level introduction to this topic at the end of the [[sicurezza|os1 course]]
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
a subject is said to have a *security clearance* of a given level, and an object is said to have a *security classification* of a given level.
- unlike *DAC*, users cannot decide to share or change the access permissions of data they own
- access is enforced by the system, not by the owner of the resource

>[!info] multilevel security
the goal of *MAC* is to enforce *multilevel security*: to manage and protect data across different hierarchical classification levels within one system

>[!example] Bell-LaPadula model
the *Bell-LaPadula model* (*BLP*) is an example of MAC. it defines two crucial rules to maintain confidentiality: 
>- *no read up* (*ss-property*): a subject can only read an object if the subject’s clearance level is greater than or equal to the object’s classification level
>- *no write down* (*\*-property*): a subject can write to an object if the subject’s clearance level is less than or equal to the object’s classification level
>	- this prevents high-level information from being copied/written-down to a low-level file that a low-clearance user could then read !! genius !
>
BLP has many limitations due to its very rigid structure

there are many implementations of MAC, the most recent being SELinux (NSA’s implementation of MAC for linux) and AppArmor for linux, and Mandatory Integrity Control for windows
### RBAC
*role-based access control* (*RBAC*) controls access based on the roles that users have within the systems, and on rules stating what accesses are allowed to users in given roles. its goals are:
- describe organizational control *policies*
- be based on job function (not on identity or clearance, but their role)
- increase flexibility/scalability in policy administration (easy to change secuirty requirements)
>[!info] i like graphs
![[Pasted image 20251103192734.png]]

>[!info] ACL matrix representation of RBAC
![[Pasted image 20251103192955.png]]


>[!info] families of role-based access control models
![[Pasted image 20251106161009.png]]
the top diagram shows how different RBAC models build upon each other, starting from the base model $RBAC_{0}$:
>- $RBAC_{0}$ is the *base model*, which includes the core concepts of users, roles, permissions, and their assignment
>- $RBAC_{1}$ extends it by introducing *hierarchical roles*: roles can inherit permissions from other roles (e.g. a manager role inherits all the permissions of a employee role)
>- $RBAC_{2}$ adds *constraints* that restrict the relationships. a common example is the *separation of duty* (*SoD*), which prevents a single user from possessing conflicting roles (e.g. a “check issuer” and a “check approver”)
>- $RBAC_{3}$ combines the features of $RBAC_{1}$ and $RBAC_{2}$
>
>the bottom diagram details the entities and their relationships in a typical RBAC system:
- *user assignments* (*UA*) connect users to roles
- *role hierarchies* (*RH*) are relationships within the roles set, defining which roles inherit permissions from others
- *permission assignments* (*PA*) connect ro
### ABAC
*attribute-based access control* (*ABAC*) controls access based on attributes of the user, the resource to be accessed, and *current enviromental conditions*
it can define authorizations that express conditions on properties of both the resource and the subject
- flexible, however its main downside is the performance impact of evaluating predicates on both resource and user properties for each access
we define the parts of this approach:
- *subject attributes*: a subject is active entity that causes information to flow among objects or changes the sytem state. the attributes define the identity and characteristics of the subject
- *object attributes*: an object is a passive information, system-related entity containing or receiving information. the attributes are "leverages" to make access control decisions
- *environment attributes*: they describe the operational, technical and even situational context in which the information access occurs. these attributes have, so far, been largely ignored in most access control policies

ABAC relies up on the evaluation of:
1. attributes of the subject
2. attributes of the object
3. an access control rule defining the allowable operations for subject-object attribute combinations *in a given environment*
we can enfore DAC, RBAC and MAC concepts, as it allows an unlimited number of attributes to be combined to satisfy any policy of privilege
- *policy*: a set of rules and relationship that govern allowable behaviour within an organization, based on the privileges of subjects and how objects are to be protected under which environment conditions
	- typically written from the perspective of the object that needs protecting and the privileges available to subjects
- *privilege*: authorized behaviours of subjects. they defined by an authority and embodied in a policy
>[!info] picture
![[Pasted image 20251103194700.png]]

