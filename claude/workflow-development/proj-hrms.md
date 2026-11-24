# Summary 
1. Project Requirement to make SRS 
2. ERD


# Scope 

## SRS
 
kivabe pdf make kora jai. using ai, kivabe IEEE standard srs create kora jai.

## ERD
AI diye kivabe ERD korte parbo, like image, file jekhane ami change korte parbo, sequencaial dekte parbo, 
ami scope by scope agabo like - employee or user table a add hobe then tar jar sathe kaj sei table a er relation dekhabe or feature wise 
need more flexible - jodi kono tools or AI thake ja diye easily kora somvob. 

ami ekhon .md file and html paitechi using claude.


# Basic
- IEEE-style structure




# Details 

## make srs using claude prompt


```text
HRMS Application

One login panel for employee / user

Navigation:
Dashboard
Attendance(Today's Attendance, All Attendances, Reconciliations, Attendance Report)
Leave management(leave request, leave approve, leave report)

MVP:
- Dashboard
- Attendance(Today's Attendance, All Attendances)
- Leave management(leave request)

Full version
- Attendance(Reconciliations, Attendance Report)
- Leave management(leave approve, leave report)



Description MVP:
Dashboard page - need card for missed attendance, pending leave, pending wfh request, holidays, and other things
Now sign in from application.
List should be - start time, end time, duration, remarks

Full version:
Need attendance record from `zkt` machine and others.
Need approval flow for leave ,
The admin panel will configure the employee workflow, and approval requests will be sent to the designated approvers according to that workflow.

Role:
Employee
Flow approval employee
HR
Super admin

Tech:
Backend:  NestJS + Prisma
Frontend: Next + TypeScript
Database: PostgreSQL
UI:       Tailwind CSS
Infrastructure:  Docker
```

make srs like @Example-SRS.pdf


> then amake akta html file dibe, seita browser a open kore, print mode a open kore pdf korte hobe.


## make ERD using 