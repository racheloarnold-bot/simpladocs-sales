# AECOM — Tech Discovery Call Prep
**Date:** June 30, 2026 · 2 PM Eastern  
**Format:** Deep-dive technical scoping  
**Attendees:** Daniel Dias (AECOM Hunt), Daniel's new data engineer, Rachel + Stephen or Scott (SimplaDocs)

---

## Deal Snapshot

| Field | Detail |
|-------|--------|
| **Deal** | AECOM - Ongoing Support Model |
| **Stage** | Initial discovery → tech scoping next |
| **Amount** | $33,000 (placeholder; retainer TBD) |
| **Close target** | Aug 5, 2026 |
| **Primary contact** | Daniel Dias — Sr. Digital Technology Manager, Data · daniel.dias@aecom.com |
| **Technical contact** | Bao Phan — Data Engineer · bao.phan@aecom.com (newly hired; may be on call) |
| **Entity** | AECOM Hunt (construction management subsidiary of AECOM) |

---

## What We Know From the Intro Call (June 22)

### Their world right now
- **AECOM Hunt** builds large infrastructure & stadium projects (FIFA, NYC high-rises) across North America
- Coda was introduced ~1.5–2 years ago to replace traditional software — at radically lower cost (replaced a $120K/yr tool with $60/mo Coda doc)
- **~150 daily active viewers, 5 doc makers** — demand for docs is outpacing internal capacity
- Daniel has been building & managing most of this solo; Bao was just hired to scale it

### Core pain: Coda is hitting its ceiling
1. **Row limits** — 10,000-row chart cap is a real constraint for construction-scale data
2. **Security model** — Coda only supports view/edit; they need row-level security (RLS) for granular access control
3. **Capacity** — demand for new docs outpacing internal bandwidth
4. **Data sprawl** — pulling from Autodesk Construction Cloud, Procore, DroneDeploy, SmartPM P6 via API; needs consolidation

### Their vision
- **"Super app"**: consolidate multiple project + national-level Coda apps into a single modular platform
- **Supabase** as the new backend (trending internally; Azure/MS Fabric still technically on the table but Supabase is the lean)
- **Custom front-ends** hosted on Vercel with Coda or fully custom UI
- Build out a formal **Data & AI function** — Daniel is in a biweekly VP-level data committee; actively building budget justification

### What they want from SimplaDocs
- Optimize & maintain existing Coda docs (cleanup, formula improvements)
- Build integrations into Supabase backend
- Develop custom front-end tooling to consolidate multiple apps
- Potentially: upskilling/training the internal team alongside delivery

### Budget & model signals
- **Rate discussed:** $275/hr
- **Suggested model:** ~40 hrs/month retainer (~$10K–$12K/mo)
- Starting point: 3-month engagement, flexible to adjust
- Budget is still being built/formalized — Daniel presenting to data committee this week

### Open action items from Rachel (June 22)
- [ ] Send formal preliminary proposal with cost estimate + Supabase use cases
- [ ] Share case studies, technical documentation, mutual action plan Coda doc
- [ ] Send NDA template (requires VP-level signature at AECOM)
- [ ] Lead today's tech discovery with Stephen/Scott + Bao

---

## MEDDPICC Snapshot

| Element | What We Know | Gap |
|---------|-------------|-----|
| **Metrics** | $120K/yr SaaS replaced at $60/mo; 150 DAU; 4-6 week delivery timelines | What's the dollar value of Bao's time + Daniel's time tied up in this? What does a failed integration cost them in project delay? |
| **Economic Buyer** | VP-level data committee; Daniel presenting up. Daniel is not the signer. | Who is the actual budget approver? Which VP owns the data function? |
| **Decision Criteria** | Technical: Supabase expertise, integration track record, custom front-end capability | Are there formal vendor evaluation criteria? Is there procurement involved? |
| **Decision Process** | Daniel → internal committee → VP sign-off → procurement/legal (NDA required) | Timeline from committee approval to contract? Who's in the committee? |
| **Identify Pain** | ✅ Confirmed: capacity constraint, row limits, security gaps, data sprawl | Does this pain have a dollar/project-delay consequence they've quantified? |
| **Champion** | Daniel Dias — very engaged, building internal case, sharing materials internally | Does Daniel have the political capital to push this through? What does the committee think so far? |
| **Competition** | Not directly discussed | Are they evaluating other vendors? Internal build only? |

---

## Goals for Today's Call

### Primary objectives
1. **Scope the technical work** — get specific enough to build an accurate proposal
2. **Meet Bao** — he'll be the day-to-day technical counterpart; align on his skill level + what gaps SimplaDocs fills
3. **Move toward proposal** — validate the $10K–$12K/mo retainer model or reshape it based on scope
4. **Advance MEDDPICC** — close the gaps above, especially economic buyer and decision process

### Secondary
- Establish credibility with Bao (he didn't attend the intro call)
- Get NDA moving
- Confirm next step is proposal review, not more discovery

---

## Discovery Questions for Today

### For Daniel — business & strategy
- "Last time you mentioned the data committee meets biweekly — have you had a chance to present since we talked? How'd it land?"
- "When you think about success at the 3-month mark, what does that look like concretely — is it specific apps delivered, integrations live, something else?"
- "You mentioned budget is still being formalized. Is there a target number the committee's working toward, or is this more of a 'show us what we need and we'll find the budget' situation?"
- "Who ultimately signs off on a vendor engagement like this — is that you, a VP, procurement?"

### For Bao — technical depth
- "Bao, excited to have you on — can you walk us through where you're starting from technically? What's your background with Supabase specifically, and what's the gap you're trying to close by bringing us in?"
- "What does your current stack look like for the integrations — are you pulling from ACC/Procore APIs directly, or is there middleware in between?"
- "Where do the existing Coda docs need the most help right now — is it the data model, the formulas, the front-end, or the integrations?"
- "How are you thinking about the split between what Bao builds vs. what SimplaDocs builds? Is this 'you lead, we support' or 'we lead, you maintain'?"

### Technical scoping
- "Walk us through one of the existing Coda apps in detail — what data sources feed it, what users do with it, and where it's breaking or falling short."
- "On the Supabase migration — are you migrating existing Coda data tables or building net-new? And what's the priority order?"
- "For the RLS implementation — what does access control actually need to look like? Is it project-based, role-based, org hierarchy-based?"
- "What's the integration that's most on fire right now — the one that needs to work in 4-6 weeks?"

### Competition / alternatives
- "Are you evaluating anyone else for this, or is this more of a 'find the right partner' search?"
- "You mentioned Azure and Microsoft Fabric are still technically on the table — what would it take for those to win out over Supabase at this point?"

---

## Talking Points / Positioning

**On Supabase expertise:** Lead with this. Daniel specifically called out Supabase + backend integration as what he needs from the proposal. Make sure Stephen/Scott brings specifics.

**On tool-agnostic approach:** We're not a Coda shop trying to protect Coda. If the answer is Supabase + custom front-end on Vercel, that's where we go. Reinforce this.

**On capacity + velocity:** Their core pain is demand outpacing internal bandwidth. The pitch isn't "we're good at Coda" — it's "we free Daniel and Bao up to move faster."

**On ROI framing:** Daniel already has an exec-friendly ROI narrative (the $120K → $60/mo story). Help him extend that narrative to include the cost of *not* having support — project delays, Bao context-switching, missed timelines.

**On the retainer model:** Don't oversell the hours. If they need 20 hrs to start, say 20. Building trust on accuracy matters more than locking in 40 hrs now.

---

## Risks & Flags

- **NDA not signed yet** — Daniel hasn't shared Coda docs yet; this meeting may be limited in technical depth until the NDA clears. Have the NDA template ready to send same-day.
- **Budget not finalized** — Daniel is still building the business case. We are partly helping him sell this internally. Make sure he leaves with clean materials.
- **Bao is brand new** — his actual skill level is unknown. The engagement model may shift significantly depending on what he can vs. can't do. Don't assume.
- **Economic buyer is unclear** — Daniel is a champion, not the signer. Don't let this close without identifying the VP and decision timeline.
- **MS Fabric / Azure still alive** — Supabase isn't locked in at AECOM. Worth knowing if a platform decision could stall the project scope.

---

## What "Win" Looks Like After This Call

- [ ] Technical scope documented well enough to write a real proposal
- [ ] Bao is on board / not a blocker
- [ ] Economic buyer identified
- [ ] NDA moving (template sent, timeline set)
- [ ] Next step is proposal delivery + exec review, not another discovery call
- [ ] Daniel has what he needs to go back to the committee with confidence

---

## Post-Call Checklist

- [ ] Update HubSpot deal record with new MEDDPICC data
- [ ] Update Granola notes → push to HubSpot
- [ ] Send NDA template same day if not already sent
- [ ] Send proposal within 48 hrs (or confirm timeline with Daniel)
- [ ] Update MAP Coda doc with new action items

---

*Deal: [AECOM - Ongoing Support Model](https://app.hubspot.com/contacts/245482189/record/0-3/331517377231)*  
*Contact: [Daniel Dias](https://app.hubspot.com/contacts/245482189/record/0-1/489121235666) · [Bao Phan](https://app.hubspot.com/contacts/245482189/record/0-1/505676714727)*
