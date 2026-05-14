# Wodify Usage Agent

Audit which Wodify features are being paid for and whether you're getting full value from each one. Run this to get a structured report: which modules are actively used, which are underutilized, which are dormant, and which have never been touched.

---

## The Problem It Solves

Wodify is a feature-rich platform and most gyms use the core features (classes, billing, members) well but leave significant functionality untouched — often modules they're actively paying for. This agent queries your live data, scores each feature area by utilization level, and tells you plainly what you're using and what you're not, so you can either activate what you're missing or renegotiate what you don't need.

---

## How It Works

1. **Connects to live Wodify data** — Uses the Wodify MCP, which provides direct query access to the gym's reporting database (refreshed twice daily).
2. **Runs the feature audit** — Queries every feature area's table(s) for record counts and recent activity.
3. **Scores each feature** — Grades each module: Fully Used / Lightly Used / Dormant / Unused.
4. **Generates recommendations** — Flags unused features worth activating and dormant features indicating workflow gaps.
5. **Outputs a structured report** — Plain English a gym owner can act on immediately.

---

## Setup

### 1. Install Claude Code

Download and install [Claude Code](https://claude.ai/code) if you haven't already.

### 2. Configure the Wodify MCP

This agent requires the Wodify MCP server, which connects Claude Code to your gym's Wodify reporting database.

**How to get access:**
- The Wodify MCP is available through Claude.ai's MCP integrations.
- Contact Wodify or visit [claude.ai](https://claude.ai) to enable the Wodify data connector for your account.
- Once enabled, the MCP provides read-only access to your gym's reporting database — no credentials are stored locally.

> The MCP connection is managed entirely through Claude.ai. There are no API keys or credential files to configure on your machine.

### 3. Install the agent

Copy `wodify-usage-agent.md` to your Claude Code agents directory:

**Mac / Linux:**
```bash
cp wodify-usage-agent.md ~/.claude/agents/wodify-usage-agent.md
```

**Windows:**
```powershell
Copy-Item wodify-usage-agent.md "$env:USERPROFILE\.claude\agents\wodify-usage-agent.md"
```

### 4. Run the audit

In a Claude Code session:
```
run the wodify usage audit
```

Or ask naturally:
```
Which Wodify features are we not using?
What are we paying for that we're not getting value from?
Run the Wodify usage audit
```

---

## Feature Areas Audited

| Module | Wodify Area | Tables Checked |
|---|---|---|
| Member Management | Core | USERS, CLIENT_ACCOUNT_HISTORY |
| Class Scheduling & Attendance | Core | CLASS, ATTENDANCE, CLASS_RESERVATIONS |
| Membership & Billing | Core | MEMBERSHIPS, REVENUE, TRANSACTIONS |
| Waivers & Compliance | Core | WAIVERS, WAIVER_QUESTIONS |
| Membership Holds & Discounts | Core | MEMBERSHIP_HOLDS, MEMBERSHIP_DISCOUNT_DETAILS |
| Workout Tracking / WODs | Performance | PERFORMANCE_RESULTS |
| Lead Management / CRM | Business | LEADS, LEAD_HISTORY |
| Marketing Segments | Business | SEGMENTS |
| Coach Management | Business | COACHES_CLASSES |
| Payroll | Business | PAYROLL_DETAILS |
| Appointments / Personal Training | Add-on | APPOINTMENT_DETAILS |
| Skill Progressions | Add-on | PROGRESSIONS, PROGRESSIONS_TRACKERS |
| Retail / Products | Add-on | PRODUCTS |
| Store Credit | Add-on | STORE_CREDIT, STORE_CREDIT_INVOICES |
| Website Traffic Integration | Integration | SITES |
| Check-in Kiosk / Devices | Integration | DEVICE_HISTORY |

---

## Utilization Grades

| Grade | Meaning |
|---|---|
| **Fully Used** | High record volume with recent activity (within 90 days) |
| **Lightly Used** | Low record count or activity slowed significantly |
| **Dormant** | Has historical data but no activity in 365+ days — was used, then stopped |
| **Unused** | Zero records — feature was never activated |

---

## Example Output

```
WODIFY FEATURE AUDIT — [Date]
Gym: [Your Gym Name]

━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
FULLY USED
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
Class Scheduling: ~40,000 classes on record, 150k+ attendance entries
Workout Tracking: 300,000+ performance results — most active feature in the system
Lead Management: 2,500+ leads with full funnel history
Membership & Billing: 5,000+ contracts, 25k revenue records, 20k transactions
Marketing Segments: 15,000+ segment memberships actively maintained
Payroll: 4,000+ payroll entries — coach compensation fully tracked

━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
LIGHTLY USED
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
Skill Progressions: ~30 records total — feature is live and recent
  but adoption is very low. Only a small subset of members have been assigned levels.

━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
DORMANT (was used, now stopped)
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
Appointments: ~30 bookings, last one ~9 months ago — tried and abandoned.
  Suggests personal training or 1-on-1 scheduling was offered and discontinued.

━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
UNUSED (never activated)
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
Store Credit: Zero records. Lets you issue credits to members for make-up classes,
  referrals, or promotions — applied at checkout.
Device History: Zero records. Tracks which devices (tablets, kiosks) members use
  to check in — useful for identifying self-service check-in adoption.

━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
RECOMMENDATIONS
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
1. Store Credit is a zero-effort activation — use it for referral rewards, make-up
   class credits, or member appreciation gestures. Increases retention with no cost.
2. Appointments was tried and abandoned. If PT is no longer offered, confirm the
   module isn't adding to your plan cost. If PT is still offered, booking may be
   happening outside Wodify (text, phone) — worth centralizing.
3. Skill Progressions is live but at ~30 records, almost no one has been assigned a
   level. Assigning all active members a starting level would make this a useful
   retention and engagement tool.
```

---

## Key Files

| File | Purpose |
|---|---|
| `README.md` | This file — agent overview, feature map, and example output |
| `wodify-usage-agent.md` | Claude Code subagent — queries, scoring rubric, and report format. Copy to `~/.claude/agents/` |

---

## Optional: General Agents Integration

The `manager.py`, `researcher.py`, and `documenter.py` agents referenced in some versions of this project are separate local automation agents (not included here). They can be used to:

- Schedule monthly audit runs and diff against prior reports
- Research Wodify pricing tiers to verify which features sit behind which plan
- Document the feature-to-plan mapping as pricing changes

These are optional and not required to run the core audit.

---

## Requirements

- Claude Code
- Wodify MCP server configured through Claude.ai (see [Setup](#setup) above)
- Data is refreshed twice daily — run the audit any time for a current snapshot
