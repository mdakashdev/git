# Sprint Capacity Planning

## 1. Sprint Overview

- **Sprint Duration:** 15 calendar days
- **Sprint Start Date:** 26 October 2026
- **Sprint End Date:** 09 November 2026
- **Working Days:** 11 days
- **Working Hours per Day:** 8 hours
- **QA:** Excluded from capacity calculation

### Weekend Calculation

The sprint includes 4 weekend days:

- **October:** 31 October
- **November:** 1, 7, 8 November

**Working Days = 15 calendar days - 4 weekend days = 11 days**

---

## 2. Team Members Included

The capacity calculation includes 5 people. QA (Suraiya) is excluded.

| Name      | Role                        | Capacity Assumption    |
|-----------|-----------------------------|------------------------|
| Akash     | Product Owner (PO)          | 30–40% of working time |
| Nahid     | Story Writer / PO Assistant | 100% of working time   |
| Shakil    | Scrum Master                | 30–40% of working time |
| Rubel     | Developer                   | 100% of working time   |
| Rajin     | Developer                   | 100% of working time   |
| **Total** | **5 people**                |                        |

For the PO and Scrum Master, 30–40% means their sprint capacity is calculated as 30–40% of their scheduled working hours. A range is shown rather than assuming one exact percentage.

---

## 3. Capacity Before Absences and Sprint Activities

Each full-time person has a maximum of:

`11 days × 8 hours = 88 hours`

| Team Member           | Calculation |              Capacity |
|-----------------------|-------------|----------------------:|
| Akash (PO)            | 88 × 30–40% |       26.4–35.2 hours |
| Nahid (PO Assistant)  | 11 × 8      |              88 hours |
| Shakil (Scrum Master) | 88 × 30–40% |       26.4–35.2 hours |
| Rubel (Developer)     | 11 × 8      |              88 hours |
| Rajin (Developer)     | 11 × 8      |              88 hours |
| **Total**             |             | **316.8–334.4 hours** |

---

## 4. Absence Deduction

The following absences are included. For the PO and Scrum Master, an absent day deducts only their planned 30–40% capacity for that day.

| Team Member |       Absent Days |  Capacity Deduction |
|-------------|------------------:|--------------------:|
| Rajin       |                 1 |             8 hours |
| Nahid       |                 1 |             8 hours |
| Rubel       |                 2 |            16 hours |
| Shakil      |                 1 |       2.4–3.2 hours |
| **Total**   | **5 person-days** | **34.4–35.2 hours** |

**Capacity after absences: 282.4–299.2 hours**

---

## 5. Sprint Activities Deduction

As in the original plan, each activity is allocated one working day. The three activities are:

- Sprint Planning
- Sprint Retrospective / Review
- Sprint Demo

For Nahid and the two developers, each activity is counted as 8 hours per person. For the PO and Scrum Master, activity time is counted at their 30–40% availability rate.

| Activity                      | Full-time team members |   PO + Scrum Master |               Total |
|-------------------------------|-----------------------:|--------------------:|--------------------:|
| Sprint Planning               |       3 × 8 = 24 hours |       4.8–6.4 hours |     28.8–30.4 hours |
| Sprint Retrospective / Review |       3 × 8 = 24 hours |       4.8–6.4 hours |     28.8–30.4 hours |
| Sprint Demo                   |       3 × 8 = 24 hours |       4.8–6.4 hours |     28.8–30.4 hours |
| **Total**                     |           **72 hours** | **14.4–19.2 hours** | **86.4–91.2 hours** |

**Capacity after absences and sprint activities: 190.4–212.8 hours**

---

## 6. Buffer Deduction (20%)

A 20% buffer is deducted from the capacity remaining after absences and sprint activities.

| Calculation                         | At 30% PO/SM availability | At 40% PO/SM availability |
|-------------------------------------|--------------------------:|--------------------------:|
| Capacity after absences             |               282.4 hours |               299.2 hours |
| Sprint activities                   |               -86.4 hours |               -91.2 hours |
| Capacity before buffer              |               196.0 hours |               208.0 hours |
| 20% buffer                          |               -39.2 hours |               -41.6 hours |
| **Final available sprint capacity** |           **156.8 hours** |           **166.4 hours** |

---

## 7. Final Sprint Capacity

**Estimated final available sprint capacity: approximately 156–166 hours.**

For planning purposes, using the midpoint of 35% availability for the PO and Scrum Master:

- PO and Scrum Master availability: 35%
- Capacity after absences: 290.8 hours
- Sprint activities: 88.8 hours
- Capacity before buffer: 202.0 hours
- 20% buffer: 40.4 hours
- **Final capacity at 35%: 161.6 hours**

---

## 8. Summary

| Description                                     |                         Result |
|-------------------------------------------------|-------------------------------:|
| Calendar days                                   |                             15 |
| Working days                                    |                             11 |
| People counted                                  |                              5 |
| QA counted                                      |                             No |
| PO availability                                 |                         30–40% |
| Scrum Master availability                       |                         30–40% |
| Absences                                        |                  5 person-days |
| Sprint activities                               | 3 days per activity assumption |
| Buffer                                          |                            20% |
| **Final available sprint capacity**             |              **157–166 hours** |
| **Planning estimate at 35% PO/SM availability** |               **161.28 hours** |

> **Assumptions:** Nahid (PO Assistant) and both developers are treated as full-time. The absence deduction for Shakil is prorated to his 30–40% planned availability. Sprint activities are deducted after absences, and the 20% buffer is applied last. This assumes the three activities each consume one working day for the full-time team members; adjust the estimate if the actual ceremonies are shorter.
