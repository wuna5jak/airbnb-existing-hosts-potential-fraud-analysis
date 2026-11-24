# Early-warning signals of risky behavior among established Airbnb hosts in Barcelona

> **Status:** work in progress. Week 1 of 13 (started 23 November 2026, due 21 February 2027).

A data analysis portfolio project built as part of the **Data Analyst Bootcamp at Fundación Somos F5**. I am working on it in the background of the course, at about 2 hours a day, over three months.

## The question

**How can changes in an established Airbnb host's behavior pattern in Barcelona signal possible fraud, and could those signals help platforms prevent it earlier?**

## The short version

Airbnb already has defenses against the most obvious risks: identity checks for hosts, machine-learning screening of new listings, payouts held until after check-in, and filters for personal details in messages. These are built mainly for the case of a new or unverified host trying to scam guests.

The harder case is a **long-standing, verified host whose behavior suddenly changes**: an account with years of good reviews that has been taken over, or a legitimate host who turns dishonest. From the outside, everything on that account still looks trustworthy.

I use public data from Inside Airbnb to measure each established host against **their own past pattern** and flag the ones that break from it. The result is a points-based risk score with three tiers (yellow, orange, red), a dashboard, and an honest assessment of what public data can and cannot show.

## Why this topic

- **It fills a gap.** Defenses against new and unverified hosts are well covered by the platform. Behavior changes in trusted accounts are harder to see, and security researchers and the press have reported a rise in scams of this kind. I treat those figures as context, not as findings, because I have not verified them independently. *(Sources to be added to the context facts in week 5.)*
- **It combines data and human behavior.** The signals are about what people do: how they price, write, collect reviews and change their accounts.
- **It fits the work I want to move into.** I want to work in fraud, risk or trust and safety, and this project practices the same reasoning on public data: baselines, flags, thresholds, false positives and validation.

## What this project is not

- It does **not** prove fraud. A flagged host is a host whose behavior changed in ways worth a closer look. Many changes are harmless, for example a host who hands their flats to a property manager.
- It does **not** use booking, message, device or payment data. Those are not public, so I work with indirect signals (reviews, prices, listing text, calendar, account details).
- I will not publish host names. Results use listing IDs only.

## Approach

1. **Define established hosts:** identity verified, account older than 2 years, 20 or more reviews.
2. **Build each host's baseline** from their own history: review rate, price level, listings, text style.
3. **Flag breaks from that baseline** with simple, explainable rules, grouped into families (portfolio, reviews, listing authenticity, account changes).
4. **Score and tier:** points per flag, a cap per family, then yellow, orange and red tiers, calibrated to roughly 10%, 3% and 1% of established hosts.
5. **Compare with machine learning:** an Isolation Forest, change-point detection and a reviewer network graph, as a check on the rules, not a replacement for them.
6. **Validate honestly:** complaint rate by tier, a weight sensitivity test, a manual read of 20 red cases, and a check of listings that disappear in later snapshots.

## Data

- **Source:** [Inside Airbnb](https://insideairbnb.com/get-the-data/), Barcelona. Released under CC BY 4.0. This project is not endorsed by Airbnb or Inside Airbnb.
- **Snapshots:** up to four from the last 12 months, about three months apart, including the newest (17 September 2026). *(Exact dates to be filled in after week 1.)*
- **Files:** detailed listings (all snapshots); detailed reviews, calendar, summary listings and neighbourhoods (newest snapshot).
- The raw files are large and are **not** stored in this repository. Download them from the link above.

## Tools

- **Python** (pandas, scikit-learn, ruptures, networkx) in **VS Code** with Jupyter notebooks
- **MySQL 8** with **MySQL Workbench** for the database, baselines and queries
- **Power BI Desktop** for the final 3-page dashboard
- **Git and GitHub** for version control

## Plan

| Phase | Weeks | Dates | What happens |
| --- | --- | --- | --- |
| 1. Core version | 1-7 | 23 Nov - 10 Jan | Set up, load and clean the data, define established hosts, build baselines and flags, score and tier them. Version 1 public by 10 January. |
| 2. Time and depth | 8-9 | 11 - 24 Jan | Add the middle snapshots, price path and calendar features, then the machine-learning comparison. |
| 3. Check and present | 10-13 | 25 Jan - 21 Feb | Validate, read the red cases, build the dashboard, write up the findings. |

Progress:

- [ ] Phase 1: core version
- [ ] Phase 2: time and depth
- [ ] Phase 3: check and present

## What I will report whatever the data shows

I decided these rules before looking at the results, so I cannot reshape the story afterwards.

- **If some flagged hosts look really suspicious:** I present them as case studies, with listing IDs only.
- **If flags fire but a manual read finds mostly harmless explanations:** I report that. Telling real risk from normal change is the finding.
- **If very little fires** (for example fewer than 1% of established hosts reach orange): I report flag rates and limitations instead of a ranked list, and explain which signals public data cannot show.

## Known limits

- The data is a set of snapshots, not a continuous record, so I only see changes between snapshot dates.
- The calendar cannot tell a booked night from a night blocked by the host.
- Listings that were removed are missing from later snapshots, so I mostly see listings that stayed up.
- There are no confirmed fraud labels, so I report precision on cases I read by hand, not an accuracy score.

## Repository structure (planned)

```
notebooks/   cleaning, features, scoring, validation
sql/         schema and queries, each with a one-line comment
data/        small derived tables only (no raw Inside Airbnb files)
dashboard/   Power BI file and screenshots
docs/        write-up and flag dictionary (name, rule, points, family)
```

## Project log

A short note every Friday: what ran, what surprised me, what is next.

**Week 1 (23 - 29 Nov):** 
- [x] repository created, snapshots downloaded, tools ready


## Credits and sources

- Inside Airbnb, [Get the Data](https://insideairbnb.com/get-the-data/) and [Data Assumptions](https://insideairbnb.com/data-assumptions/)
- Reinhold, S. and Dolnicar, S. (2018). [How Airbnb creates value](https://library.oapen.org/bitstream/20.500.12657/30986/1/640674.pdf). In Dolnicar (ed.), *Peer-to-Peer Accommodation Networks*, pp. 39-53. Favorable to Airbnb and based on 2017-era journalistic sources.
