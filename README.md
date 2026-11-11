# Payroll Workload & Client Allocation System

A Power BI decision-support application for payroll team capacity management.

**Core question:** Which payroll agent can take a new client without creating an unacceptable workload or delivery risk?

## Problem

Payroll clients follow recurring monthly cycles (input → processing → output → updates). When several clients share the same dates, the assigned agent becomes overloaded. New clients are assigned by headcount alone, with no visibility on overload, no early warning on conflicts, and no objective basis for allocation.

## Solution

```
Excel Data Sources → Validation Layer → Decision Engine → Manager Dashboard
```

- **Data sources:** agents, clients, 12-month payroll calendar
- **Validation:** data quality checks, date logic, relationship integrity
- **Decision engine:** capacity calculation, conflict detection, suitability score
- **Dashboard:** workload views, alerts, allocation recommendation

## Data Model

| Table | Key | Content |
|---|---|---|
| `ResponsablePayroll` | `Coderesponsable` (PK) | Agents, FTE, expertise, backup agent, monthly capacity |
| `Payroll Dataset Clients` | `CodeCli` (PK) | Client size, convention, treatment mode, assigned agent |
| `12MonthsCalendar` | `CodeCli` (FK), `Coderesponsable` (FK) | Monthly input, output and updates dates |

One agent manages many clients. One client has up to 12 monthly calendar records.

## Workload & Scoring Model

```
Monthly Workload = Base Workload × Size × Treatment × Convention × Timing Pressure + Update Allowance
```

| Utilization | Status |
|---|---|
| < 70% | Available |
| 70% – 85% | Balanced |
| 85% – 100% | High |
| > 100% | Overloaded |

Agents over the maximum utilization, blocked, or missing required skills are excluded. Each eligible agent gets a 0–100 suitability score:

| Factor | Weight |
|---|---|
| Available Capacity | 35% |
| Calendar Compatibility | 25% |
| Client-Size Fit | 10% |
| Convention Experience | 10% |
| Treatment Experience | 10% |
| Portfolio Balance | 10% |

All coefficients and weights are configurable in the dashboard.

## Dashboard Pages

| Page | Purpose |
|---|---|
| **Team Overview** | KPIs, agent utilization heatmap, workload vs. capacity, alert list |
| **Agent Detail** | One agent's utilization, 12-month trend, skill mix, top clients, processing-window risk |
| **Client Detail** | Client profile, payroll calendar, conflicts with other clients |
| **Allocation Simulator** | New client or reassignment: ranked agents, exclusion reasons, before/after workload, score breakdown, Confirm / Override |
| **Data Quality & Configuration** | Data completeness, validation log, coefficient values and their status |

## Business Impact

- Replaces manual assessment with a repeatable method
- Detects overload and conflicts before deadlines are affected
- Gives explainable, factor-by-factor allocation recommendations
- Reduces key-person dependency
- Supports redistribution and hiring decisions

## Repository Structure

```
├── README.md
├── Dashboard/        # Power BI report
├── DataDefinition/   # Data definitions
├── Dataset/          # Source Excel data
└── Presentation/     # Project presentation
```

## Next Steps

- Pilot on live data
- Migrate from Excel to a database source

## Author

**Ons Guizani**
Supervised by Wissem Hammami
