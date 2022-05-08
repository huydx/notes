# Reading note from 'Building Secure & Reliable Systems' book

Google team shared an interesting story on how their internal password management store failed, and
even failed harder because of over-protected security layers.
That said, Reliability and Security strongly intersect each others, and has a lot of commonalities to
be designed correctly (eg: invisibility, assessment, simplicity, evolution)

Framework for modeling insider risk

Actor/Role  -- Motive -- Actions -- Target
Eg: Engineering, Accidental, Data access, User data

Framework to model steps that an attacker may have: Cyber Kill Chains TM 

https://attack.mitre.org/ Sites that provide adversary tactics and techniques based on realworld observations. 
They has a very intuitive matrix to quickly lookup different tactics

Use proxy to review, approve, run risky commands without establishing an SSH connection to systems. Google uses
[Zero touch prod](https://www.usenix.org/conference/srecon19emea/presentation/czapinski) to require every change
in production to be made by automation, validated by software..

# PRR: [Production readiness review](https://sre.google/sre-book/evolving-sre-engagement-model/)

Step needed to require SRE engagements, which identifies reliability needs of a service based on its details.
Objectives: verify that a service meets accepted standards of production setup + minimize future incidents + bad impacts.
PRR has multiple phases, one of those is Analysis. Examples of checklist items in this phase: 

- Do updates to the service impact an unreasonably large percentage of the system at once?
- Does the service report errors to central logging systems for analysis? Does it report all exceptional conditions that result in degraded responses or failures to the end users?
...

The Analysis phase leads to the identification of recommended improvements for the service. This next phase proceeds as follows:

Improvements are prioritized based upon importance for service reliability.
The priorities are discussed and negotiated with the development team, and a plan of execution is agreed upon.
Both SRE and product development teams participate and assist each other in refactoring parts of the service or implementing additional features.

SRE related section within Google design doc:
- Data integrity: how to detect data corruption / data loss
- SLA requirement
- Security and privacy considerations: 





