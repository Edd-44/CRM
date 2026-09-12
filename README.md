# JustClaim Fundraising Desk

A shared CRM for managing relationships with fundraising partners, built for The Claim Is Just Ltd (trading as JustClaim).

Published as a private Claude Artifact backed by a shared, real-time database, so several team members can work in it side by side. This repository holds the source.

## Pages

1. **Targets** - every partner conversation in flight: name, main contact, relationship, stage, responsible team member, indicative commitment (GBP, EUR or USD), instrument, first/last conversation dates, next steps, target close date, free-text notes and a dated conversation log. Optional convertible note fields: PIK rate, cash interest rate, months to conversion and free-text note terms.
2. **Raise builder** - set a target raise in sterling, then toggle individual targets in and out to see which combinations reach it. Supports multiple named scenarios, a greedy "fill to target" helper, and a breakdown of the selection by currency.
3. **Timeline** - a Gantt-style bar per target running from first conversation to expected completion, coloured by stage, with the last conversation marked and today shown as a dotted line. Below it, a stage board showing where every conversation currently sits.

## Conventions

- **Base currency is sterling.** EUR and USD amounts are stored in their own currency and converted for every total. Rates are point in time and entered by hand in Settings ("units per GBP"), because the published page cannot reach an external rates feed. The Targets and Raise builder pages both carry a rates box showing the values in force and the date they were last updated; that date stamps itself to today when a rate is changed, and can be overridden. The box turns amber once the rates are more than 30 days old.
- **Stages** default to Identified, Contacted, First meeting, Diligence, Term sheet, Committed, Closed, Passed. They are editable in Settings. A stage cannot be deleted while targets still sit in it.
- **Team members** are maintained in Settings and drive the "Responsible" field.
- **Going cold** flags any live conversation with no contact for more than a set number of days (21 by default, editable).

## Data

All records live in the artifact's shared database:

| Path | Contents |
| --- | --- |
| `config/settings` | team, stages, FX rates, cold-conversation threshold |
| `targets/<id>` | one document per fundraising target |
| `scenarios/<id>` | one document per raise scenario (name, target amount, selected target ids) |

Every viewer of the artifact reads and writes the same records; changes appear live in other people's browsers. Export to CSV from the Targets page.

## Running it outside Claude

`index.html` is a single self-contained file. Opened directly in a browser, with no Claude runtime present, it falls back to that browser's local storage so it still works standalone - but nothing is shared between people in that mode.

## Publishing changes

Edit `index.html`, then republish it to the same artifact URL so the team keeps the same link.
