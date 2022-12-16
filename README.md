# ENS - Versus Mitigation Review
- Total Prize Pool: $9,000 USDC
- [Warden guidelines for C4 mitigation reviews](https://code4rena.notion.site/Guidelines-for-mitigation-reviews-ed10fc5cfbf640bd8dcec66f38b343c4)
- Submit findings [using the C4 form](https://code4rena.com/contests/2022-12-ens-versus-mitigation-contest/submit)
- Starts December 16, 2022 20:00 UTC
- Ends December 21, 2022 20:00 UTC

## Important note 

Each warden must submit a mitigation review for *every High and Medium finding* from the parent contest. **Incomplete mitigation reviews will not be eligible for awards.**

# Findings being mitigated

- [H-01: `PARENT_CANNOT_CONTROL` and `CANNOT_CREATE_SUBDOMAIN` fuses can be bypassed](https://github.com/code-423n4/2022-11-ens-findings/issues/14)
- [H-02: During the deprecation period where both .eth registrar controllers are active, a crafted hack can be launched and cause the same malicious consequences of [H-01] even if [H-01] is properly fixed](https://github.com/code-423n4/2022-11-ens-findings/issues/16)
- [M-01: NameWrapper: Cannot prevent transfer while upgrade even with `CANNOT_TRANSFER fuse regardless of the upgraded NameWrapper's implementation](https://github.com/code-423n4/2022-11-ens-findings/issues/6)
- [M-02: NameWrapper: expired names behave unwrapped](https://github.com/code-423n4/2022-11-ens-findings/issues/7)
- [M-03: NameWrapper: Wrapped to Unregistered to ignore `PARENT_CANNOT_CONTROL`](https://github.com/code-423n4/2022-11-ens-findings/issues/8)

# Overview of changes

The mitigations were grouped into 4 separate PRs. Subnode related issues (M-02/M-03) were grouped together as their mitigations were interrelated and we wanted to make sure one mitigation didn't break the other.

For the H-01 and H-02 the main issue was related to implied unwrapping. Implied unwrapping are probably the most dangerous of all the bugs within the Name Wrapper as they involve calling contracts outside of the Name Wrapper to take control over a name that should be protected under fuses, expiry or both. Both the registry and the .eth registrar contracts have no awareness of the wrapper and therefore ignore all protections. This means we must be careful about what we consider a wrapped name. By detecting situations where a name *could* be taken over by these wrapper unaware contracts and forcing a state that makes them either unwrapped OR unable to change the state within the wrapper we can protect against these kinds of attacks. The mitigations redefine what it means to be wrapped for both .eth names and normal names. The wrapper will now check if a name both has an owner in the wrapper AND the owner in the registry is the wrapper. For .eth names we also add an additional requirement of the wrapper needing to be the owner in the .eth registrar. To accomplish this we zero out the owner in getData() if the wrapper is not the owner.

M-01 the mitigation we treat the upgrading of a name to a different owner as a transfer and call `_preTransferCheck()` if we detect it is going to a different owner. 

M-02 and M-03 are related to the state of a subname. They highlighted we needed tighter constraints on what we consider an uncreated subname. Previously expired names would still be considered created (and therefore in the unwrapped state) and therefore could be taken over by a parent that had `CANNOT_CREATE_SUBDOMAINS` burnt already. The general mitigation for M-02 and M-03 was to change the logic so names need to be expired before they can be considered "Unregistered". For M-02 we ensure that names that have an owner in the registry are considered as created. For M-03 we ensure that names that have been burned (ownerOf returns 0 and registry.owner returns 0) are considered created until the name itself expires. The initial mitigation also broke the ability for the subname to be protected under PCC as when ownerOf returned 0, the name is considered uncreated/unregistered and therefore the parent could also recreate it. To ensure this constraint is maintained, we also check that the name is also expired when ownerOf returns 0.

# Scope

| URL | Mitigation of | Purpose | 
| ----------- | ------------- | ----------- |
| https://github.com/ensdomains/ens-contracts/pull/159 | H-01 | Protects names against implied unwrapping | 
| https://github.com/ensdomains/ens-contracts/pull/162 | H-02 | Forces .eth names to be unwrapped if wrapper is owner of ERC721 |
| https://github.com/ensdomains/ens-contracts/pull/167 | M-01 | Add transfer check in upgrade functions |
| https://github.com/ensdomains/ens-contracts/pull/164 | M-02 | Resolves inconsistencies in subnode states |
| https://github.com/ensdomains/ens-contracts/pull/164 | M-03 | Resolves inconsistencies in subnode states |
