# ✨ Mitigation review contest repo setup

This `README.md` contains a set of checklists for our contest collaboration.

Your contest will use two repos: 
- **a _contest_ repo** (this one), which is used for scoping your contest and for providing information to contestants (wardens)
- **a _findings_ repo**, where issues are submitted (shared with you after the contest) 

Ultimately, when we launch the contest, this contest repo will be made public and will contain the smart contracts to be reviewed and all the information needed for contest participants. The findings repo will be made public after the contest report is published and your team has mitigated the identified issues.

Some of the checklists in this doc are for **C4 (🐺)** and some of them are for **you as the contest sponsor (⭐️)**.

---

# Contest setup

## 🐺 C4: Set up repos
- [x] Create a new private repo named `YYYY-MM-sponsorname` using this repo as a template.
- [x] Rename this repo to reflect contest date (if applicable)
- [x] Rename contest H1 below
- [x] Update pot sizes
- [x] Fill in start and end times in contest bullets below
- [ ] Add link to submission form in contest details below
- [x] Add sponsor to this private repo with 'maintain' level access.
- [ ] Send the sponsor contact the url for this repo to follow the instructions below and add contracts here. 

# Repo setup

## ⭐️ Sponsor: Add code to this repo

- [ ] Create a PR to this repo with the below changes:
- [ ] Add your PR links to this README file
- [ ] Make sure your code is thoroughly commented using the [NatSpec format](https://docs.soliditylang.org/en/v0.5.10/natspec-format.html#natspec-format).
- [ ] Be prepared for a 🚨code freeze🚨 for the duration of the contest — important because it establishes a level playing field. We want to ensure everyone's looking at the same code, no matter when they look during the contest. (Note: this includes your own repo, since a PR can leak alpha to our wardens!)


---

## ⭐️ Sponsor: Edit this README

Under "SPONSORS ADD INFO HERE" heading below, include the following:

- [ ] All information should be provided in markdown format (HTML does not render on Code4rena.com)
- [ ] Identify any areas of specific concern in reviewing the code
- [ ] Delete this checklist and all text above the line below when you're ready.

---

# ENS - Versus Mitigation Review
- Total Prize Pool: $9,000 USDC
- [Warden guidelines for C4 mitigation reviews](https://code4rena.notion.site/Guidelines-for-mitigation-reviews-ed10fc5cfbf640bd8dcec66f38b343c4)
- Submit findings [using the C4 form](https://code4rena.com/contests/2022-12-ens-mitigation-contest/submit)
- Starts December 16, 2022 20:00 UTC
- Ends December 21, 2022 20:00 UTC

## Important note 

Each warden must submit a mitigation review for *every High and Medium finding* from the parent contest. **Incomplete mitigation reviews will not be eligible for awards.**

[ ⭐️ SPONSORS ADD INFO HERE ]

# Findings being mitigated

- [H-01: `PARENT_CANNOT_CONTROL` and `CANNOT_CREATE_SUBDOMAIN` fuses can be bypassed](https://github.com/code-423n4/2022-11-ens-findings/issues/14)
- [H-02: During the deprecation period where both .eth registrar controllers are active, a crafted hack can be launched and cause the same malicious consequences of [H-01] even if [H-01] is properly fixed](https://github.com/code-423n4/2022-11-ens-findings/issues/16)
- [M-01: NameWrapper: Cannot prevent transfer while upgrade even with `CANNOT_TRANSFER fuse regardless of the upgraded NameWrapper's implementation](https://github.com/code-423n4/2022-11-ens-findings/issues/6)
- [M-02: NameWrapper: expired names behave unwrapped](https://github.com/code-423n4/2022-11-ens-findings/issues/7)
- [M-03: NameWrapper: Wrapped to Unregistered to ignore `PARENT_CANNOT_CONTROL`](https://github.com/code-423n4/2022-11-ens-findings/issues/8)

# Scope

*List all PRs in scope in the table below -- and feel free to add notes here to emphasize areas of focus.*

| URL | Mitigation of | Purpose | 
| ----------- | ----------- | ----------- | ----------- |
| contracts/folder/sample.sol | 123 | This contract does XYZ | 

# Additional Context

*Any additional notes can be shared here.*
