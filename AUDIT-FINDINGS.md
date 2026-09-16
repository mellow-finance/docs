# AUDIT-FINDINGS.md

Docs consistency-pass audit — Phase 1 (page inventory) and Phase 2 (automated grep audit).
Audit-only. No content files, `docs.json`, or navigation were modified as part of this pass.

## Methodology notes

- Every `.md`/`.mdx` file under the repo was inventoried (158 content files; `AGENTS.md` and `README.md` excluded as repo meta, noted below).
- Nav group and URL path are derived by walking `docs.json`'s `navigation.pages` tree directly (breadcrumb of `group` names + the page path string as declared in the tree).
- Regex categories that are scoped to current-generation pages (legacy vocabulary, first person, promotional vocabulary, ERC-4626 misclaim) exclude matches that fall purely inside fenced code blocks (```` ``` ````) or inline code spans (`` `like this` ``) naming an actual identifier — those are instead recorded separately under **2.16 Legacy identifiers found only inside code** so nothing is silently dropped.
- The `revert(s|ed)?` category excludes matches inside fenced code blocks entirely (a code block full of Solidity is expected to say `revert`). Outside code blocks, a hit is treated as **naming an actual Solidity error** (and therefore exempt, no action needed) when the sentence names a specific revert reason (e.g. `` Reverts with `InvalidPrice` ``, `` reverts with `Forbidden` ``) or is a structural NatSpec-style "Reverts if:" / "Reverts:" section header. Generic prose use of "revert(s)" with no named error (e.g. *"Operations outside the mandate revert"*) is classified `rewrite` per the revert→"cannot be executed" rule.
- Per the task's Section 2 rule, on **legacy-generation pages** (`lrt-legacy`, `alm-legacy`) the editorial rules — legacy vocabulary, first-person body prose, ERC-4626 (correct there), promotional vocabulary — do not apply; only mechanics apply everywhere (onchain, en-dash not em-dash, straight quotes, no emoji, sentence-case headings, revert→"cannot be executed" with the Solidity-error carve-out, and first person in headings/banners). Accordingly, legacy-page hits for the editorial-only categories are reported as **per-file counts only** (not exploded to every line) with a pointer to the raw grep data; full line-level detail is given for every mechanical category and for every current-generation / shared-technical hit in every category.
- Two alm-legacy pages — `mellow-alm/overview/api.mdx` (11,131 lines) and `mellow-alm/overview/contracts-specs.mdx` (~2,400 lines) — are auto-generated-style contract/test-spec dumps and are responsible for the overwhelming majority of raw hits in `emoji`, `revert`, and `first-person` (982+737 of 1,734 emoji hits; 200+180 of 430 revert hits; 38 of 149 first-person hits). Individually excerpting each of those lines would make this document unusable, so those two files are reported as aggregate counts + line-number ranges per category, with a handful of representative samples. All other files, in every category, are listed at full line-level detail.
- `\bI\b` in the mandated first-person regex, applied case-insensitively, also matches the lowercase index variable `i` and the abbreviation `i.e.` — the large majority of `first-person` hits in code-dense pages are this false positive, not a genuine first-person pronoun. Each hit below is still recorded per spec; ones that are clearly `i.e.`/loop-variable false positives are marked as such rather than classified `rewrite`.

## 1. Page inventory

**Total content files:** 158  
**Repo meta (excluded from the count above, not docs pages):** `AGENTS.md`, `README.md`

**By generation:** alm-legacy = 43, current = 61, lrt-legacy = 48, shared-technical = 6

**Orphan page (file exists, not referenced anywhere in `docs.json` navigation):** `quickstart.mdx` — current-generation content with no nav entry. Reachable only by direct URL. See Section 5 (other observations).

**Nav structure discrepancy vs. an earlier audit's assumption** (per task instructions, documented rather than acted on): `docs.json`'s `Resources` group already contains a `Legacy` subgroup nesting `Restaking Vaults` (Multivault / Interoperable Vaults / Simple LRT / "LRT V1.0") and a sibling "DVstETH Vault" group — so that part of the legacy-subordination work is already done. However, **"Mellow ALM" and "Points" are current TOP-LEVEL nav groups, not nested under Legacy** — this is open (see Section 4, Ilya). Also: the LRT V1.0 group's nav label is literally **"LRT V1.0"**, not "Mellow-LRT (Depreciated)" as an earlier audit assumed — that specific typo-fix instruction is stale/inapplicable against the current `docs.json`.

| Path | URL path | Nav group | Generation |
|---|---|---|---|
| api-reference/get-defi-protocols.mdx | /api-reference/get-defi-protocols | API Reference | shared-technical |
| api-reference/get-user-defi-positions.mdx | /api-reference/get-user-defi-positions | API Reference | shared-technical |
| api-reference/get-user-positions.mdx | /api-reference/get-user-positions | API Reference | shared-technical |
| api-reference/get-vaults.mdx | /api-reference/get-vaults | API Reference | shared-technical |
| core-vaults-and-other-approaches.mdx | /core-vaults-and-other-approaches | Core Vaults | current |
| core-vaults-for-rwa-allocation.mdx | /core-vaults-for-rwa-allocation | Core Vaults | current |
| core-vaults-for-stablecoins.mdx | /core-vaults-for-stablecoins | Core Vaults | current |
| core-vaults-integration-guide.mdx | /core-vaults-integration-guide | Core Vaults | current |
| core-vaults/architecture/factory.mdx | /core-vaults/architecture/factory | Core Vaults > Architecture | current |
| core-vaults/architecture/hooks/basicredeemhook.mdx | /core-vaults/architecture/hooks/basicredeemhook | Core Vaults > Architecture > Hooks | current |
| core-vaults/architecture/hooks/index.mdx | /core-vaults/architecture/hooks/index | Core Vaults > Architecture > Hooks | current |
| core-vaults/architecture/hooks/lidodeposithook.mdx | /core-vaults/architecture/hooks/lidodeposithook | Core Vaults > Architecture > Hooks | current |
| core-vaults/architecture/hooks/redirectingdeposithook.mdx | /core-vaults/architecture/hooks/redirectingdeposithook | Core Vaults > Architecture > Hooks | current |
| core-vaults/architecture/index.mdx | /core-vaults/architecture/index | Core Vaults > Architecture | current |
| core-vaults/architecture/libraries/fenwicktreelibrary.mdx | /core-vaults/architecture/libraries/fenwicktreelibrary | Core Vaults > Architecture > Libraries | current |
| core-vaults/architecture/libraries/index.mdx | /core-vaults/architecture/libraries/index | Core Vaults > Architecture > Libraries | current |
| core-vaults/architecture/libraries/sharemanagerlibrary.mdx | /core-vaults/architecture/libraries/sharemanagerlibrary | Core Vaults > Architecture > Libraries | current |
| core-vaults/architecture/libraries/slotlibrary.mdx | /core-vaults/architecture/libraries/slotlibrary | Core Vaults > Architecture > Libraries | current |
| core-vaults/architecture/libraries/transferlibrary.mdx | /core-vaults/architecture/libraries/transferlibrary | Core Vaults > Architecture > Libraries | current |
| core-vaults/architecture/managers/basicsharemanager.mdx | /core-vaults/architecture/managers/basicsharemanager | Core Vaults > Architecture > Managers | current |
| core-vaults/architecture/managers/feemanager.mdx | /core-vaults/architecture/managers/feemanager | Core Vaults > Architecture > Managers | current |
| core-vaults/architecture/managers/index.mdx | /core-vaults/architecture/managers/index | Core Vaults > Architecture > Managers | current |
| core-vaults/architecture/managers/riskmanager.mdx | /core-vaults/architecture/managers/riskmanager | Core Vaults > Architecture > Managers | current |
| core-vaults/architecture/managers/sharemanager.mdx | /core-vaults/architecture/managers/sharemanager | Core Vaults > Architecture > Managers | current |
| core-vaults/architecture/managers/tokenizedsharemanager.mdx | /core-vaults/architecture/managers/tokenizedsharemanager | Core Vaults > Architecture > Managers | current |
| core-vaults/architecture/modules/aclmodule.mdx | /core-vaults/architecture/modules/aclmodule | Core Vaults > Architecture > Modules | current |
| core-vaults/architecture/modules/basemodule.mdx | /core-vaults/architecture/modules/basemodule | Core Vaults > Architecture > Modules | current |
| core-vaults/architecture/modules/callmodule.mdx | /core-vaults/architecture/modules/callmodule | Core Vaults > Architecture > Modules | current |
| core-vaults/architecture/modules/index.mdx | /core-vaults/architecture/modules/index | Core Vaults > Architecture > Modules | current |
| core-vaults/architecture/modules/sharemodule.mdx | /core-vaults/architecture/modules/sharemodule | Core Vaults > Architecture > Modules | current |
| core-vaults/architecture/modules/subvaultmodule.mdx | /core-vaults/architecture/modules/subvaultmodule | Core Vaults > Architecture > Modules | current |
| core-vaults/architecture/modules/vaultmodule.mdx | /core-vaults/architecture/modules/vaultmodule | Core Vaults > Architecture > Modules | current |
| core-vaults/architecture/modules/verifiermodule.mdx | /core-vaults/architecture/modules/verifiermodule | Core Vaults > Architecture > Modules | current |
| core-vaults/architecture/oracle.mdx | /core-vaults/architecture/oracle | Core Vaults > Architecture | current |
| core-vaults/architecture/permissions/bitmaskverifier.mdx | /core-vaults/architecture/permissions/bitmaskverifier | Core Vaults > Architecture > Permissions | current |
| core-vaults/architecture/permissions/consensus.mdx | /core-vaults/architecture/permissions/consensus | Core Vaults > Architecture > Permissions | current |
| core-vaults/architecture/permissions/index.mdx | /core-vaults/architecture/permissions/index | Core Vaults > Architecture > Permissions | current |
| core-vaults/architecture/permissions/mellowacl.mdx | /core-vaults/architecture/permissions/mellowacl | Core Vaults > Architecture > Permissions | current |
| core-vaults/architecture/permissions/protocols/eigenlayerverifier.mdx | /core-vaults/architecture/permissions/protocols/eigenlayerverifier | Core Vaults > Architecture > Permissions > Protocols | current |
| core-vaults/architecture/permissions/protocols/erc20verifier.mdx | /core-vaults/architecture/permissions/protocols/erc20verifier | Core Vaults > Architecture > Permissions > Protocols | current |
| core-vaults/architecture/permissions/protocols/index.mdx | /core-vaults/architecture/permissions/protocols/index | Core Vaults > Architecture > Permissions > Protocols | current |
| core-vaults/architecture/permissions/protocols/ownedcustomverifier.mdx | /core-vaults/architecture/permissions/protocols/ownedcustomverifier | Core Vaults > Architecture > Permissions > Protocols | current |
| core-vaults/architecture/permissions/protocols/symbioticverifier.mdx | /core-vaults/architecture/permissions/protocols/symbioticverifier | Core Vaults > Architecture > Permissions > Protocols | current |
| core-vaults/architecture/permissions/verifier.mdx | /core-vaults/architecture/permissions/verifier | Core Vaults > Architecture > Permissions | current |
| core-vaults/architecture/queues/depositqueue.mdx | /core-vaults/architecture/queues/depositqueue | Core Vaults > Architecture > Queues | current |
| core-vaults/architecture/queues/index.mdx | /core-vaults/architecture/queues/index | Core Vaults > Architecture > Queues | current |
| core-vaults/architecture/queues/queue.mdx | /core-vaults/architecture/queues/queue | Core Vaults > Architecture > Queues | current |
| core-vaults/architecture/queues/redeemqueue.mdx | /core-vaults/architecture/queues/redeemqueue | Core Vaults > Architecture > Queues | current |
| core-vaults/architecture/queues/signaturedepositqueue.mdx | /core-vaults/architecture/queues/signaturedepositqueue | Core Vaults > Architecture > Queues | current |
| core-vaults/architecture/queues/signaturequeue.mdx | /core-vaults/architecture/queues/signaturequeue | Core Vaults > Architecture > Queues | current |
| core-vaults/architecture/queues/signatureredeemqueue.mdx | /core-vaults/architecture/queues/signatureredeemqueue | Core Vaults > Architecture > Queues | current |
| core-vaults/architecture/vaults/index.mdx | /core-vaults/architecture/vaults/index | Core Vaults > Architecture > Vaults | current |
| core-vaults/architecture/vaults/subvault.mdx | /core-vaults/architecture/vaults/subvault | Core Vaults > Architecture > Vaults | current |
| core-vaults/architecture/vaults/vault.mdx | /core-vaults/architecture/vaults/vault | Core Vaults > Architecture > Vaults | current |
| core-vaults/architecture/vaults/vaultconfigurator.mdx | /core-vaults/architecture/vaults/vaultconfigurator | Core Vaults > Architecture > Vaults | current |
| core-vaults/core-deployments.mdx | /core-vaults/core-deployments | Core Vaults | current |
| core-vaults/overview.mdx | /core-vaults/overview | Core Vaults | current |
| dvsteth-vault/dvv-deployment.mdx | /dvsteth-vault/dvv-deployment | Resources > Legacy > DVstETH Vault | lrt-legacy |
| dvsteth-vault/overview.mdx | /dvsteth-vault/overview | Resources > Legacy > DVstETH Vault | lrt-legacy |
| index.mdx | /index | Overview | current |
| mellow-alm-toolkit/amm-adapters/index.mdx | /mellow-alm-toolkit/amm-adapters/index | Mellow ALM > Mellow ALM Toolkit > AMM Adapters | alm-legacy |
| mellow-alm-toolkit/amm-adapters/veloammmodule.mdx | /mellow-alm-toolkit/amm-adapters/veloammmodule | Mellow ALM > Mellow ALM Toolkit > AMM Adapters | alm-legacy |
| mellow-alm-toolkit/components.mdx | /mellow-alm-toolkit/components | Mellow ALM > Mellow ALM Toolkit | alm-legacy |
| mellow-alm-toolkit/core.mdx | /mellow-alm-toolkit/core | Mellow ALM > Mellow ALM Toolkit | alm-legacy |
| mellow-alm-toolkit/domain-objects.mdx | /mellow-alm-toolkit/domain-objects | Mellow ALM > Mellow ALM Toolkit | alm-legacy |
| mellow-alm-toolkit/oracles/index.mdx | /mellow-alm-toolkit/oracles/index | Mellow ALM > Mellow ALM Toolkit > Oracles | alm-legacy |
| mellow-alm-toolkit/oracles/velooracle.mdx | /mellow-alm-toolkit/oracles/velooracle | Mellow ALM > Mellow ALM Toolkit > Oracles | alm-legacy |
| mellow-alm-toolkit/processes.mdx | /mellow-alm-toolkit/processes | Mellow ALM > Mellow ALM Toolkit | alm-legacy |
| mellow-alm-toolkit/strategy/index.mdx | /mellow-alm-toolkit/strategy/index | Mellow ALM > Mellow ALM Toolkit > Strategy | alm-legacy |
| mellow-alm-toolkit/strategy/pulsestrategymodule.mdx | /mellow-alm-toolkit/strategy/pulsestrategymodule | Mellow ALM > Mellow ALM Toolkit > Strategy | alm-legacy |
| mellow-alm-toolkit/utility-contracts/ammdepositwithdrawmodule.mdx | /mellow-alm-toolkit/utility-contracts/ammdepositwithdrawmodule | Mellow ALM > Mellow ALM Toolkit > Utility contracts | alm-legacy |
| mellow-alm-toolkit/utility-contracts/counter.mdx | /mellow-alm-toolkit/utility-contracts/counter | Mellow ALM > Mellow ALM Toolkit > Utility contracts | alm-legacy |
| mellow-alm-toolkit/utility-contracts/index.mdx | /mellow-alm-toolkit/utility-contracts/index | Mellow ALM > Mellow ALM Toolkit > Utility contracts | alm-legacy |
| mellow-alm-toolkit/utility-contracts/lpwrapper.mdx | /mellow-alm-toolkit/utility-contracts/lpwrapper | Mellow ALM > Mellow ALM Toolkit > Utility contracts | alm-legacy |
| mellow-alm-toolkit/utility-contracts/velodeployfactory.mdx | /mellow-alm-toolkit/utility-contracts/velodeployfactory | Mellow ALM > Mellow ALM Toolkit > Utility contracts | alm-legacy |
| mellow-alm/mellow-alm-toolkit/index.mdx | /mellow-alm/mellow-alm-toolkit/index | Mellow ALM > Mellow ALM Toolkit | alm-legacy |
| mellow-alm/overview/api.mdx | /mellow-alm/overview/api | Mellow ALM > Overview | alm-legacy |
| mellow-alm/overview/architecture.mdx | /mellow-alm/overview/architecture | Mellow ALM > Overview | alm-legacy |
| mellow-alm/overview/contracts-specs.mdx | /mellow-alm/overview/contracts-specs | Mellow ALM > Overview | alm-legacy |
| mellow-alm/overview/definitions.mdx | /mellow-alm/overview/definitions | Mellow ALM > Overview | alm-legacy |
| mellow-alm/overview/faq.mdx | /mellow-alm/overview/faq | Mellow ALM > Overview | alm-legacy |
| mellow-alm/overview/governance-parameters.mdx | /mellow-alm/overview/governance-parameters | Mellow ALM > Overview | alm-legacy |
| mellow-alm/overview/index.mdx | /mellow-alm/overview/index | Mellow ALM > Overview | alm-legacy |
| mellow-alm/overview/mellow-contracts-addresses/aerodrome-cl-strategies.mdx | /mellow-alm/overview/mellow-contracts-addresses/aerodrome-cl-strategies | Mellow ALM > Overview > Mellow contracts addresses | alm-legacy |
| mellow-alm/overview/mellow-contracts-addresses/gearbox-fearless-strategy.mdx | /mellow-alm/overview/mellow-contracts-addresses/gearbox-fearless-strategy | Mellow ALM > Overview > Mellow contracts addresses | alm-legacy |
| mellow-alm/overview/mellow-contracts-addresses/index.mdx | /mellow-alm/overview/mellow-contracts-addresses/index | Mellow ALM > Overview > Mellow contracts addresses | alm-legacy |
| mellow-alm/overview/mellow-contracts-addresses/lstrategy.mdx | /mellow-alm/overview/mellow-contracts-addresses/lstrategy | Mellow ALM > Overview > Mellow contracts addresses | alm-legacy |
| mellow-alm/overview/mellow-contracts-addresses/mellow-protocol-addresses-mainnet.mdx | /mellow-alm/overview/mellow-contracts-addresses/mellow-protocol-addresses-mainnet | Mellow ALM > Overview > Mellow contracts addresses | alm-legacy |
| mellow-alm/overview/mellow-contracts-addresses/mellow-protocol-addresses-polygon.mdx | /mellow-alm/overview/mellow-contracts-addresses/mellow-protocol-addresses-polygon | Mellow ALM > Overview > Mellow contracts addresses | alm-legacy |
| mellow-alm/overview/mellow-contracts-addresses/univ3-pulse-v2-wsteth-usdc.mdx | /mellow-alm/overview/mellow-contracts-addresses/univ3-pulse-v2-wsteth-usdc | Mellow ALM > Overview > Mellow contracts addresses | alm-legacy |
| mellow-alm/overview/mellow-contracts-addresses/univ3-pulse-wsteth-usdc.mdx | /mellow-alm/overview/mellow-contracts-addresses/univ3-pulse-wsteth-usdc | Mellow ALM > Overview > Mellow contracts addresses | alm-legacy |
| mellow-alm/overview/mellow-contracts-addresses/velodrome-cl-strategies.mdx | /mellow-alm/overview/mellow-contracts-addresses/velodrome-cl-strategies | Mellow ALM > Overview > Mellow contracts addresses | alm-legacy |
| mellow-alm/overview/strategies/gearbox-strategy.mdx | /mellow-alm/overview/strategies/gearbox-strategy | Mellow ALM > Overview > Strategies | alm-legacy |
| mellow-alm/overview/strategies/index.mdx | /mellow-alm/overview/strategies/index | Mellow ALM > Overview > Strategies | alm-legacy |
| mellow-alm/overview/strategies/lstrategy-1.mdx | /mellow-alm/overview/strategies/lstrategy-1 | Mellow ALM > Overview > Strategies | alm-legacy |
| mellow-alm/overview/strategies/lstrategy.mdx | /mellow-alm/overview/strategies/lstrategy | Mellow ALM > Overview > Strategies | alm-legacy |
| mellow-alm/overview/strategies/pulse-strategy-v2.mdx | /mellow-alm/overview/strategies/pulse-strategy-v2 | Mellow ALM > Overview > Strategies | alm-legacy |
| mellow-alm/overview/strategies/pulse-strategy.mdx | /mellow-alm/overview/strategies/pulse-strategy | Mellow ALM > Overview > Strategies | alm-legacy |
| mellow-alm/overview/strategies/uni-v3-boosted-strategy.mdx | /mellow-alm/overview/strategies/uni-v3-boosted-strategy | Mellow ALM > Overview > Strategies | alm-legacy |
| mellow-alm/overview/tutorials/contracts-deployments.mdx | /mellow-alm/overview/tutorials/contracts-deployments | Mellow ALM > Overview > Tutorials | alm-legacy |
| mellow-alm/overview/tutorials/deploy-your-own-strategy.mdx | /mellow-alm/overview/tutorials/deploy-your-own-strategy | Mellow ALM > Overview > Tutorials | alm-legacy |
| mellow-alm/overview/tutorials/index.mdx | /mellow-alm/overview/tutorials/index | Mellow ALM > Overview > Tutorials | alm-legacy |
| mellow-alm/overview/tutorials/wsteth-strategies-deposit-guide.mdx | /mellow-alm/overview/tutorials/wsteth-strategies-deposit-guide | Mellow ALM > Overview > Tutorials | alm-legacy |
| mellow-vaults-overview.mdx | /mellow-vaults-overview | Overview | current |
| points/defi-points-integration-instructions.mdx | /points/defi-points-integration-instructions | Points | lrt-legacy |
| points/overview.mdx | /points/overview | Points | lrt-legacy |
| points/points-in-symbiotic-pre-deposit-contracts.mdx | /points/points-in-symbiotic-pre-deposit-contracts | Points | lrt-legacy |
| quickstart.mdx | /quickstart | **NOT IN NAV** | current |
| resources/api.mdx | /resources/api | Resources | shared-technical |
| resources/brand-assets.mdx | /resources/brand-assets | Resources | shared-technical |
| resources/mellow-lrt-depreciated/dvv-legacy-architecture.mdx | /resources/mellow-lrt-depreciated/dvv-legacy-architecture | Resources > Legacy > Restaking Vaults > LRT V1.0 | lrt-legacy |
| resources/mellow-lrt-depreciated/emergency-withdrawal-guide-advanced.mdx | /resources/mellow-lrt-depreciated/emergency-withdrawal-guide-advanced | Resources > Legacy > Restaking Vaults > LRT V1.0 | lrt-legacy |
| resources/mellow-lrt-depreciated/index.mdx | /resources/mellow-lrt-depreciated/index | Resources > Legacy > Restaking Vaults > LRT V1.0 | lrt-legacy |
| resources/mellow-lrt-depreciated/modules/delegatemodules/defaultbondmodule.mdx | /resources/mellow-lrt-depreciated/modules/delegatemodules/defaultbondmodule | Resources > Legacy > Restaking Vaults > LRT V1.0 > Modules > DelegateModules | lrt-legacy |
| resources/mellow-lrt-depreciated/modules/delegatemodules/erc20swapmodule.mdx | /resources/mellow-lrt-depreciated/modules/delegatemodules/erc20swapmodule | Resources > Legacy > Restaking Vaults > LRT V1.0 > Modules > DelegateModules | lrt-legacy |
| resources/mellow-lrt-depreciated/modules/delegatemodules/index.mdx | /resources/mellow-lrt-depreciated/modules/delegatemodules/index | Resources > Legacy > Restaking Vaults > LRT V1.0 > Modules > DelegateModules | lrt-legacy |
| resources/mellow-lrt-depreciated/modules/delegatemodules/stakingmodule.mdx | /resources/mellow-lrt-depreciated/modules/delegatemodules/stakingmodule | Resources > Legacy > Restaking Vaults > LRT V1.0 > Modules > DelegateModules | lrt-legacy |
| resources/mellow-lrt-depreciated/modules/externalmodules.mdx | /resources/mellow-lrt-depreciated/modules/externalmodules | Resources > Legacy > Restaking Vaults > LRT V1.0 > Modules | lrt-legacy |
| resources/mellow-lrt-depreciated/modules/index.mdx | /resources/mellow-lrt-depreciated/modules/index | Resources > Legacy > Restaking Vaults > LRT V1.0 > Modules | lrt-legacy |
| resources/mellow-lrt-depreciated/modules/tvlmodules/defaultbondtvlmodule.mdx | /resources/mellow-lrt-depreciated/modules/tvlmodules/defaultbondtvlmodule | Resources > Legacy > Restaking Vaults > LRT V1.0 > Modules > TvlModules | lrt-legacy |
| resources/mellow-lrt-depreciated/modules/tvlmodules/erc20tvlmodule.mdx | /resources/mellow-lrt-depreciated/modules/tvlmodules/erc20tvlmodule | Resources > Legacy > Restaking Vaults > LRT V1.0 > Modules > TvlModules | lrt-legacy |
| resources/mellow-lrt-depreciated/modules/tvlmodules/index.mdx | /resources/mellow-lrt-depreciated/modules/tvlmodules/index | Resources > Legacy > Restaking Vaults > LRT V1.0 > Modules > TvlModules | lrt-legacy |
| resources/mellow-lrt-depreciated/modules/tvlmodules/managedtvlmodule.mdx | /resources/mellow-lrt-depreciated/modules/tvlmodules/managedtvlmodule | Resources > Legacy > Restaking Vaults > LRT V1.0 > Modules > TvlModules | lrt-legacy |
| resources/mellow-lrt-depreciated/oracles/chainlinkoracle.mdx | /resources/mellow-lrt-depreciated/oracles/chainlinkoracle | Resources > Legacy > Restaking Vaults > LRT V1.0 > Oracles | lrt-legacy |
| resources/mellow-lrt-depreciated/oracles/index.mdx | /resources/mellow-lrt-depreciated/oracles/index | Resources > Legacy > Restaking Vaults > LRT V1.0 > Oracles | lrt-legacy |
| resources/mellow-lrt-depreciated/oracles/managedratiosoracle.mdx | /resources/mellow-lrt-depreciated/oracles/managedratiosoracle | Resources > Legacy > Restaking Vaults > LRT V1.0 > Oracles | lrt-legacy |
| resources/mellow-lrt-depreciated/security/adminproxy.mdx | /resources/mellow-lrt-depreciated/security/adminproxy | Resources > Legacy > Restaking Vaults > LRT V1.0 > Security | lrt-legacy |
| resources/mellow-lrt-depreciated/security/index.mdx | /resources/mellow-lrt-depreciated/security/index | Resources > Legacy > Restaking Vaults > LRT V1.0 > Security | lrt-legacy |
| resources/mellow-lrt-depreciated/strategies/defaultbondstrategy.mdx | /resources/mellow-lrt-depreciated/strategies/defaultbondstrategy | Resources > Legacy > Restaking Vaults > LRT V1.0 > Strategies | lrt-legacy |
| resources/mellow-lrt-depreciated/strategies/index.mdx | /resources/mellow-lrt-depreciated/strategies/index | Resources > Legacy > Restaking Vaults > LRT V1.0 > Strategies | lrt-legacy |
| resources/mellow-lrt-depreciated/strategies/simpledvtstakingstrategy.mdx | /resources/mellow-lrt-depreciated/strategies/simpledvtstakingstrategy | Resources > Legacy > Restaking Vaults > LRT V1.0 > Strategies | lrt-legacy |
| resources/mellow-lrt-depreciated/utils/defaultaccesscontrol.mdx | /resources/mellow-lrt-depreciated/utils/defaultaccesscontrol | Resources > Legacy > Restaking Vaults > LRT V1.0 > Utils | lrt-legacy |
| resources/mellow-lrt-depreciated/utils/depositwrapper.mdx | /resources/mellow-lrt-depreciated/utils/depositwrapper | Resources > Legacy > Restaking Vaults > LRT V1.0 > Utils | lrt-legacy |
| resources/mellow-lrt-depreciated/utils/index.mdx | /resources/mellow-lrt-depreciated/utils/index | Resources > Legacy > Restaking Vaults > LRT V1.0 > Utils | lrt-legacy |
| resources/mellow-lrt-depreciated/validators/allowallvalidator.mdx | /resources/mellow-lrt-depreciated/validators/allowallvalidator | Resources > Legacy > Restaking Vaults > LRT V1.0 > Validators | lrt-legacy |
| resources/mellow-lrt-depreciated/validators/defaultbondvalidator.mdx | /resources/mellow-lrt-depreciated/validators/defaultbondvalidator | Resources > Legacy > Restaking Vaults > LRT V1.0 > Validators | lrt-legacy |
| resources/mellow-lrt-depreciated/validators/erc20swapvalidator.mdx | /resources/mellow-lrt-depreciated/validators/erc20swapvalidator | Resources > Legacy > Restaking Vaults > LRT V1.0 > Validators | lrt-legacy |
| resources/mellow-lrt-depreciated/validators/index.mdx | /resources/mellow-lrt-depreciated/validators/index | Resources > Legacy > Restaking Vaults > LRT V1.0 > Validators | lrt-legacy |
| resources/mellow-lrt-depreciated/validators/managedvalidator.mdx | /resources/mellow-lrt-depreciated/validators/managedvalidator | Resources > Legacy > Restaking Vaults > LRT V1.0 > Validators | lrt-legacy |
| resources/mellow-lrt-depreciated/vault.mdx | /resources/mellow-lrt-depreciated/vault | Resources > Legacy > Restaking Vaults > LRT V1.0 | lrt-legacy |
| resources/mellow-lrt-depreciated/vaultconfigurator.mdx | /resources/mellow-lrt-depreciated/vaultconfigurator | Resources > Legacy > Restaking Vaults > LRT V1.0 | lrt-legacy |
| restaking-vaults/interoperable-vaults/architecture.mdx | /restaking-vaults/interoperable-vaults/architecture | Resources > Legacy > Restaking Vaults > Interoperable Vaults | lrt-legacy |
| restaking-vaults/interoperable-vaults/index.mdx | /restaking-vaults/interoperable-vaults/index | Resources > Legacy > Restaking Vaults > Interoperable Vaults | lrt-legacy |
| restaking-vaults/interoperable-vaults/interop-deployments.mdx | /restaking-vaults/interoperable-vaults/interop-deployments | Resources > Legacy > Restaking Vaults > Interoperable Vaults | lrt-legacy |
| restaking-vaults/interoperable-vaults/overview.mdx | /restaking-vaults/interoperable-vaults/overview | Resources > Legacy > Restaking Vaults > Interoperable Vaults | lrt-legacy |
| restaking-vaults/multivault/architecture.mdx | /restaking-vaults/multivault/architecture | Resources > Legacy > Restaking Vaults > Multivault | lrt-legacy |
| restaking-vaults/multivault/index.mdx | /restaking-vaults/multivault/index | Resources > Legacy > Restaking Vaults > Multivault | lrt-legacy |
| restaking-vaults/multivault/multi-deployments.mdx | /restaking-vaults/multivault/multi-deployments | Resources > Legacy > Restaking Vaults > Multivault | lrt-legacy |
| restaking-vaults/multivault/overview.mdx | /restaking-vaults/multivault/overview | Resources > Legacy > Restaking Vaults > Multivault | lrt-legacy |
| restaking-vaults/simple-lrt/architecture.mdx | /restaking-vaults/simple-lrt/architecture | Resources > Legacy > Restaking Vaults > Simple LRT | lrt-legacy |
| restaking-vaults/simple-lrt/index.mdx | /restaking-vaults/simple-lrt/index | Resources > Legacy > Restaking Vaults > Simple LRT | lrt-legacy |
| restaking-vaults/simple-lrt/overview.mdx | /restaking-vaults/simple-lrt/overview | Resources > Legacy > Restaking Vaults > Simple LRT | lrt-legacy |
| restaking-vaults/simple-lrt/simple-deployments.mdx | /restaking-vaults/simple-lrt/simple-deployments | Resources > Legacy > Restaking Vaults > Simple LRT | lrt-legacy |
| security.mdx | /security | Overview | current |
| strategy-vault/overview.mdx | /strategy-vault/overview | stRATEGY Vault | current |
| strategy-vault/rewards.mdx | /strategy-vault/rewards | stRATEGY Vault | current |
| strategy-vault/streth-deployment.mdx | /strategy-vault/streth-deployment | stRATEGY Vault | current |
| vault-infrastructure-for-fintech-earn-products.mdx | /vault-infrastructure-for-fintech-earn-products | Overview | current |


## 2. Grep audit results

Categories below follow the task's required patterns. Within each category, current-generation and shared-technical hits are listed first (full line-level detail), followed by legacy-generation hits (full detail for mechanical categories; aggregated for editorial-only categories per the methodology note above).

### 2.1 Legacy vocabulary
`\b(LRT|LRTs|liquid restaking|restaking|restaker|AVS|EigenLayer|Symbiotic|DVT|DVV|DVstETH)\b` (case-insensitive)

Total hits: 425 — current-gen: 44, lrt-legacy: 381, alm-legacy: 0.

Per Section 2, this rule applies only to current-generation and shared-technical pages. All current-gen hits are listed below at full detail.

| Path | Line | Excerpt | Class | Note |
|---|---|---|---|---|
| core-vaults-and-other-approaches.mdx | 109 | * **DeFi access:** Aave, Morpho, Euler, Fluid, Spark, Ethena, Gearbox, Curve, Uniswap, Cowswap, Pendle, Symbiotic, EigenLayer, and others | flag | DeFi integration list naming third-party protocols (Symbiotic, EigenLayer) Core Vaults actually connect to today — not legacy Mellow product jargon. Confirm acceptable. |
| core-vaults/architecture/index.mdx | 383 | * Symbiotic, EigenLayer (provide liquidity for restaking rewards) | flag | Restaking case study on this page — see Needs-human-decision (Andrey): case study replacement. |
| core-vaults/architecture/index.mdx | 403 | 1. Two subvaults representing different yield sources: a delta-neutral trading strategy and restaking | flag | Restaking case study — see Needs-human-decision (Andrey). |
| core-vaults/architecture/index.mdx | 421 | 8. Funds from Subvault 2 are allocated to the restaking strategy via Symbiotic. | flag | Restaking case study — see Needs-human-decision (Andrey). |
| core-vaults/architecture/index.mdx | 433 | * Second one allowing deposits, withdrawals, withdrawal claims and reward claims & swaps from Symbiotic | flag | Restaking case study on this page — see Needs-human-decision (Andrey): case study replacement. |
| core-vaults/architecture/index.mdx | 455 | 3. Allocate to Subvault 2 (Symbiotic Restaking). | flag | Restaking case study — see Needs-human-decision (Andrey). |
| core-vaults/architecture/index.mdx | 457 | * Clicks **New Call** → target = **Restaking Subvault** → calls `deposit(subvault, 5e11)` | flag | Restaking case study — see Needs-human-decision (Andrey). |
| core-vaults/architecture/index.mdx | 472 | 4. Curator regularly claims rewards from Symbiotic Restaking and swaps them into assets before depositing them using `subvault2.call` | flag | Restaking case study on this page — see Needs-human-decision (Andrey): case study replacement. |
| core-vaults/architecture/permissions/protocols/eigenlayerverifier.mdx | 7 | `EigenLayerVerifier` is a custom `ICustomVerifier` implementation tailored to securely authorize calls to **EigenLayer** contracts like `DelegationMan | flag | Verifier module literally names the third-party protocol (EigenLayer/Symbiotic) it authorizes calls to — technically necessary naming, not legacy-product jargon. Not a rewrite candidate; flagged for confirmation only. |
| core-vaults/architecture/permissions/protocols/eigenlayerverifier.mdx | 11 | This verifier protects EigenLayer operations by: | flag | Verifier module literally names the third-party protocol (EigenLayer/Symbiotic) it authorizes calls to — technically necessary naming, not legacy-product jargon. Not a rewrite candidate; flagged for confirmation only. |
| core-vaults/architecture/permissions/protocols/eigenlayerverifier.mdx | 21 | \| `CALLER_ROLE`       \| Address allowed to initiate EigenLayer calls (typically curators)     \| | flag | Verifier module literally names the third-party protocol (EigenLayer/Symbiotic) it authorizes calls to — technically necessary naming, not legacy-product jargon. Not a rewrite candidate; flagged for confirmation only. |
| core-vaults/architecture/permissions/protocols/eigenlayerverifier.mdx | 23 | \| `STRATEGY_ROLE`     \| Whitelisted EigenLayer strategy contracts                             \| | flag | Verifier module literally names the third-party protocol (EigenLayer/Symbiotic) it authorizes calls to — technically necessary naming, not legacy-product jargon. Not a rewrite candidate; flagged for confirmation only. |
| core-vaults/architecture/permissions/protocols/eigenlayerverifier.mdx | 24 | \| `OPERATOR_ROLE`     \| Approved EigenLayer operator address for delegation                   \| | flag | Verifier module literally names the third-party protocol (EigenLayer/Symbiotic) it authorizes calls to — technically necessary naming, not legacy-product jargon. Not a rewrite candidate; flagged for confirmation only. |
| core-vaults/architecture/permissions/protocols/eigenlayerverifier.mdx | 36 | * Setting immutable references to EigenLayer’s: | flag | Verifier module literally names the third-party protocol (EigenLayer/Symbiotic) it authorizes calls to — technically necessary naming, not legacy-product jargon. Not a rewrite candidate; flagged for confirmation only. |
| core-vaults/architecture/permissions/protocols/eigenlayerverifier.mdx | 99 | * **Role enforcement:** Prevents unauthorized usage of EigenLayer functions | flag | Verifier module literally names the third-party protocol (EigenLayer/Symbiotic) it authorizes calls to — technically necessary naming, not legacy-product jargon. Not a rewrite candidate; flagged for confirmation only. |
| core-vaults/architecture/permissions/protocols/symbioticverifier.mdx | 7 | `SymbioticVerifier` is a custom `ICustomVerifier` implementation used to authorize interactions with the Symbiotic protocol. It restricts access to `d | flag | Verifier module literally names the third-party protocol (EigenLayer/Symbiotic) it authorizes calls to — technically necessary naming, not legacy-product jargon. Not a rewrite candidate; flagged for confirmation only. |
| core-vaults/architecture/permissions/protocols/symbioticverifier.mdx | 9 | This verifier ensures that only allowed addresses (typically curators) can perform specific actions within the Symbiotic ecosystem. | flag | Verifier module literally names the third-party protocol (EigenLayer/Symbiotic) it authorizes calls to — technically necessary naming, not legacy-product jargon. Not a rewrite candidate; flagged for confirmation only. |
| core-vaults/architecture/permissions/protocols/symbioticverifier.mdx | 15 | * Only whitelisted vaults can act on behalf of themselves in Symbiotic vaults and farms | flag | Verifier module literally names the third-party protocol (EigenLayer/Symbiotic) it authorizes calls to — technically necessary naming, not legacy-product jargon. Not a rewrite candidate; flagged for confirmation only. |
| core-vaults/architecture/permissions/protocols/symbioticverifier.mdx | 23 | \| `CALLER_ROLE`          \| Who is allowed to initiate Symbiotic operations (typically curators)                                   \| | flag | Verifier module literally names the third-party protocol (EigenLayer/Symbiotic) it authorizes calls to — technically necessary naming, not legacy-product jargon. Not a rewrite candidate; flagged for confirmation only. |
| core-vaults/architecture/permissions/protocols/symbioticverifier.mdx | 25 | \| `SYMBIOTIC_VAULT_ROLE` \| Contracts that are approved as Symbiotic vault                                                         \| | flag | Verifier module literally names the third-party protocol (EigenLayer/Symbiotic) it authorizes calls to — technically necessary naming, not legacy-product jargon. Not a rewrite candidate; flagged for confirmation only. |
| core-vaults/architecture/permissions/protocols/symbioticverifier.mdx | 26 | \| `SYMBIOTIC_FARM_ROLE`  \| Contracts that are approved as Symbiotic farm                                                          \| | flag | Verifier module literally names the third-party protocol (EigenLayer/Symbiotic) it authorizes calls to — technically necessary naming, not legacy-product jargon. Not a rewrite candidate; flagged for confirmation only. |
| core-vaults/architecture/permissions/protocols/symbioticverifier.mdx | 49 | * Matches target contract (`where`) with either a Symbiotic vault or farm | flag | Verifier module literally names the third-party protocol (EigenLayer/Symbiotic) it authorizes calls to — technically necessary naming, not legacy-product jargon. Not a rewrite candidate; flagged for confirmation only. |
| core-vaults/architecture/permissions/protocols/symbioticverifier.mdx | 57 | \| Symbiotic Vault \| `deposit(onBehalfOf, amount)`          \| `ISymbioticVault.deposit.selector`              \| `onBehalfOf` must have `MELLOW_VAULT_RO | flag | Verifier module literally names the third-party protocol (EigenLayer/Symbiotic) it authorizes calls to — technically necessary naming, not legacy-product jargon. Not a rewrite candidate; flagged for confirmation only. |
| core-vaults/architecture/permissions/protocols/symbioticverifier.mdx | 58 | \| Symbiotic Vault \| `withdraw(claimer, amount)`            \| `ISymbioticVault.withdraw.selector`             \| `claimer` must have `MELLOW_VAULT_ROLE` | flag | Verifier module literally names the third-party protocol (EigenLayer/Symbiotic) it authorizes calls to — technically necessary naming, not legacy-product jargon. Not a rewrite candidate; flagged for confirmation only. |
| core-vaults/architecture/permissions/protocols/symbioticverifier.mdx | 59 | \| Symbiotic Vault \| `claim(recipient, epoch)`              \| `ISymbioticVault.claim.selector`                \| `recipient` must have `MELLOW_VAULT_ROL | flag | Verifier module literally names the third-party protocol (EigenLayer/Symbiotic) it authorizes calls to — technically necessary naming, not legacy-product jargon. Not a rewrite candidate; flagged for confirmation only. |
| core-vaults/architecture/permissions/protocols/symbioticverifier.mdx | 60 | \| Symbiotic Farm  \| `claimRewards(recipient, token, data)` \| `ISymbioticStakerRewards.claimRewards.selector` \| `recipient` must have `MELLOW_VAULT_ROL | flag | Verifier module literally names the third-party protocol (EigenLayer/Symbiotic) it authorizes calls to — technically necessary naming, not legacy-product jargon. Not a rewrite candidate; flagged for confirmation only. |
| core-vaults/core-deployments.mdx | 446 | \| SyncDepositQueue(DVV) \| [0xbcdbf4d3b3b3345f8348c6aa557949deaa6bc57b](0xbcdbf4d3b3b3345f8348c6aa557949deaa6bc57b) \| | flag | DVV/DVstETH as a deployment-table label on a current-gen page — is this a real current deployment sharing the legacy product name, or misfiled content? See Needs-human-decision (Andrey). |
| core-vaults/core-deployments.mdx | 628 | \| DepositQueue (DVstETH) \| 0x4bDd2Ea1E20acb13f2758190c92a84175107A86f \| | flag | DVV/DVstETH as a deployment-table label on a current-gen page — is this a real current deployment sharing the legacy product name, or misfiled content? See Needs-human-decision (Andrey). |
| core-vaults/core-deployments.mdx | 629 | \| SyncDepositQueue (DVstETH) \| 0xA80f247b92C79740b0610b754403D5cb0bf216b5 \| | flag | DVV/DVstETH as a deployment-table label on a current-gen page — is this a real current deployment sharing the legacy product name, or misfiled content? See Needs-human-decision (Andrey). |
| index.mdx | 10 | Core Vaults are the primary architecture for current structured product deployments, while other vault types cover specialized use cases such as DVT-o | flag | Explicit, apparently-intentional reference distinguishing current Core Vaults from earlier/legacy generations — reads as already-correct labeling, but flagged since it matches the raw pattern. |
| index.mdx | 17 | Audit reports across Core Vaults, MultiVault, Interoperable Vault, Simple LRT, and DVV. | flag | Explicit, apparently-intentional reference distinguishing current Core Vaults from earlier/legacy generations — reads as already-correct labeling, but flagged since it matches the raw pattern. |
| mellow-vaults-overview.mdx | 101 | Live DeFi integrations include Aave, Morpho, Euler, Fluid, Gearbox, Curve, Uniswap, Cowswap, Pendle, Symbiotic, and EigenLayer. | flag | Explicit, apparently-intentional reference distinguishing current Core Vaults from earlier/legacy generations — reads as already-correct labeling, but flagged since it matches the raw pattern. |
| mellow-vaults-overview.mdx | 120 | Earlier generations of Mellow infrastructure include Mellow ALM, MultiVault and Restaking Vaults, Interoperable Vaults, Simple LRT, and DVstETH. | flag | Explicit, apparently-intentional reference distinguishing current Core Vaults from earlier/legacy generations — reads as already-correct labeling, but flagged since it matches the raw pattern. |
| security.mdx | 3 | description: "Audit reports from StateMind, ChainSecurity, Sherlock, MixBytes, Nethermind, and Decurity across Core Vaults, MultiVault, Interoperable  | rewrite |  |
| security.mdx | 8 | Mellow LRT smart contracts has been thoroughly audited by multiple firms specializing in smart contract audit and Sherlock community contributors. | rewrite | High-traffic /security page describes Mellow's security posture almost entirely in legacy LRT/DVV terms with no Core Vaults framing — this is the exact "legacy vocab leaked into a high-traffic current-gen page" problem called out in the project background. Cross-ref Needs-human-decision (Andrey): which audit reports belong to which architecture. |
| security.mdx | 12 | StateMind Mellow LRT report with deployment | rewrite | High-traffic /security page describes Mellow's security posture almost entirely in legacy LRT/DVV terms with no Core Vaults framing — this is the exact "legacy vocab leaked into a high-traffic current-gen page" problem called out in the project background. Cross-ref Needs-human-decision (Andrey): which audit reports belong to which architecture. |
| security.mdx | 16 | Sherlock Mellow Modular LRTs Audit Report | rewrite | High-traffic /security page describes Mellow's security posture almost entirely in legacy LRT/DVV terms with no Core Vaults framing — this is the exact "legacy vocab leaked into a high-traffic current-gen page" problem called out in the project background. Cross-ref Needs-human-decision (Andrey): which audit reports belong to which architecture. |
| security.mdx | 19 | <Card title="MixBytes" icon="file-pdf" href="https://cdn.jsdelivr.net/gh/mellow-finance/docs@main/audit-reports/Mellow%20Finance%20Simple-LRT%20and%20 | rewrite | High-traffic /security page describes Mellow's security posture almost entirely in legacy LRT/DVV terms with no Core Vaults framing — this is the exact "legacy vocab leaked into a high-traffic current-gen page" problem called out in the project background. Cross-ref Needs-human-decision (Andrey): which audit reports belong to which architecture. |
| security.mdx | 20 | Mellow Finance Simple-LRT and DVV Vault Security Audit Report | rewrite | High-traffic /security page describes Mellow's security posture almost entirely in legacy LRT/DVV terms with no Core Vaults framing — this is the exact "legacy vocab leaked into a high-traffic current-gen page" problem called out in the project background. Cross-ref Needs-human-decision (Andrey): which audit reports belong to which architecture. |
| security.mdx | 24 | ChainSecurity Mellow Finance Mellow LRT Audit | rewrite | High-traffic /security page describes Mellow's security posture almost entirely in legacy LRT/DVV terms with no Core Vaults framing — this is the exact "legacy vocab leaked into a high-traffic current-gen page" problem called out in the project background. Cross-ref Needs-human-decision (Andrey): which audit reports belong to which architecture. |
| security.mdx | 110 | <br />DVV<br /> | rewrite | High-traffic /security page describes Mellow's security posture almost entirely in legacy LRT/DVV terms with no Core Vaults framing — this is the exact "legacy vocab leaked into a high-traffic current-gen page" problem called out in the project background. Cross-ref Needs-human-decision (Andrey): which audit reports belong to which architecture. |
| security.mdx | 113 | <Card title="MixBytes" icon="file-pdf" href="https://cdn.jsdelivr.net/gh/mellow-finance/docs@main/audit-reports/Mellow%20Finance%20Simple-LRT%20and%20 | rewrite | High-traffic /security page describes Mellow's security posture almost entirely in legacy LRT/DVV terms with no Core Vaults framing — this is the exact "legacy vocab leaked into a high-traffic current-gen page" problem called out in the project background. Cross-ref Needs-human-decision (Andrey): which audit reports belong to which architecture. |
| security.mdx | 114 | Mellow Finance Simple-LRT and DVV Vault Security Audit Report | rewrite | High-traffic /security page describes Mellow's security posture almost entirely in legacy LRT/DVV terms with no Core Vaults framing — this is the exact "legacy vocab leaked into a high-traffic current-gen page" problem called out in the project background. Cross-ref Needs-human-decision (Andrey): which audit reports belong to which architecture. |
| strategy-vault/streth-deployment.mdx | 16 | \| SyncDepositQueue(DVV) \| [0xbcdbf4d3b3b3345f8348c6aa557949deaa6bc57b](0xbcdbf4d3b3b3345f8348c6aa557949deaa6bc57b) \| | flag | DVV/DVstETH as a deployment-table label on a current-gen page — is this a real current deployment sharing the legacy product name, or misfiled content? See Needs-human-decision (Andrey). |

**Legacy-generation pages (informational only — rule does not apply there; per-file counts, not exploded to every line):**

| File | Hits |
|---|---|
| restaking-vaults/multivault/multi-deployments.mdx | 256 |
| restaking-vaults/simple-lrt/simple-deployments.mdx | 56 |
| restaking-vaults/multivault/architecture.mdx | 17 |
| restaking-vaults/simple-lrt/architecture.mdx | 10 |
| points/overview.mdx | 8 |
| points/points-in-symbiotic-pre-deposit-contracts.mdx | 7 |
| dvsteth-vault/overview.mdx | 6 |
| dvsteth-vault/dvv-deployment.mdx | 4 |
| points/defi-points-integration-instructions.mdx | 4 |
| resources/mellow-lrt-depreciated/index.mdx | 3 |
| restaking-vaults/interoperable-vaults/architecture.mdx | 3 |
| resources/mellow-lrt-depreciated/dvv-legacy-architecture.mdx | 1 |
| resources/mellow-lrt-depreciated/modules/delegatemodules/defaultbondmodule.mdx | 1 |
| restaking-vaults/interoperable-vaults/index.mdx | 1 |
| restaking-vaults/interoperable-vaults/overview.mdx | 1 |
| restaking-vaults/multivault/overview.mdx | 1 |
| restaking-vaults/simple-lrt/index.mdx | 1 |
| restaking-vaults/simple-lrt/overview.mdx | 1 |

### 2.2 First person
`(^|[^a-z])(we|our|ours|us|I)([^a-z]|$)` (case-insensitive)

Total hits: 149 — current: 9, lrt-legacy: 9, alm-legacy: 131.

Current-generation and shared-technical hits (full detail; most are `i.e.`/loop-variable false positives from the case-insensitive `I` match, marked below):

| Path | Line | Excerpt | Class | Note |
|---|---|---|---|---|
| core-vaults/architecture/hooks/lidodeposithook.mdx | 34 | 4. `ETH` (i.e. `address(0xEeeeeEeeeEeEeeEeEeEeeEEEeeeeEeeeeeeeEEeE)`) — directly deposited into `wstETH` | fix:none | False positive — regex match is "i.e." (case-insensitive "I"), not a first-person pronoun. No action. |
| core-vaults/architecture/index.mdx | 16 | Mellow’s response is our **Core Vaults** – infrastructure purpose-built for curators and asset managers to design, deploy, and scale structured produc | rewrite | Genuine first person ("our") in prose on a current-gen page. |
| core-vaults/architecture/index.mdx | 126 | 2. Time-sensitive request handling Oracle reports can only process a redemption request if at least `redeemInterval` seconds have passed since the req | fix:none | False positive — regex match is "i.e." (case-insensitive "I"), not a first-person pronoun. No action. |
| core-vaults/architecture/permissions/mellowacl.mdx | 14 | * Keep track of all active (i.e., assigned) roles in a dedicated set | fix:none | False positive — regex match is "i.e." (case-insensitive "I"), not a first-person pronoun. No action. |
| core-vaults/architecture/permissions/mellowacl.mdx | 37 | Returns the number of currently active roles (i.e., roles with at least one member). | fix:none | False positive — regex match is "i.e." (case-insensitive "I"), not a first-person pronoun. No action. |
| core-vaults/architecture/permissions/mellowacl.mdx | 45 | Returns `true` if the role is currently active (i.e., assigned to at least one account). | fix:none | False positive — regex match is "i.e." (case-insensitive "I"), not a first-person pronoun. No action. |
| core-vaults/architecture/queues/depositqueue.mdx | 44 | * Cancellation reverts if the request has already become claimable (i.e., processed by an oracle report). | fix:none | False positive — regex match is "i.e." (case-insensitive "I"), not a first-person pronoun. No action. |
| core-vaults/architecture/queues/signaturequeue.mdx | 15 | * **Stateless and removable** (i.e., does not accumulate shares or process claims) | fix:none | False positive — regex match is "i.e." (case-insensitive "I"), not a first-person pronoun. No action. |
| quickstart.mdx | 46 | Example: Need help? Reach out to us at [support@yourcompany.com](mailto:support@yourcompany.com). | rewrite | Genuine first person ("us") — also note: placeholder email support@yourcompany.com looks like unfinished boilerplate (see Section 5). |

**Legacy-generation pages:** body prose is exempt per Section 2 (149 total hits, 140 on legacy pages — overwhelmingly genuine "we/our" prose in mellow-alm/overview/api.mdx, strategy write-ups, and points/restaking-vaults docs, not actionable). Only headings/banners on legacy pages are in scope; one such hit was found and is listed above (dvsteth-vault/overview.mdx:20). Per-file counts for the remainder:

| File | Hits |
|---|---|
| mellow-alm/overview/api.mdx | 38 |
| mellow-alm/overview/strategies/lstrategy-1.mdx | 19 |
| mellow-alm/overview/strategies/lstrategy.mdx | 17 |
| mellow-alm/overview/tutorials/deploy-your-own-strategy.mdx | 15 |
| mellow-alm-toolkit/oracles/index.mdx | 9 |
| mellow-alm/overview/strategies/gearbox-strategy.mdx | 8 |
| mellow-alm/overview/strategies/uni-v3-boosted-strategy.mdx | 7 |
| mellow-alm/overview/architecture.mdx | 6 |
| mellow-alm-toolkit/strategy/index.mdx | 4 |
| mellow-alm/overview/faq.mdx | 2 |
| mellow-alm/overview/governance-parameters.mdx | 2 |
| mellow-alm/overview/tutorials/contracts-deployments.mdx | 2 |
| points/overview.mdx | 2 |
| points/points-in-symbiotic-pre-deposit-contracts.mdx | 2 |
| restaking-vaults/simple-lrt/architecture.mdx | 2 |
| mellow-alm/overview/definitions.mdx | 1 |
| mellow-alm/overview/index.mdx | 1 |
| resources/mellow-lrt-depreciated/index.mdx | 1 |
| restaking-vaults/multivault/architecture.mdx | 1 |

### 2.3 Em dash
Literal `—` (U+2014). Mechanic — applies to all pages, all generations. All hits classified `fix` (replace with en dash or rephrase) unless noted.

Total hits: 92.

| Path | Line | Excerpt | Class | Note |
|---|---|---|---|---|
| core-vaults-integration-guide.mdx | 12 | All deposit and redemption flows are time-buffered through an off-chain oracle — protecting depositors against flash-loan attacks and front-running by | fix |  |
| core-vaults-integration-guide.mdx | 32 | Step 1 — User calls deposit(assets, referral, merkleProof) | fix |  |
| core-vaults-integration-guide.mdx | 35 | Step 2 — Handle Report submitted handleReport(priceD18, depositTimestamp) | fix |  |
| core-vaults-integration-guide.mdx | 39 | Step 3 — User calls DepositQueue.claim(account) | fix |  |
| core-vaults-integration-guide.mdx | 48 | Step 1 — User calls deposit(assets, referral, merkleProof) | fix |  |
| core-vaults-integration-guide.mdx | 55 | Step 1 — User calls RedeemQueue.redeem(shares) | fix |  |
| core-vaults-integration-guide.mdx | 58 | Step 2 — Handle Report submitted handleReport(priceD18, redeemTimestamp) | fix |  |
| core-vaults-integration-guide.mdx | 61 | Step 3 — Curator calls RedeemQueue.handleBatches(n) | fix |  |
| core-vaults-integration-guide.mdx | 65 | Step 4 — User calls RedeemQueue.claim(receiver, timestamps[]) | fix |  |
| core-vaults-integration-guide.mdx | 132 | /** Decimals used for vault shares — use this when parsing redeem amounts */ | fix |  |
| core-vaults-integration-guide.mdx | 160 | /** Maximum value for Solidity uint224 — the deposit() assets parameter type */ | fix |  |
| core-vaults-integration-guide.mdx | 360 | ### 5.3 ERC20 ABI (subset — for approval) | fix |  |
| core-vaults-integration-guide.mdx | 394 | ### 5.4 Vault ABI (subset — for share balance lookup) | fix |  |
| core-vaults-integration-guide.mdx | 486 | Depositing submits tokens to a `DepositQueue` contract. For **async** queues the shares are not immediately available — an oracle processes the batch  | fix |  |
| core-vaults-integration-guide.mdx | 489 | Step 1 — approve token spend (ERC20 only) | fix |  |
| core-vaults-integration-guide.mdx | 490 | Step 2 — call deposit() | fix |  |
| core-vaults-integration-guide.mdx | 494 | Step 3 — call claim() to receive vault shares | fix |  |
| core-vaults-integration-guide.mdx | 578 | // 2. Whitelist check — read shareManager.flags() to see if this vault requires a proof | fix |  |
| core-vaults-integration-guide.mdx | 636 | // Check for pending request — only one pending request per user per queue is allowed | fix |  |
| core-vaults-integration-guide.mdx | 645 | throw new Error('Pending deposit request already exists — cancel or claim it first') | fix |  |
| core-vaults-integration-guide.mdx | 695 | **Merkle proof & whitelisting:** Pass `[]` for public vaults. When `shareManager.flags().hasWhitelist === true`, the vault is permissioned — `deposit( | fix |  |
| core-vaults-integration-guide.mdx | 704 | You **cannot** cancel once the request is claimable — call `claim` instead. | fix |  |
| core-vaults-integration-guide.mdx | 710 | Call `requestOf(userAddress)` on the deposit queue. Check `timestamp > 0n` — if zero, there is no pending request. | fix |  |
| core-vaults-integration-guide.mdx | 714 | Call `claimableOf(userAddress)`. If `> 0n`, the oracle has already processed the request — you must claim it, not cancel it. | fix |  |
| core-vaults-integration-guide.mdx | 718 | Call `cancelDepositRequest()`. This function takes no arguments — it cancels the caller's own request. | fix |  |
| core-vaults-integration-guide.mdx | 745 | throw new Error('Request already processed — call claim() instead of cancel') | fix |  |
| core-vaults-integration-guide.mdx | 773 | If `claimableOf` returns `0n`, call `requestOf(userAddress)`. If `timestamp > 0n`, the oracle has not yet processed the request — wait and retry later | fix |  |
| core-vaults-integration-guide.mdx | 804 | throw new Error('Deposit is pending oracle processing — try again later') | fix |  |
| core-vaults-integration-guide.mdx | 824 | Redemption burns vault shares and, after oracle processing, returns the underlying asset to the user. Redeem queues are **always async** — there is al | fix |  |
| core-vaults-integration-guide.mdx | 827 | Step 1 — call redeem(shares) | fix |  |
| core-vaults-integration-guide.mdx | 831 | Step 2 — call claim(receiver, timestamps) | fix |  |
| core-vaults-integration-guide.mdx | 847 | * Call `shareManager.activeSharesOf(userAddress)` — this returns only shares that are **not** currently locked in a pending redeem request. Use `share | fix |  |
| core-vaults-integration-guide.mdx | 851 | Parse the share amount using **vault decimals** (`vault.decimals`), **not** the token's decimals. This is a common mistake — the share token uses the  | fix |  |
| core-vaults-integration-guide.mdx | 859 | Call `redeem(parsedShares)`. No ETH value, no ERC20 approval — the vault contract locks shares directly from the caller. | fix |  |
| core-vaults-integration-guide.mdx | 872 | **Requests cannot be cancelled.** Once submitted, a redemption request is permanent. This is intentional — cancellable redemptions would allow yield-g | fix |  |
| core-vaults-integration-guide.mdx | 876 | **Settlement flow:** After `redeem()`, two off-chain steps must happen before you can claim: (1) the oracle calls `handleReport()` to price the batch, | fix |  |
| core-vaults-integration-guide.mdx | 911 | // 4. Submit redeem — no approval required, vault locks shares from caller | fix |  |
| core-vaults-integration-guide.mdx | 948 | Extract the `timestamp` field from each claimable request. Cast to `number` — timestamps are `uint32` values, safely representable as JavaScript numbe | fix |  |
| core-vaults-integration-guide.mdx | 957 | **`claim()` is idempotent.** Non-claimable or already-claimed timestamps are silently skipped — the contract does not revert. You may safely pass all  | fix |  |
| core-vaults-integration-guide.mdx | 992 | `${allRequests.length} redemption request(s) are pending oracle processing — try again later`, | fix |  |
| core-vaults-integration-guide.mdx | 996 | // 3. Extract timestamps as number[] (uint32 — safe as JS number) | fix |  |
| core-vaults-integration-guide.mdx | 1021 | Fees in Mellow Core Vaults are paid in **vault shares**, not in underlying assets. The `FeeManager` contract calculates and deducts fees automatically | fix |  |
| core-vaults-integration-guide.mdx | 1037 | \| `ClaimableRequestExists` \| DepositQueue \| `cancelDepositRequest()` called when request is already claimable                     \| The oracle process | fix |  |
| core-vaults-integration-guide.mdx | 1040 | \| `DepositNotAllowed`      \| DepositQueue \| Deposit rejected — queue paused, or vault has a whitelist and address is not included \| Check `flags.hasWh | fix |  |
| core-vaults-integration-guide.mdx | 1052 | \| `DepositRequestClaimed(account, shares, timestamp)`          \| DepositQueue \| `claim()` succeeds — user received vault shares                        | fix |  |
| core-vaults-integration-guide.mdx | 1053 | \| `DepositRequestCanceled(account, assets, timestamp)`         \| DepositQueue \| `cancelDepositRequest()` succeeds — tokens returned                    | fix |  |
| core-vaults-integration-guide.mdx | 1055 | \| `RedeemRequestsHandled(counter, demand)`                     \| RedeemQueue  \| Curator called `handleBatches()` and settled one or more batches — **l | fix |  |
| core-vaults-integration-guide.mdx | 1056 | \| `RedeemRequestClaimed(account, receiver, assets, timestamp)` \| RedeemQueue  \| `claim()` succeeds — user received underlying assets                   | fix |  |
| core-vaults/architecture/hooks/basicredeemhook.mdx | 58 | * Assumes vault validates which hook is active — no permissioning within the hook itself. | fix |  |
| core-vaults/architecture/hooks/lidodeposithook.mdx | 31 | 1. `wstETH` — forwarded directly | fix |  |
| core-vaults/architecture/hooks/lidodeposithook.mdx | 32 | 2. `stETH` — wrapped into `wstETH` via `IWSTETH(wsteth).wrap()` | fix |  |
| core-vaults/architecture/hooks/lidodeposithook.mdx | 33 | 3. `WETH` — unwrapped into ETH via `IWETH(weth).withdraw()`, then deposited into `wstETH` | fix |  |
| core-vaults/architecture/hooks/lidodeposithook.mdx | 34 | 4. `ETH` (i.e. `address(0xEeeeeEeeeEeEeeEeEeEeeEEEeeeeEeeeeeeeEEeE)`) — directly deposited into `wstETH` | fix |  |
| core-vaults/architecture/hooks/lidodeposithook.mdx | 53 | * `UnsupportedAsset(address asset)` — Thrown if the provided asset is neither `wstETH`, `stETH`, `WETH`, nor `ETH` | fix |  |
| core-vaults/architecture/index.mdx | 91 | 3. Lazy Claiming Deposits are converted into shares during oracle processing, but users must call the claim function (in `ShareModule`, `ShareManager` | fix |  |
| core-vaults/architecture/index.mdx | 105 | 2. Liquidity settlement — Vault liquidity is pulled asynchronously, allowing the curator to finalize withdrawals and perform asset swaps before proces | fix |  |
| core-vaults/architecture/index.mdx | 126 | 2. Time-sensitive request handling Oracle reports can only process a redemption request if at least `redeemInterval` seconds have passed since the req | fix |  |
| core-vaults/architecture/libraries/fenwicktreelibrary.mdx | 16 | * Index bounds are enforced — access beyond the current capacity reverts with `IndexOutOfBounds()`. | fix |  |
| core-vaults/architecture/modules/basemodule.mdx | 33 | * `slot` — The `bytes32` identifier of the storage slot. | fix |  |
| core-vaults/architecture/modules/basemodule.mdx | 49 | * `IERC721Receiver.onERC721Received.selector` — confirms compliance. | fix |  |
| core-vaults/architecture/modules/verifiermodule.mdx | 19 | * `name_` — Unique identifier used to namespace the storage slot. | fix |  |
| core-vaults/architecture/modules/verifiermodule.mdx | 20 | * `version_` — Version number used for slot derivation. | fix |  |
| core-vaults/architecture/modules/verifiermodule.mdx | 34 | * `IVerifier` — The verifier contract associated with the module. | fix |  |
| core-vaults/architecture/modules/verifiermodule.mdx | 48 | * `verifier_` — Address of the verifier contract. | fix |  |
| core-vaults/architecture/modules/verifiermodule.mdx | 66 | * `VerifierModuleStorage` — Storage struct holding verifier address. | fix |  |
| core-vaults/architecture/oracle.mdx | 140 | * No pricing logic oracles trust external feeds — price validation is local | fix |  |
| strategy-vault/streth-deployment.mdx | 3 | description: "strETH contract addresses across Mainnet, Arbitrum, Plasma — vault, deposit/redeem queues, oracle, share/fee/risk managers, 8 subvaults  | fix |  |
| points/points-in-symbiotic-pre-deposit-contracts.mdx | 16 | Gaming the Loyalty Points program — i.e. using the same LRT to generate rewards multiple times by selling LRT for ETH to repeat the process is not enc | fix |  |
| points/points-in-symbiotic-pre-deposit-contracts.mdx | 18 | \*-Initial limit of 41290 wstETH was filled at block height of 20070701 and 210600 — at 20227052, info on new limit raise fill, be there any, will be  | fix |  |
| resources/mellow-lrt-depreciated/utils/defaultaccesscontrol.mdx | 7 | `DefaultAccessControl` contract provides a flexible role-based access control mechanism. It leverages the OpenZeppelin `AccessControlEnumerable`. The  | fix |  |
| restaking-vaults/interoperable-vaults/architecture.mdx | 12 | 4. An OFT ( LayerZero Omnichain Fungible Token Standard) counterpart is minted on the target chain — Ethereum mainnet, where Mellow restaking vaults a | fix |  |
| restaking-vaults/interoperable-vaults/architecture.mdx | 14 | 6. The user earns rewards in two ways: first, through L2 incentives—some of the vault’s accumulated liquidity is staked directly, and some is used in  | fix |  |
| mellow-alm/mellow-alm-toolkit/index.mdx | 3 | description: "Concentrated liquidity management toolkit for AMMs (Uniswap, Sushiswap, PancakeSwap, Velodrome) — bootstrapping, position management, ri | fix |  |
| mellow-alm/overview/strategies/uni-v3-boosted-strategy.mdx | 15 | UniV3 Boosted strategy is a strategy for a pair of tokens X and Y, for example — WBTC/WETH or USDC/WETH. The entire capital of the strategy is divided | fix |  |
| mellow-alm/overview/strategies/uni-v3-boosted-strategy.mdx | 70 | `$$u_{1}= 1 - u_2 - u_3 =\frac{2\sqrt{c}-\sqrt{a}-\frac{c}{\sqrt{b}}}{2\sqrt{c}-\sqrt{a_{0}}-\frac{c}{\sqrt{b_{0}}}}$$` — fraction of capital to Unisw | fix |  |
| mellow-alm/overview/strategies/uni-v3-boosted-strategy.mdx | 72 | `$$u_{2}=\frac{\frac{c}{\sqrt{b}}-\frac{c}{\sqrt{b_{0}}}}{2\sqrt{c}-\sqrt{a_{0}}-\frac{c}{\sqrt{b_{0}}}}$$` — fraction of capital to the yield protoco | fix |  |
| mellow-alm/overview/strategies/uni-v3-boosted-strategy.mdx | 74 | `$$u_{3}=\frac{\sqrt{a}-\sqrt{a_{0}}}{2\sqrt{c}-\sqrt{a_{0}}-\frac{c}{\sqrt{b_{0}}}}$$` — fraction of capital to the yield protocol in token Y. where: | fix |  |
| mellow-alm/overview/strategies/uni-v3-boosted-strategy.mdx | 76 | * `$$a_0$$`, `$$b_0$$` — domain interval in Uniswap V3 | fix |  |
| mellow-alm/overview/strategies/uni-v3-boosted-strategy.mdx | 77 | * `$$a$$`, `$$b$$` — short interval in Uniswap V3 | fix |  |
| mellow-alm/overview/strategies/uni-v3-boosted-strategy.mdx | 78 | * `$$c$$` — current price | fix |  |
| mellow-alm/overview/strategies/uni-v3-boosted-strategy.mdx | 88 | * `$$x$$` — the amount of token X weis | fix |  |
| mellow-alm/overview/strategies/uni-v3-boosted-strategy.mdx | 89 | * `$$y$$`— the amount of token Y weis | fix |  |
| mellow-alm/overview/strategies/uni-v3-boosted-strategy.mdx | 90 | * `$$\sqrt{P}$$` — square root of the current price | fix |  |
| mellow-alm/overview/strategies/uni-v3-boosted-strategy.mdx | 91 | * `$$\sqrt{p_a}$$` — square root of the price at the left border of the position interval | fix |  |
| mellow-alm/overview/strategies/uni-v3-boosted-strategy.mdx | 92 | * `$$\sqrt{p_b}$$` — square root of the price at the right border of the position interval | fix |  |
| mellow-alm/overview/strategies/uni-v3-boosted-strategy.mdx | 162 | 1. `$$strategyCapital = L(c (\sqrt{b_0} - \sqrt{b}) / \sqrt{b b_0} + \sqrt{b} - \sqrt{a_0})$$` — capital of the strategy in token Y if there has not b | fix |  |
| mellow-alm/overview/strategies/uni-v3-boosted-strategy.mdx | 163 | 2. `$$uniswapCapital = L (c(\sqrt{b_0} - \sqrt{c}) / \sqrt{c b_0} + \sqrt{c} - \sqrt{a_0})$$` — capital of uniswap position in token Y | fix |  |
| mellow-alm/overview/strategies/uni-v3-boosted-strategy.mdx | 164 | 3. `$$L$$`— liquidity of the uniswap position in terms of UniswapV3 | fix |  |
| mellow-alm/overview/strategies/uni-v3-boosted-strategy.mdx | 169 | 1. `$$strategyRatioX = c(\sqrt{b_0} - \sqrt{b}) / (\sqrt{b b_0} * (\sqrt{b} - \sqrt{a_0}) + c(\sqrt{b_0} - \sqrt{b}))$$` — the ratio of token X to the | fix |  |
| mellow-alm/overview/strategies/uni-v3-boosted-strategy.mdx | 170 | 2. `$$uniswapRatioX = c(\sqrt{b_0} - \sqrt{c}) / (\sqrt{c b_0} * (\sqrt{c} - \sqrt{a_0}) + c(\sqrt{b_0} - \sqrt{c}))$$` — the ratio of token X to the  | fix |  |
| mellow-alm/overview/strategies/uni-v3-boosted-strategy.mdx | 176 | 2. `$$uniswapCapital = L (c(\sqrt{b_0} - \sqrt{c}) / \sqrt{c b_0} + \sqrt{c} - \sqrt{a_0})$$` — capital of uniswap position in token Y | fix |  |
| mellow-alm/overview/strategies/uni-v3-boosted-strategy.mdx | 177 | 3. `$$L$$` — liquidity of the uniswap position in terms of UniswapV3 | fix |  |

### 2.4 "on-chain" (should be "onchain")
Mechanic — applies to all pages. All hits classified `fix`.

Total hits: 15.

| Path | Line | Excerpt | Class | Note |
|---|---|---|---|---|
| core-vaults-integration-guide.mdx | 1042 | \| `InsufficientBalance`    \| Both         \| Token or share balance too low                                                        \| Validate balance o | fix |  |
| core-vaults-integration-guide.mdx | 1047 | Listen for these events to drive UI state or index on-chain activity. | fix |  |
| core-vaults/architecture/libraries/transferlibrary.mdx | 5 | This utility abstracts away the differences between transferring native ETH and ERC20 tokens by introducing a unified interface for both sending and r | fix |  |
| core-vaults/architecture/managers/basicsharemanager.mdx | 9 | This contract is intended for setups where shares are not tokenized on-chain as ERC20s but are still tracked internally using the ERC20Upgradeable sto | fix |  |
| core-vaults/architecture/managers/riskmanager.mdx | 5 | **On-chain risk control and allocation policy manager for modular vaults.** | fix |  |
| core-vaults/architecture/permissions/verifier.mdx | 9 | * On-chain allowlists using hashed shortened calls (`CompactCall`) | fix |  |
| core-vaults/architecture/permissions/verifier.mdx | 18 | * Grants or revokes execution rights using on-chain and off-chain mechanisms | fix |  |
| core-vaults/architecture/queues/signaturedepositqueue.mdx | 7 | `SignatureDepositQueue` extends `SignatureQueue` to enable **instant deposit** of assets into a vault, bypassing the standard on-chain `DepositQueue`  | fix |  |
| core-vaults/architecture/queues/signaturedepositqueue.mdx | 9 | This contract is optimized for high-trust environments requiring immediate asset onboarding while maintaining on-chain price safety guarantees. | fix |  |
| core-vaults/architecture/queues/signaturedepositqueue.mdx | 25 | 2. User submits the order on-chain by calling `deposit` function: | fix |  |
| core-vaults/architecture/queues/signaturequeue.mdx | 14 | * **Oracle price validation** enforced on-chain | fix |  |
| core-vaults/architecture/queues/signatureredeemqueue.mdx | 7 | `SignatureRedeemQueue` extends `SignatureQueue` to enable **instant share redemption** from a vault without the usual delay of on-chain oracle process | fix |  |
| mellow-alm-toolkit/core.mdx | 210 | 5. **Postprocessing**: After the callback execution, updates the ManagedPositions within the Core contract to reflect the new state, ensuring the inte | fix |  |
| mellow-alm/overview/strategies/lstrategy-1.mdx | 71 | In the other case, we have to swap some amount of an excessively presented token to get the second token and align the ratio. We do swaps using Cowswa | fix |  |
| mellow-alm/overview/strategies/lstrategy.mdx | 71 | In the other case, we have to swap some amount of an excessively presented token to get the second token and align the ratio. We do swaps using Cowswa | fix |  |

### 2.5 Curly quotes
U+2018 / U+2019 / U+201C / U+201D. Mechanic — applies to all pages. All hits classified `fix` (straight quotes).

Total hits: 61.

| Path | Line | Excerpt | Class | Note |
|---|---|---|---|---|
| core-vaults/architecture/hooks/basicredeemhook.mdx | 9 | This hook is typically invoked by a vault’s redemption queue or during `redeem()` operations when assets must be made liquid. | fix |  |
| core-vaults/architecture/hooks/redirectingdeposithook.mdx | 14 | * Delegates decision-making to the vault’s configured `RiskManager`, which determines per-subvault deposit limits. | fix |  |
| core-vaults/architecture/index.mdx | 12 | Meanwhile, the latest wave of adoption is bringing thousands of new participants from traditional markets into crypto. These users aren’t looking for  | fix |  |
| core-vaults/architecture/index.mdx | 16 | Mellow’s response is our **Core Vaults** – infrastructure purpose-built for curators and asset managers to design, deploy, and scale structured produc | fix |  |
| core-vaults/architecture/index.mdx | 70 | * The queue handles deposit requests that are pending for at least `depositInterval` seconds (the interval is specified in the oracle’s security param | fix |  |
| core-vaults/architecture/index.mdx | 71 | * The contract stores a `(timestamp, reducedByDepositFeePriceD18)` pair in the `prices` array. This value is used to convert accumulated assets into s | fix |  |
| core-vaults/architecture/index.mdx | 353 | If actual balances deviate significantly from the stored `balance` values due to oracle drift, delayed execution, or protocol-side changes, a **truste | fix |  |
| core-vaults/architecture/libraries/slotlibrary.mdx | 5 | This library generates unique and collision-resistant storage slots for use in upgradeable Solidity contracts. It ensures that different modules or in | fix |  |
| core-vaults/architecture/managers/riskmanager.mdx | 40 | * `MODIFY_SUBVAULT_BALANCE_ROLE`: Can update a subvault’s internal balance. | fix |  |
| core-vaults/architecture/managers/riskmanager.mdx | 91 | If actual balances deviate significantly from the stored `balance` values due to oracle drift, delayed execution, or protocol-side changes, a **truste | fix |  |
| core-vaults/architecture/managers/sharemanager.mdx | 63 | * `claimShares(...)`: Claims shares from the vault’s `IShareModule`. | fix |  |
| core-vaults/architecture/managers/sharemanager.mdx | 67 | * `burn(...)`: Burns a user’s shares (only queue). | fix |  |
| core-vaults/architecture/modules/sharemodule.mdx | 59 | * `callHook(assets)`: Calls the queue’s associated hook. Transfers assets to the queue if redeem. | fix |  |
| core-vaults/architecture/permissions/consensus.mdx | 62 | * Valid according to the signer’s configured signature type | fix |  |
| core-vaults/architecture/permissions/mellowacl.mdx | 7 | `MellowACL` is a lightweight but extendable access control layer that wraps OpenZeppelin’s `AccessControlEnumerableUpgradeable`. It introduces automat | fix |  |
| core-vaults/architecture/permissions/protocols/eigenlayerverifier.mdx | 36 | * Setting immutable references to EigenLayer’s: | fix |  |
| core-vaults/architecture/permissions/protocols/ownedcustomverifier.mdx | 16 | * `MellowACL`: Upgradeable, role-based access control module compatible with OpenZeppelin’s `AccessControl` | fix |  |
| core-vaults/architecture/permissions/protocols/ownedcustomverifier.mdx | 40 | * Sets `admin` as the contract’s `DEFAULT_ADMIN_ROLE` | fix |  |
| core-vaults/architecture/queues/depositqueue.mdx | 26 | * The queue handles deposit requests that are pending for longer than `depositInterval` seconds (the interval is specified in the oracle’s security pa | fix |  |
| core-vaults/architecture/queues/depositqueue.mdx | 51 | * `requestOf(address account)`: Returns the `(timestamp, amount)` tuple of a user’s current pending request. | fix |  |
| core-vaults/architecture/queues/depositqueue.mdx | 94 | * Each user’s **claimable shares** are finalized only during subsequent calling `claim()`. | fix |  |
| core-vaults/architecture/queues/signaturedepositqueue.mdx | 51 | 2. Increments the caller’s nonce to prevent replay | fix |  |
| core-vaults/architecture/queues/signaturequeue.mdx | 74 | After signature verification, `SignatureQueue` uses the vault’s `Oracle` to validate the price: | fix |  |
| core-vaults/architecture/vaults/subvault.mdx | 40 | * `name_`: A unique string identifier for the deployment (e.g., “Mellow”). | fix |  |
| core-vaults/overview.mdx | 6 | Core Vaults are Mellow’s primary vault architecture for deploying curated onchain structured products. | fix |  |
| core-vaults/overview.mdx | 14 | Core Vaults operate under a curated model. Depositors supply capital to the vault, while curators operate strategy logic within predefined guardrails. | fix |  |
| index.mdx | 8 | Mellow vaults operate under a curated model. Depositors provide capital, while designated curators define and operate the strategy within predefined o | fix |  |
| strategy-vault/overview.mdx | 14 | You can deposit ETH, WETH, or wstETH to receive strETH share tokens of the stRATEGY vault. Once submitted, your deposit request will appear as pending | fix |  |
| strategy-vault/overview.mdx | 36 | * Platform fee: 1% annually, pro-rated for the time your deposited tokens stay in the vault, is built into the strETH token’s price. | fix |  |
| strategy-vault/overview.mdx | 37 | * Performance fee: 10% of the rewards accrued is deducted from gains before it's reflected in the strETH token’s price. | fix |  |
| strategy-vault/rewards.mdx | 7 | When you deposit your tokens, you receive strETH tokens that represent your portion of the vault.  While your strETH token balance stays the same, the | fix |  |
| strategy-vault/rewards.mdx | 13 | When your funds enter the vault and strETH tokens are generated and can be claimed in the Lido UI or Mellow UI. Not claiming your tokens _won’t affect | fix |  |
| strategy-vault/rewards.mdx | 25 | Please note that APY figures are only estimates and subject to change at any time. Past performance is not a guarantee of future results. Rewards are  | fix |  |
| dvsteth-vault/overview.mdx | 6 | DVstETH is a wrapped Liquid Staking Token powered by the Lido protocol’s wstETH, allowing vault depositors to reuse their staking receipts across the  | fix |  |
| dvsteth-vault/overview.mdx | 14 | Users will receive incentives by depositing (W)ETH into the Vault, with the number of incentives calculated based on how much stake they have in the V | fix |  |
| restaking-vaults/interoperable-vaults/architecture.mdx | 14 | 6. The user earns rewards in two ways: first, through L2 incentives—some of the vault’s accumulated liquidity is staked directly, and some is used in  | fix |  |
| restaking-vaults/interoperable-vaults/overview.mdx | 5 | Interoperable Vaults facilitate cross‐chain restaking, allowing users to deposit assets on EVM networks and receive vault shares, which are then resta | fix |  |
| restaking-vaults/multivault/architecture.mdx | 147 | When a SYMBIOTIC-type subvault is added (Symbiotic Vault), a corresponding **SymbioticWithdrawalQueue** is created (if not already present in adapter’ | fix |  |
| restaking-vaults/multivault/overview.mdx | 5 | The MultiVault aggregates multiple isolated vaults (“subvaults”) into a single top-level vault, enabling cross-protocol liquidity rebalancing, particu | fix |  |
| restaking-vaults/simple-lrt/architecture.mdx | 5 | SimpleLRT is a modular “liquid restaking primitive” built on a series of vault contracts tailored to different risk profiles. | fix |  |
| restaking-vaults/simple-lrt/architecture.mdx | 33 | 7. **getBalances(userAddress)**: Returns four parameters related to the user’s holdings: | fix |  |
| restaking-vaults/simple-lrt/architecture.mdx | 35 | * `accountInstantAssets`: The portion of the user’s assets that can be instantly withdrawn from the vault. | fix |  |
| restaking-vaults/simple-lrt/architecture.mdx | 37 | * `accountInstantShares`: The portion of the user’s shares that can be instantly withdrawn. | fix |  |
| restaking-vaults/simple-lrt/architecture.mdx | 69 | 1. User transfers ETH/WETH/STETH/WSTETH to `EthWrapper`, where it’s wrapped into the WSTETH. In case of other base asset, skip to step 2. | fix |  |
| restaking-vaults/simple-lrt/architecture.mdx | 81 | 3. If there’s some WSTETH on the vault balance then it’s immediately used to fulfill the request | fix |  |
| restaking-vaults/simple-lrt/architecture.mdx | 84 | 6. `SymbioticWithdrawalQueue` checks the last epoch the user claimed assets (`epoch` variable). It then updates the data for account’s available funds | fix |  |
| mellow-alm-toolkit/oracles/index.mdx | 11 | As mentioned above, spot oracles provide price estimations for tokens that are tradable on DEXes (e.g. governance tokens like `UNI`), which _have no u | fix |  |
| mellow-alm-toolkit/oracles/index.mdx | 31 | There’s also a **Delisting Criteria** for the token: | fix |  |
| mellow-alm-toolkit/oracles/index.mdx | 111 | 2. **Existence of Prior Swaps in Current Block:** Conversely, if swaps have occurred earlier in the current block (latest observation timestamp doens’ | fix |  |
| mellow-alm-toolkit/strategy/pulsestrategymodule.mdx | 94 | * Calculates the lower boundary (`targetLower`) of the target position and the liquidity ratio for the lower range (`lowerLiquidityRatioX96`) based on | fix |  |
| mellow-alm-toolkit/strategy/pulsestrategymodule.mdx | 162 | * Ensures `maxLiquidityRatioDeviationX96` is zero, as it’s only relevant to `Tamper`. Any non-zero value results in `InvalidParams`. | fix |  |
| mellow-alm/overview/strategies/gearbox-strategy.mdx | 19 | Gearbox V2 doesn’t have an option of a partial withdrawal of funds from its credit accounts. Hence, there is a new RootVault (named GearboxRootVault)  | fix |  |
| mellow-alm/overview/strategies/gearbox-strategy.mdx | 27 | The vault has a parameter _marginalFactor_ (let’s call it `$$M > 1$$` for simplicity). If the vault has `$$x$$` USD of pure capital (total assets - to | fix |  |
| mellow-alm/overview/strategies/gearbox-strategy.mdx | 29 | Here are the main functions’ logic of the vault: | fix |  |
| mellow-alm/overview/strategies/gearbox-strategy.mdx | 34 | * \__pull()_ closes the credit account, sends the required amount of money to the ERC20Vault, and the remaining funds remain in the vault. Either pays | fix |  |
| mellow-alm/overview/strategies/gearbox-strategy.mdx | 35 | * _adjustPosition()_ adjusts the amount of total assets to `$$M \cdot x$$`. This means, if the amount of the current assets is smaller, the vault take | fix |  |
| mellow-alm/overview/strategies/gearbox-strategy.mdx | 42 | * If the vault’s token doesn’t equal the token of the credit account, we maintain that _amount ≤ tvl,_ where _amount_ is how many vault’s tokens are i | fix |  |
| mellow-alm/overview/strategies/gearbox-strategy.mdx | 43 | * Before _pull()_, tokens of the credit account are not swapped to the tokens of the vault if it’s not necessary. After closing the credit account, bo | fix |  |
| mellow-alm/overview/strategies/uni-v3-boosted-strategy.mdx | 9 | Consider UniV3 ETH/USDC 0.05% pool and assume we’d like to put our liquidity into the \[1000, 2000] price range (we refer to it as the Domain price ra | fix |  |
| mellow-alm/overview/strategies/uni-v3-boosted-strategy.mdx | 59 | 3. If the deviation between current amounts and expected amounts is large enough, then the following transfers are done. “enough” is determined by the | fix |  |
| mellow-alm/overview/strategies/uni-v3-boosted-strategy.mdx | 94 | Thus, if we have `$$x_0$$` amount of weis in token X and we’d like to calculate how much we need to convert to token Y, we are solving the following s | fix |  |

### 2.6 Emoji
Ranges U+1F300–U+1FAFF, U+2600–U+27BF, U+2B00–U+2BFF. Mechanic — applies to all pages. All hits classified `fix` (remove).

Total hits: 1734 — current: 11, alm-legacy: 1722, lrt-legacy: 1.

Full detail for every file except the two giant alm-legacy API/spec dumps (aggregated below):

| Path | Line | Excerpt | Class | Note |
|---|---|---|---|---|
| core-vaults/architecture/index.mdx | 49 | #### 📥 Deposit Queue | fix |  |
| core-vaults/architecture/index.mdx | 98 | #### 📤 Redeem Queue | fix |  |
| core-vaults/architecture/index.mdx | 131 | #### 🔏 Signature Deposit and Redeem Queues | fix |  |
| core-vaults/architecture/index.mdx | 166 | #### 💼 Vault | fix |  |
| core-vaults/architecture/index.mdx | 177 | #### 🗃️ Subvault | fix |  |
| core-vaults/architecture/index.mdx | 188 | #### 👁️ Verifier | fix |  |
| core-vaults/architecture/index.mdx | 211 | #### 🔮 Oracle | fix |  |
| core-vaults/architecture/index.mdx | 269 | #### 📊 Share Manager | fix |  |
| core-vaults/architecture/index.mdx | 303 | #### 💰 Fee Manager | fix |  |
| core-vaults/architecture/index.mdx | 332 | #### 🎯 Risk Manager | fix |  |
| core-vaults/architecture/index.mdx | 361 | #### 🔐Access Control | fix |  |
| resources/mellow-lrt-depreciated/vaultconfigurator.mdx | 2 | title: "🔧 VaultConfigurator" | fix |  |
| mellow-alm-toolkit/oracles/index.mdx | 58 | \<aside> 💡 When utilizing the Spot Oracle, it's important to always be aware that the token in question could potentially be delisted. | fix |  |
| mellow-alm-toolkit/oracles/index.mdx | 102 | \<aside> 💡 This mechanism, while effective, still permits a degree of price manipulation, which varies across different pools. It is crucial for the p | fix |  |
| mellow-alm-toolkit/oracles/index.mdx | 113 | \<aside> 💡 If the risk of cross-block sandwich attacks is negligible, relying solely on this oracle is sufficient. However, for enhanced security agai | fix |  |

**Aggregated (giant auto-generated alm-legacy files):**

| File | Hits | Line range | Pattern |
|---|---|---|---|
| mellow-alm/overview/api.mdx | 982 | 9–11116 | Overwhelmingly ⛽ gas-cost markers and ✅ test-checklist bullets throughout an auto-generated-style API/test spec |
| mellow-alm/overview/contracts-specs.mdx | 737 | 21–2371 | Same ⛽/✅ pattern in a contract spec dump |
| core-vaults/architecture/index.mdx | 11 | 49–361 | Emoji used as heading decoration (`#### 📥 Deposit Queue`, etc.) — see full listing above (current-gen, classified `fix`) |

### 2.7 revert(s|ed)?
Case-insensitive. Prose should say "cannot be executed" instead. Code blocks and lines naming an actual Solidity error/revert reason are exempt (see Methodology notes). Applies to all pages (mechanic), with the Solidity-error carve-out applying everywhere too.

Total raw hits (pre code-fence exclusion already applied in the script): 430 — current: 38, alm-legacy: 390, lrt-legacy: 2.

Full detail for every file except the two giant alm-legacy files (aggregated below):

| Path | Line | Excerpt | Class | Note |
|---|---|---|---|---|
| core-vaults-and-other-approaches.mdx | 54 | \| Operation-level risk         \| Permissions are external or vault-boundary-only \| 60+ onchain permission types scoped by strategy, asset, operator, a | rewrite | Generic prose "revert(s)" with no named error — apply revert→"cannot be executed" rule. Recurring near-verbatim across several current-gen pages ("Operations outside the mandate revert"). |
| core-vaults-and-other-approaches.mdx | 67 | Core Vaults separate what the vault can do from what it is allowed to do. Execution is handled by shared vault logic. Permissions are defined by verif | rewrite | Generic prose "revert(s)" with no named error — apply revert→"cannot be executed" rule. Recurring near-verbatim across several current-gen pages ("Operations outside the mandate revert"). |
| core-vaults-for-rwa-allocation.mdx | 22 | A mandate is the set of rules that defines how a vault may operate. It includes eligible depositors, supported assets and issuers, approved venues, al | rewrite | Generic prose "revert(s)" with no named error — apply revert→"cannot be executed" rule. Recurring near-verbatim across several current-gen pages ("Operations outside the mandate revert"). |
| core-vaults-for-rwa-allocation.mdx | 92 | RWA allocation suits agent operation: continuous, mandate-bounded decisions across instruments with different settlement and liquidity profiles. The s | rewrite | Generic prose "revert(s)" with no named error — apply revert→"cannot be executed" rule. Recurring near-verbatim across several current-gen pages ("Operations outside the mandate revert"). |
| core-vaults-for-stablecoins.mdx | 20 | A mandate is the set of rules that defines how a vault may operate. It includes eligible depositors, supported assets, approved venues, allocation cap | rewrite | Generic prose "revert(s)" with no named error — apply revert→"cannot be executed" rule. Recurring near-verbatim across several current-gen pages ("Operations outside the mandate revert"). |
| core-vaults-for-stablecoins.mdx | 112 | Core Vaults provide this through the verifier model – approved targets, functions, assets, allocation bands, NAV constraints, and time-locked operatio | rewrite | Generic prose "revert(s)" with no named error — apply revert→"cannot be executed" rule. Recurring near-verbatim across several current-gen pages ("Operations outside the mandate revert"). |
| core-vaults-integration-guide.mdx | 509 | Read `shareManager.flags()`. If `flags.hasWhitelist === true`, call `shareManager.isDepositorWhitelisted(userAddress, merkleProof)`. If it returns `fa | flag | Not individually reviewed — verify whether this names an actual Solidity error (exempt) or is generic prose (rewrite). |
| core-vaults-integration-guide.mdx | 537 | * If `currentAllowance > 0n`, send `approve(queueAddress, 0n)` first. This is required for tokens like USDT that revert if you set a non-zero allowanc | flag | Describes third-party token (USDT) revert behavior, not Mellow's own semantics — mechanical replacement with "cannot be executed" would misdescribe an external token's behavior. Needs editorial judgment. |
| core-vaults-integration-guide.mdx | 695 | **Merkle proof & whitelisting:** Pass `[]` for public vaults. When `shareManager.flags().hasWhitelist === true`, the vault is permissioned — `deposit( | flag | Not individually reviewed — verify whether this names an actual Solidity error (exempt) or is generic prose (rewrite). |
| core-vaults-integration-guide.mdx | 957 | **`claim()` is idempotent.** Non-claimable or already-claimed timestamps are silently skipped — the contract does not revert. You may safely pass all  | fix:none | Names an actual Solidity error / is a structural "Reverts if:"-style NatSpec section header — exempt per the Solidity-error carve-out. |
| core-vaults/architecture/index.mdx | 238 | * If `timeout` has not passed, and the report is not suspicious → revert `TooEarly` | fix:none | Names an actual Solidity error / is a structural "Reverts if:"-style NatSpec section header — exempt per the Solidity-error carve-out. |
| core-vaults/architecture/index.mdx | 240 | * `maxAbsolute` → revert `InvalidPrice` | fix:none | Names an actual Solidity error / is a structural "Reverts if:"-style NatSpec section header — exempt per the Solidity-error carve-out. |
| core-vaults/architecture/libraries/fenwicktreelibrary.mdx | 16 | * Index bounds are enforced — access beyond the current capacity reverts with `IndexOutOfBounds()`. | fix:none | Names an actual Solidity error / is a structural "Reverts if:"-style NatSpec section header — exempt per the Solidity-error carve-out. |
| core-vaults/architecture/libraries/fenwicktreelibrary.mdx | 35 | * Reverts with `InvalidLength()` if `length_ == 0` or not a power of two. | fix:none | Names an actual Solidity error / is a structural "Reverts if:"-style NatSpec section header — exempt per the Solidity-error carve-out. |
| core-vaults/architecture/libraries/fenwicktreelibrary.mdx | 47 | * Reverts with `InvalidLength()` on overflow. | fix:none | Names an actual Solidity error / is a structural "Reverts if:"-style NatSpec section header — exempt per the Solidity-error carve-out. |
| core-vaults/architecture/libraries/fenwicktreelibrary.mdx | 54 | * Reverts if index is out of bounds. | fix:none | Names an actual Solidity error / is a structural "Reverts if:"-style NatSpec section header — exempt per the Solidity-error carve-out. |
| core-vaults/architecture/libraries/transferlibrary.mdx | 34 | **Reverts if:** ETH transfer fails or ERC20 transfer fails via `SafeERC20`. | fix:none | Names an actual Solidity error / is a structural "Reverts if:"-style NatSpec section header — exempt per the Solidity-error carve-out. |
| core-vaults/architecture/libraries/transferlibrary.mdx | 49 | **Reverts if:** | fix:none | Names an actual Solidity error / is a structural "Reverts if:"-style NatSpec section header — exempt per the Solidity-error carve-out. |
| core-vaults/architecture/managers/basicsharemanager.mdx | 47 | Reverts if: | fix:none | Names an actual Solidity error / is a structural "Reverts if:"-style NatSpec section header — exempt per the Solidity-error carve-out. |
| core-vaults/architecture/managers/basicsharemanager.mdx | 58 | Reverts if: | fix:none | Names an actual Solidity error / is a structural "Reverts if:"-style NatSpec section header — exempt per the Solidity-error carve-out. |
| core-vaults/architecture/managers/sharemanager.mdx | 55 | * `updateChecks(from, to)`: Reverts on violations (paused actions, lockups, blacklisting, etc.). | fix:none | Names an actual Solidity error / is a structural "Reverts if:"-style NatSpec section header — exempt per the Solidity-error carve-out. |
| core-vaults/architecture/modules/subvaultmodule.mdx | 44 | * **Reverts**: With `NotVault()` if the caller is not the vault | fix:none | Names an actual Solidity error / is a structural "Reverts if:"-style NatSpec section header — exempt per the Solidity-error carve-out. |
| core-vaults/architecture/modules/vaultmodule.mdx | 57 | * Reverts with `NotConnected` if not already linked | fix:none | Names an actual Solidity error / is a structural "Reverts if:"-style NatSpec section header — exempt per the Solidity-error carve-out. |
| core-vaults/architecture/modules/vaultmodule.mdx | 62 | * Reverts with `InvalidSubvault`, `NotEntity`, or `AlreadyConnected` if checks fail | fix:none | Names an actual Solidity error / is a structural "Reverts if:"-style NatSpec section header — exempt per the Solidity-error carve-out. |
| core-vaults/architecture/modules/verifiermodule.mdx | 50 | **Reverts:** | fix:none | Names an actual Solidity error / is a structural "Reverts if:"-style NatSpec section header — exempt per the Solidity-error carve-out. |
| core-vaults/architecture/oracle.mdx | 98 | * If `timeout` has not passed, and the report is not suspicious → **revert** `TooEarly` | fix:none | Names an actual Solidity error / is a structural "Reverts if:"-style NatSpec section header — exempt per the Solidity-error carve-out. |
| core-vaults/architecture/oracle.mdx | 100 | * Too far off → **revert** `InvalidPrice` | fix:none | Names an actual Solidity error / is a structural "Reverts if:"-style NatSpec section header — exempt per the Solidity-error carve-out. |
| core-vaults/architecture/permissions/consensus.mdx | 76 | * Same logic as `checkSignatures`, but reverts with `InvalidSignatures` error if validation fails | fix:none | Names an actual Solidity error / is a structural "Reverts if:"-style NatSpec section header — exempt per the Solidity-error carve-out. |
| core-vaults/architecture/permissions/consensus.mdx | 98 | * Reverts if: | fix:none | Names an actual Solidity error / is a structural "Reverts if:"-style NatSpec section header — exempt per the Solidity-error carve-out. |
| core-vaults/architecture/permissions/consensus.mdx | 111 | * Reverts if signer not found | fix:none | Names an actual Solidity error / is a structural "Reverts if:"-style NatSpec section header — exempt per the Solidity-error carve-out. |
| core-vaults/architecture/permissions/consensus.mdx | 129 | * `InvalidSignatures(bytes32 hash, Signature[] signatures)` (used in revert) | fix:none | Names an actual Solidity error / is a structural "Reverts if:"-style NatSpec section header — exempt per the Solidity-error carve-out. |
| core-vaults/architecture/permissions/protocols/ownedcustomverifier.mdx | 42 | * Reverts with `ZeroValue` if: | fix:none | Names an actual Solidity error / is a structural "Reverts if:"-style NatSpec section header — exempt per the Solidity-error carve-out. |
| core-vaults/architecture/permissions/verifier.mdx | 113 | * Reverts with `VerificationFailed` on failure | fix:none | Names an actual Solidity error / is a structural "Reverts if:"-style NatSpec section header — exempt per the Solidity-error carve-out. |
| core-vaults/architecture/permissions/verifier.mdx | 145 | * Reverts on duplicates (calls already allowed) | fix:none | Names an actual Solidity error / is a structural "Reverts if:"-style NatSpec section header — exempt per the Solidity-error carve-out. |
| core-vaults/architecture/permissions/verifier.mdx | 148 | * Reverts if call is not found in allowlist | fix:none | Names an actual Solidity error / is a structural "Reverts if:"-style NatSpec section header — exempt per the Solidity-error carve-out. |
| core-vaults/architecture/queues/depositqueue.mdx | 44 | * Cancellation reverts if the request has already become claimable (i.e., processed by an oracle report). | fix:none | Names an actual Solidity error / is a structural "Reverts if:"-style NatSpec section header — exempt per the Solidity-error carve-out. |
| core-vaults/architecture/queues/signatureredeemqueue.mdx | 43 | * Reverts with `InsufficientAssets` if funds are lacking | fix:none | Names an actual Solidity error / is a structural "Reverts if:"-style NatSpec section header — exempt per the Solidity-error carve-out. |
| mellow-vaults-overview.mdx | 28 | Every curator action passes through onchain verifiers. Operations outside the mandate revert. | rewrite | Generic prose "revert(s)" with no named error — apply revert→"cannot be executed" rule. Recurring near-verbatim across several current-gen pages ("Operations outside the mandate revert"). |
| resources/mellow-lrt-depreciated/validators/managedvalidator.mdx | 46 | * `requirePermission(address, address, bytes4)`: Verifies that a user has the necessary permissions; reverts with `Forbidden` if not. | fix:none | Names an actual Solidity error / is a structural "Reverts if:"-style NatSpec section header — exempt per the Solidity-error carve-out. |
| resources/mellow-lrt-depreciated/vault.mdx | 29 | Vault also has the functionality of adding and removing `underlyingTokens`, as well as tvlModules. For this purpose, the following functions are avail | fix:none | Legacy page body prose — rule does not apply (mechanics-only on legacy pages, and this describes actual function availability, not error semantics). |
| mellow-alm-toolkit/oracles/velooracle.mdx | 30 | * If any of these deltas is greater in magnitude than `maxAllowedDelta`, the function reverts with the `PriceManipulationDetected` error, indicating a | fix:none | Names an actual Solidity error / is a structural "Reverts if:"-style NatSpec section header — exempt per the Solidity-error carve-out. |
| mellow-alm-toolkit/oracles/velooracle.mdx | 31 | * If there are insufficient observations at any step of the process, the function reverts with the `NotEnoughObservations` error, indicating that the  | fix:none | Names an actual Solidity error / is a structural "Reverts if:"-style NatSpec section header — exempt per the Solidity-error carve-out. |
| mellow-alm-toolkit/strategy/pulsestrategymodule.mdx | 139 | * Validates that `params_` has a length of `0xa0` bytes (160 bytes). If not, it reverts with an `InvalidLength` error, ensuring the data structure is  | fix:none | Names an actual Solidity error / is a structural "Reverts if:"-style NatSpec section header — exempt per the Solidity-error carve-out. |
| mellow-alm-toolkit/strategy/pulsestrategymodule.mdx | 147 | * If any of these checks fail, the function reverts with an `InvalidParams` error. | fix:none | Names an actual Solidity error / is a structural "Reverts if:"-style NatSpec section header — exempt per the Solidity-error carve-out. |
| mellow-alm-toolkit/strategy/pulsestrategymodule.mdx | 152 | * If this condition fails, it reverts with `InvalidParams`. | fix:none | Names an actual Solidity error / is a structural "Reverts if:"-style NatSpec section header — exempt per the Solidity-error carve-out. |
| mellow-alm-toolkit/strategy/pulsestrategymodule.mdx | 160 | * If any condition fails, the function reverts with `InvalidParams`. | fix:none | Names an actual Solidity error / is a structural "Reverts if:"-style NatSpec section header — exempt per the Solidity-error carve-out. |
| mellow-alm/overview/strategies/pulse-strategy-v2.mdx | 37 | \| maxDeviationForVaultPool    \| If the spot tick in UniswapV3Pool deviates from the average tick by a larger value, then rebalance reverts with LIMIT\ | fix:none | Names an actual Solidity error / is a structural "Reverts if:"-style NatSpec section header — exempt per the Solidity-error carve-out. |
| mellow-alm/overview/strategies/pulse-strategy.mdx | 15 | If the spot tick deviates from the weighted average over the last timespanForAverageTick seconds, then the transaction is reverted. Otherwise, the cur | fix:none | Names an actual Solidity error / is a structural "Reverts if:"-style NatSpec section header — exempt per the Solidity-error carve-out. |
| mellow-alm/overview/strategies/pulse-strategy.mdx | 43 | \| maxDeviationForVaultPool    \| If the spot tick in UniswapV3Pool deviates from the average tick by a larger value, then rebalance reverts with LIMIT\ | fix:none | Names an actual Solidity error / is a structural "Reverts if:"-style NatSpec section header — exempt per the Solidity-error carve-out. |
| mellow-alm/overview/strategies/uni-v3-boosted-strategy.mdx | 131 | \| maxTickDeviation     \| If the spot tick in UniswapV3Pool deviates from the average tick by a larger value, then rebalance reverts with LIMIT\_OVERFL | fix:none | Names an actual Solidity error / is a structural "Reverts if:"-style NatSpec section header — exempt per the Solidity-error carve-out. |

**Aggregated (giant auto-generated alm-legacy files):**

| File | Hits | Line range | Pattern |
|---|---|---|---|
| mellow-alm/overview/api.mdx | 200 | 106–11049 | Test-spec bullets of the form "✅ ... reverts with ERRORCODE" — structurally the same as the exempt Solidity-error pattern seen elsewhere in this category; not individually reviewed line-by-line but sampled and consistent with that pattern |
| mellow-alm/overview/contracts-specs.mdx | 180 | 31–2340 | Same pattern |

### 2.8 ERC-4626
`ERC.?4626` (case-insensitive). Core Vaults must never be described as ERC-4626 compatible; legacy vaults (MultiVault, Simple LRT) genuinely are ERC-4626 extensions, which is correct as long as it's scoped to the legacy product by name.

Total hits: 17 — current: 10, lrt-legacy: 7.

| Path | Line | Excerpt | Class | Note |
|---|---|---|---|---|
| core-vaults-and-other-approaches.mdx | 3 | description: "How Core Vaults compare to ERC-4626, ERC-7540, and common vault architectural patterns – evaluated against institutional requirements." | flag | Contrastive/comparative use ("Core Vaults vs ERC-4626") — appears correctly scoped already (comparing to, not claiming to be). Confirm no rewrite needed. |
| core-vaults-and-other-approaches.mdx | 10 | ERC-4626 and ERC-7540 are not competitors to Core Vaults – they define vault interfaces. Core Vaults operate at the management layer: strategy executi | flag | Contrastive/comparative use ("Core Vaults vs ERC-4626") — appears correctly scoped already (comparing to, not claiming to be). Confirm no rewrite needed. |
| core-vaults-and-other-approaches.mdx | 38 | **Interface standards: ERC-4626 and ERC-7540.** ERC-4626 standardized synchronous deposit-and-share operations. ERC-7540 added async requests. Both ad | flag | Contrastive/comparative use ("Core Vaults vs ERC-4626") — appears correctly scoped already (comparing to, not claiming to be). Confirm no rewrite needed. |
| core-vaults-and-other-approaches.mdx | 75 | \| Capability                  \| Core Vaults                    \| ERC-4626 \| ERC-7540                 \| Adapter-based          \| MPC-based  \| Single-pr | flag | Contrastive/comparative use ("Core Vaults vs ERC-4626") — appears correctly scoped already (comparing to, not claiming to be). Confirm no rewrite needed. |
| core-vaults-for-rwa-allocation.mdx | 49 | RWA allocation uses the full Core Vault feature set – multi-asset accounting, asynchronous liquidity, asset eligibility, and oracle-defined NAV – rath | flag | Contextual mention, appears already correctly scoped as contrast — confirm. |
| core-vaults/architecture/managers/basicsharemanager.mdx | 69 | * This implementation is ideal when the vault owner requires non-transferable shares for internal logic, without compliance to ERC20 or ERC4626 standa | flag | Describes a specific ShareManager mode explicitly built WITHOUT ERC20/ERC4626 compliance — reads as correctly scoped (states the vault does NOT follow ERC4626 here). Confirm. |
| core-vaults/architecture/managers/tokenizedsharemanager.mdx | 11 | * Suitable for use cases where share liquidity, composability, or token standard compatibility (e.g., ERC20, ERC4626 wrappers) is required. | flag | Describes ERC4626-style wrapper compatibility as one supported use case for TokenizedShareManager — borderline, confirm this doesn't read as "Core Vaults are ERC-4626." |
| mellow-vaults-overview.mdx | 44 | ## Beyond ERC-4626 | fix:none | Correctly and explicitly scoped ("Beyond ERC-4626" — contrastive), matches the required framing already. |
| mellow-vaults-overview.mdx | 46 | Core Vaults are architecturally beyond ERC-4626. | fix:none | Correctly and explicitly scoped ("Beyond ERC-4626" — contrastive), matches the required framing already. |
| mellow-vaults-overview.mdx | 48 | ERC-4626 standardizes the interface for single-asset, single-strategy vaults with synchronous liquidity. It does not define multi-asset accounting, as | fix:none | Correctly and explicitly scoped ("Beyond ERC-4626" — contrastive), matches the required framing already. |
| restaking-vaults/multivault/architecture.mdx | 9 | The **MultiVault** is designed to address the challenges of aggregating and managing various forms of restaking and yield sources. It builds upon the  | fix:none | Legacy vault genuinely extends ERC-4626 — correct as scoped to the named legacy product. |
| restaking-vaults/multivault/architecture.mdx | 24 | These adapters unify the interaction with various restaking protocols, abstracting away protocol-specific complexities. Additionally, a **generic ERC4 | fix:none | Legacy vault genuinely extends ERC-4626 — correct as scoped to the named legacy product. |
| restaking-vaults/multivault/architecture.mdx | 45 | 3. **ERC4626** | fix:none | Legacy vault genuinely extends ERC-4626 — correct as scoped to the named legacy product. |
| restaking-vaults/multivault/index.mdx | 3 | description: "Aggregates multiple isolated subvaults into a single user-facing vault with ERC4626 extension, configurable limits, whitelists, and lock | fix:none | Legacy vault genuinely extends ERC-4626 — correct as scoped to the named legacy product. |
| restaking-vaults/simple-lrt/architecture.mdx | 15 | 1. Deposit and withdrawal are ERC4626 style, withdrawals are async though. | fix:none | Legacy vault genuinely extends ERC-4626 — correct as scoped to the named legacy product. |
| restaking-vaults/simple-lrt/index.mdx | 3 | description: "ERC4626-modified vault with queued withdrawals via SymbioticWithdrawalQueue. Built around MellowSymbioticVault or MellowVaultCompat." | fix:none | Legacy vault genuinely extends ERC-4626 — correct as scoped to the named legacy product. |
| restaking-vaults/simple-lrt/overview.mdx | 5 | The system is comprised of a set of contracts, with the main one being the vault contract (MellowSymbioticVault / MellowVaultCompat). This vault is a  | fix:none | Legacy vault genuinely extends ERC-4626 — correct as scoped to the named legacy product. |

### 2.9 Copper
`Copper(?! ClearLoop)` (case-insensitive). CeFi venues must be named as the pair "Copper ClearLoop and Ceffu".

Total hits: 5 (all on `core-vaults/architecture/index.mdx`, current-gen).

| Path | Line | Excerpt | Class | Note |
|---|---|---|---|---|
| core-vaults/architecture/index.mdx | 390 | #### **CEXes** via custodial off-exchange solutions (Copper and Ceffu) | rewrite | Bare "Copper" not paired with "Copper ClearLoop and Ceffu" per the naming rule. Lines 444/448 are inside a code sample (variable/comment naming) — mechanical rename not needed there, only the prose at 390/432/438. |
| core-vaults/architecture/index.mdx | 432 | * First one allowing liquidity transfer into a Copper or Ceffu account | rewrite | Bare "Copper" not paired with "Copper ClearLoop and Ceffu" per the naming rule. Lines 444/448 are inside a code sample (variable/comment naming) — mechanical rename not needed there, only the prose at 390/432/438. |
| core-vaults/architecture/index.mdx | 438 | * In UI, Curator clicks **New Call** → sets target to **Copper Subvault** → selects **USDC** asset and B**ybit Clearloop** as destination. | rewrite | Bare "Copper" not paired with "Copper ClearLoop and Ceffu" per the naming rule. Lines 444/448 are inside a code sample (variable/comment naming) — mechanical rename not needed there, only the prose at 390/432/438. |
| core-vaults/architecture/index.mdx | 444 | asset,                          // address of the asset (USDC) to be sent to the Copper account | rewrite | Bare "Copper" not paired with "Copper ClearLoop and Ceffu" per the naming rule. Lines 444/448 are inside a code sample (variable/comment naming) — mechanical rename not needed there, only the prose at 390/432/438. |
| core-vaults/architecture/index.mdx | 448 | (copperAccountAddress, 5e11)  // (recipient, amount) | rewrite | Bare "Copper" not paired with "Copper ClearLoop and Ceffu" per the naming rule. Lines 444/448 are inside a code sample (variable/comment naming) — mechanical rename not needed there, only the prose at 390/432/438. |

### 2.10 "asset management" / "asset manager" (flag only)
`\b(asset management|asset manager)\b` (case-insensitive). Flag only per instructions — do not fix. Cross-ref Needs-human-decision (Ilya): this phrase (and "onchain asset management" specifically) is currently used in the site's primary tagline.

Total hits: 11 — current: 10, lrt-legacy: 1.

| Path | Line | Excerpt | Class | Note |
|---|---|---|---|---|
| core-vaults-for-stablecoins.mdx | 110 | As autonomous agents take on more asset management responsibilities, the infrastructure they operate in matters as much as the models behind them. Aut | flag | Ilya — editorial/positioning review (see Needs-human-decision). |
| core-vaults-integration-guide.mdx | 10 | A **Mellow Core Vault** is a programmable, modular asset management contract. It serves as the central hub for capital management, risk control, and c | flag | Ilya — editorial/positioning review (see Needs-human-decision). |
| core-vaults/architecture/oracle.mdx | 16 | * **Asset Management**: Controls which tokens are supported by the oracle | flag | Ilya — editorial/positioning review (see Needs-human-decision). |
| core-vaults/architecture/vaults/subvault.mdx | 7 | The `Subvault` contract represents a modular, permissioned vault component designed to manage delegated asset strategies within a parent `Vault`. It e | flag | Ilya — editorial/positioning review (see Needs-human-decision). |
| index.mdx | 3 | description: "Mellow is vault infrastructure for onchain asset management. Fintechs, exchanges, and asset issuers launch Earn, treasury, and managed y | flag | Ilya — editorial/positioning review (see Needs-human-decision). |
| mellow-vaults-overview.mdx | 8 | Mellow is vault infrastructure for onchain asset management. | flag | Ilya — editorial/positioning review (see Needs-human-decision). |
| mellow-vaults-overview.mdx | 78 | ### Agentic asset management | flag | Ilya — editorial/positioning review (see Needs-human-decision). |
| strategy-vault/overview.mdx | 6 | The stRATEGY Vault amplifies staking rewards by allocating deposited tokens across highly liquid and diverse DeFi opportunities, while strengthening t | flag | Ilya — editorial/positioning review (see Needs-human-decision). |
| vault-infrastructure-for-fintech-earn-products.mdx | 88 | Vaults where the operator is an autonomous agent. Core Vaults act as the safety harness and execution environment for agentic asset management – the a | flag | Ilya — editorial/positioning review (see Needs-human-decision). |
| vault-infrastructure-for-fintech-earn-products.mdx | 98 | The **distribution partner** – wallet, exchange, fintech, bank, or broker – owns the customer and the brand. The **vault manager** – an institutional  | flag | Ilya — editorial/positioning review (see Needs-human-decision). |
| restaking-vaults/multivault/architecture.mdx | 49 | #### Asset Management Strategy | flag | Ilya — editorial/positioning review (see Needs-human-decision). |

### 2.11 "compliant"/"compliance"/"KYC"/"AML" (flag only)
`\b(compliant|compliance|KYC|AML)\b` (case-insensitive). Flag only per instructions. Cross-ref Needs-human-decision (Legal).

Total hits: 43 — current: 40, lrt-legacy: 3.

| Path | Line | Excerpt | Class | Note |
|---|---|---|---|---|
| core-vaults-and-other-approaches.mdx | 10 | ERC-4626 and ERC-7540 are not competitors to Core Vaults – they define vault interfaces. Core Vaults operate at the management layer: strategy executi | flag | Legal review (see Needs-human-decision). |
| core-vaults-and-other-approaches.mdx | 16 | Basic vault interfaces cover deposits, share issuance, and redemptions. Institutional vault infrastructure requires additional controls for settlement | flag | Legal review (see Needs-human-decision). |
| core-vaults-and-other-approaches.mdx | 30 | **Compliance at the vault level.** Jurisdiction-aware mandates, allowlisted depositors, transfer-restricted shares, and per-issuer eligibility constra | flag | Legal review (see Needs-human-decision). |
| core-vaults-and-other-approaches.mdx | 38 | **Interface standards: ERC-4626 and ERC-7540.** ERC-4626 standardized synchronous deposit-and-share operations. ERC-7540 added async requests. Both ad | flag | Legal review (see Needs-human-decision). |
| core-vaults-and-other-approaches.mdx | 58 | \| Vault-level compliance       \| External or at distribution endpoint            \| Jurisdiction-aware mandates, allowlisted depositors, transfer-restr | flag | Legal review (see Needs-human-decision). |
| core-vaults-and-other-approaches.mdx | 87 | \| Vault-level compliance      \| Vault-level                    \| No       \| No                       \| Varies                 \| Varies     \| Varies    | flag | Legal review (see Needs-human-decision). |
| core-vaults-and-other-approaches.mdx | 96 | Interface standards and single-purpose architectures each solve a slice of the problem: a synchronous interface, an async primitive, a fixed set of pr | flag | Legal review (see Needs-human-decision). |
| core-vaults-and-other-approaches.mdx | 98 | Core Vaults are built for the full surface rather than a slice. Multi-protocol allocation, mixed DeFi and CeFi execution, operation-level risk control | flag | Legal review (see Needs-human-decision). |
| core-vaults-and-other-approaches.mdx | 112 | * **Compliance:** Jurisdiction-aware mandates, allowlisted depositors, transfer-restricted shares | flag | Legal review (see Needs-human-decision). |
| core-vaults-for-rwa-allocation.mdx | 10 | Tokenized real-world assets (RWA) are one of the primary asset types for vault-based allocation products built on Mellow. Asset managers, funds, treas | flag | Legal review (see Needs-human-decision). |
| core-vaults-for-rwa-allocation.mdx | 16 | Building an RWA allocation product from scratch requires smart contracts, security audits, integrations with regulated issuers, risk monitoring, and c | flag | Legal review (see Needs-human-decision). |
| core-vaults-for-rwa-allocation.mdx | 77 | **Compliance** | flag | Legal review (see Needs-human-decision). |
| core-vaults-for-rwa-allocation.mdx | 81 | Compliance rules are applied at the vault level, so different products can enforce different eligibility and transfer requirements without changing th | flag | Legal review (see Needs-human-decision). |
| core-vaults-for-rwa-allocation.mdx | 83 | * Share-token eligibility – configurable hooks for KYC and KYB providers, transfer agents, accreditation registries, and jurisdiction-based restrictio | flag | Legal review (see Needs-human-decision). |
| core-vaults-for-rwa-allocation.mdx | 88 | Different products on the same platform enforce different compliance perimeters without separate codebases. | flag | Legal review (see Needs-human-decision). |
| core-vaults-for-stablecoins.mdx | 8 | Stablecoins are one of the primary asset types for vault-based yield and treasury products on Mellow. Companies holding stablecoin balances – payment  | flag | Legal review (see Needs-human-decision). |
| core-vaults-for-stablecoins.mdx | 14 | Building a stablecoin vault product from scratch requires smart contracts, security audits, DeFi integrations, risk monitoring, and compliance review  | flag | Legal review (see Needs-human-decision). |
| core-vaults-for-stablecoins.mdx | 38 | In the stablecoin context, an Earn product is a vault with a yield-focused mandate: approved strategies, risk limits, compliance constraints, and depo | flag | Legal review (see Needs-human-decision). |
| core-vaults-for-stablecoins.mdx | 53 | Treasury management applies to any holder of stablecoin balances – whether a company managing its balance sheet or an individual managing personal hol | flag | Legal review (see Needs-human-decision). |
| core-vaults-for-stablecoins.mdx | 57 | Companies using stablecoin settlement – payment processors, marketplaces, fintechs, trading platforms – accumulate balances that need active managemen | flag | Legal review (see Needs-human-decision). |
| core-vaults-for-stablecoins.mdx | 98 | #### **Compliance** | flag | Legal review (see Needs-human-decision). |
| core-vaults-for-stablecoins.mdx | 100 | Compliance rules are applied at the vault level, so different products can enforce different eligibility and transfer requirements without changing th | flag | Legal review (see Needs-human-decision). |
| core-vaults-for-stablecoins.mdx | 102 | ​Share-token eligibility – configurable hooks for KYC/KYB providers, transfer agents, accreditation registries, jurisdiction-based restrictions | flag | Legal review (see Needs-human-decision). |
| core-vaults-for-stablecoins.mdx | 106 | Different products on the same platform enforce different compliance perimeters without separate codebases. | flag | Legal review (see Needs-human-decision). |
| core-vaults-integration-guide.mdx | 21 | \| **ShareManager** \| ERC20-compatible contract managing share supply, whitelisting, global lockups, and compliance controls.                           | flag | Legal review (see Needs-human-decision). |
| core-vaults/architecture/factory.mdx | 9 | It conforms to the `IFactory` interface and supports deploying any `IFactoryEntity`-compliant contracts via `TransparentUpgradeableProxy`, using `init | flag | Legal review (see Needs-human-decision). |
| core-vaults/architecture/index.mdx | 12 | Meanwhile, the latest wave of adoption is bringing thousands of new participants from traditional markets into crypto. These users aren’t looking for  | flag | Legal review (see Needs-human-decision). |
| core-vaults/architecture/index.mdx | 280 | * Whitelisting for implementing KYC & compliance features | flag | Legal review (see Needs-human-decision). |
| core-vaults/architecture/managers/basicsharemanager.mdx | 69 | * This implementation is ideal when the vault owner requires non-transferable shares for internal logic, without compliance to ERC20 or ERC4626 standa | flag | Legal review (see Needs-human-decision). |
| core-vaults/architecture/managers/tokenizedsharemanager.mdx | 7 | * This module extends `ShareManager` and `ERC20Upgradeable`, making vault shares externally transferable and fully compliant with the ERC20 standard. | flag | Legal review (see Needs-human-decision). |
| core-vaults/architecture/modules/basemodule.mdx | 7 | `BaseModule` is an abstract contract that acts as a foundational layer for modules within the system. It integrates shared logic such as initializer p | flag | Legal review (see Needs-human-decision). |
| core-vaults/architecture/modules/basemodule.mdx | 49 | * `IERC721Receiver.onERC721Received.selector` — confirms compliance. | flag | Legal review (see Needs-human-decision). |
| core-vaults/architecture/queues/signaturedepositqueue.mdx | 7 | `SignatureDepositQueue` extends `SignatureQueue` to enable **instant deposit** of assets into a vault, bypassing the standard on-chain `DepositQueue`  | flag | Legal review (see Needs-human-decision). |
| mellow-vaults-overview.mdx | 62 | Companies can manage stablecoin balances and other onchain assets under defined liquidity, allocation, venue, and compliance constraints. | flag | Legal review (see Needs-human-decision). |
| vault-infrastructure-for-fintech-earn-products.mdx | 3 | description: "Reference page for teams evaluating vault infrastructure to power Earn, treasury, and managed yield products. Covers Mellow's architectu | flag | Legal review (see Needs-human-decision). |
| vault-infrastructure-for-fintech-earn-products.mdx | 10 | The partner owns the customer and the brand. Vault manager provides the strategy. Mellow provides the vault layer – smart contracts, risk enforcement, | flag | Legal review (see Needs-human-decision). |
| vault-infrastructure-for-fintech-earn-products.mdx | 46 | #### Compliance architecture | flag | Legal review (see Needs-human-decision). |
| vault-infrastructure-for-fintech-earn-products.mdx | 52 | Share-token eligibility supports configurable hooks for KYC/KYB providers, transfer agents, and accreditation registries. | flag | Legal review (see Needs-human-decision). |
| vault-infrastructure-for-fintech-earn-products.mdx | 56 | Different products on the same platform enforce different compliance perimeters without separate codebases. | flag | Legal review (see Needs-human-decision). |
| vault-infrastructure-for-fintech-earn-products.mdx | 98 | The **distribution partner** – wallet, exchange, fintech, bank, or broker – owns the customer and the brand. The **vault manager** – an institutional  | flag | Legal review (see Needs-human-decision). |
| resources/mellow-lrt-depreciated/validators/defaultbondvalidator.mdx | 12 | * **Validation**: Enforces compliance for bond operations, ensuring only supported bonds are accessed. | flag | Legal review (see Needs-human-decision). |
| resources/mellow-lrt-depreciated/validators/erc20swapvalidator.mdx | 12 | * **Swap Validation**: Enforces rules for swap operations, ensuring compliance with authorized routers and tokens. | flag | Legal review (see Needs-human-decision). |
| restaking-vaults/multivault/architecture.mdx | 192 | * The **EigenLayerAdapter** interacts with these isolated vaults instead of directly managing EigenLayer strategies. This abstraction simplifies Multi | flag | Legal review (see Needs-human-decision). |

### 2.12 Promotional / AI-voice vocabulary
`\b(innovative|seamless|seamlessly|thoroughly|robust|cutting-edge|best-in-class|unlock(s|ing)?|leverage the power|comprehensive|significantly enhance)\b` (case-insensitive).

Total hits: 15 — current: 2, alm-legacy: 8, lrt-legacy: 5.

| Path | Line | Excerpt | Class | Note |
|---|---|---|---|---|
| core-vaults/architecture/modules/vaultmodule.mdx | 7 | `VaultModule` is a core component of the modular vault architecture. It manages liquidity routing between the [`Vault`](https://www.notion.so/Vault-23 | rewrite | Current-gen page. |
| security.mdx | 8 | Mellow LRT smart contracts has been thoroughly audited by multiple firms specializing in smart contract audit and Sherlock community contributors. | rewrite | "thoroughly audited" on the high-traffic current-gen /security page — also a legacy-vocab hit on the same line. |
| resources/mellow-lrt-depreciated/strategies/simpledvtstakingstrategy.mdx | 7 | The `SimpleDVTStakingStrategy` contract manages staking operations by interacting with a staking module and vault. It extends the `DefaultAccessContro | flag | Legacy-generation page — promotional-vocab mechanic is ambiguous under Section 2's rules (not explicitly listed as an all-pages mechanic); flagged informational, not actioned. Note: several of these are literal function-name matches ("revokeDepositsLock" -> "Unlocks", "unlock the original L2 liquidity") rather than marketing language. |
| resources/mellow-lrt-depreciated/validators/defaultbondvalidator.mdx | 7 | `DefaultBondValidator` ensures that only authorized bonds are involved in deposit and withdrawal operations. It uses validation methods, mappings for  | flag | Legacy-generation page — promotional-vocab mechanic is ambiguous under Section 2's rules (not explicitly listed as an all-pages mechanic); flagged informational, not actioned. Note: several of these are literal function-name matches ("revokeDepositsLock" -> "Unlocks", "unlock the original L2 liquidity") rather than marketing language. |
| resources/mellow-lrt-depreciated/vaultconfigurator.mdx | 89 | * `revokeDepositsLock()`: Unlocks deposits. | flag | Legacy-generation page — promotional-vocab mechanic is ambiguous under Section 2's rules (not explicitly listed as an all-pages mechanic); flagged informational, not actioned. Note: several of these are literal function-name matches ("revokeDepositsLock" -> "Unlocks", "unlock the original L2 liquidity") rather than marketing language. |
| restaking-vaults/interoperable-vaults/architecture.mdx | 16 | If a slashing event occurs, the OFT itself can be slashed. Any non-burned OFT can still be redeemed to unlock the original L2 liquidity. | flag | Legacy-generation page — promotional-vocab mechanic is ambiguous under Section 2's rules (not explicitly listed as an all-pages mechanic); flagged informational, not actioned. Note: several of these are literal function-name matches ("revokeDepositsLock" -> "Unlocks", "unlock the original L2 liquidity") rather than marketing language. |
| restaking-vaults/interoperable-vaults/overview.mdx | 5 | Interoperable Vaults facilitate cross‐chain restaking, allowing users to deposit assets on EVM networks and receive vault shares, which are then resta | flag | Legacy-generation page — promotional-vocab mechanic is ambiguous under Section 2's rules (not explicitly listed as an all-pages mechanic); flagged informational, not actioned. Note: several of these are literal function-name matches ("revokeDepositsLock" -> "Unlocks", "unlock the original L2 liquidity") rather than marketing language. |
| mellow-alm-toolkit/components.mdx | 11 | 1. **Core Module**: This is the heart of the MALM system, acting as the central hub that seamlessly integrates all other modules. It ensures cohesive  | flag | Legacy-generation page — promotional-vocab mechanic is ambiguous under Section 2's rules (not explicitly listed as an all-pages mechanic); flagged informational, not actioned. Note: several of these are literal function-name matches ("revokeDepositsLock" -> "Unlocks", "unlock the original L2 liquidity") rather than marketing language. |
| mellow-alm-toolkit/core.mdx | 5 | The `Core` contract is a central component, orchestrating interactions between users, liquidity pools, and various strategy modules within Automated M | flag | Legacy-generation page — promotional-vocab mechanic is ambiguous under Section 2's rules (not explicitly listed as an all-pages mechanic); flagged informational, not actioned. Note: several of these are literal function-name matches ("revokeDepositsLock" -> "Unlocks", "unlock the original L2 liquidity") rather than marketing language. |
| mellow-alm-toolkit/oracles/velooracle.mdx | 5 | `VeloOracle` extends the generic oracle functionalities to cater specifically to the Velo protocol, incorporating advanced mechanisms for detecting an | flag | Legacy-generation page — promotional-vocab mechanic is ambiguous under Section 2's rules (not explicitly listed as an all-pages mechanic); flagged informational, not actioned. Note: several of these are literal function-name matches ("revokeDepositsLock" -> "Unlocks", "unlock the original L2 liquidity") rather than marketing language. |
| mellow-alm-toolkit/oracles/velooracle.mdx | 12 | * `NotEnoughObservations()`: Occurs when there is insufficient historical data to conduct a reliable analysis, underscoring the need for a comprehensi | flag | Legacy-generation page — promotional-vocab mechanic is ambiguous under Section 2's rules (not explicitly listed as an all-pages mechanic); flagged informational, not actioned. Note: several of these are literal function-name matches ("revokeDepositsLock" -> "Unlocks", "unlock the original L2 liquidity") rather than marketing language. |
| mellow-alm-toolkit/utility-contracts/velodeployfactory.mdx | 27 | Encapsulates both immutable and mutable parameters, providing a comprehensive view of the factory's configuration and enabling cohesive management of  | flag | Legacy-generation page — promotional-vocab mechanic is ambiguous under Section 2's rules (not explicitly listed as an all-pages mechanic); flagged informational, not actioned. Note: several of these are literal function-name matches ("revokeDepositsLock" -> "Unlocks", "unlock the original L2 liquidity") rather than marketing language. |
| mellow-alm-toolkit/utility-contracts/velodeployfactory.mdx | 70 | * **Purpose**: Facilitates the creation of new strategies for given token pairs and tick spacings, encapsulating the strategic logic in deployable ent | flag | Legacy-generation page — promotional-vocab mechanic is ambiguous under Section 2's rules (not explicitly listed as an all-pages mechanic); flagged informational, not actioned. Note: several of these are literal function-name matches ("revokeDepositsLock" -> "Unlocks", "unlock the original L2 liquidity") rather than marketing language. |
| mellow-alm-toolkit/utility-contracts/velodeployfactory.mdx | 81 | * **Impact**: Supports informed decision-making and governance within the Velo ecosystem by providing stakeholders with comprehensive insights into th | flag | Legacy-generation page — promotional-vocab mechanic is ambiguous under Section 2's rules (not explicitly listed as an all-pages mechanic); flagged informational, not actioned. Note: several of these are literal function-name matches ("revokeDepositsLock" -> "Unlocks", "unlock the original L2 liquidity") rather than marketing language. |
| mellow-alm/mellow-alm-toolkit/index.mdx | 6 | The Mellow Automatic Liquidity Management (MALM) toolkit is an a comprehensive suite of tools designed specifically for the effective management of co | flag | Legacy-generation page — promotional-vocab mechanic is ambiguous under Section 2's rules (not explicitly listed as an all-pages mechanic); flagged informational, not actioned. Note: several of these are literal function-name matches ("revokeDepositsLock" -> "Unlocks", "unlock the original L2 liquidity") rather than marketing language. |

### 2.13 Stale time-bound copy
`(coming soon|will be (launched|available)|first [0-9]+ weeks|starting (January|...|December) [0-9]{1,2}, 20(24|25))` (case-insensitive).

Total hits: 2.

| Path | Line | Excerpt | Class | Note |
|---|---|---|---|---|
| strategy-vault/rewards.mdx | 33 | The stRATEGY Vault offers boosted Mellow Points for the first 4 weeks after launch (starting October 01, 2025): | rewrite | Current-gen page: "boosted Mellow Points for the first 4 weeks after launch (starting October 01, 2025)" — that window has already elapsed relative to today (2026-09-16). Stale, needs update/removal. |
| mellow-alm/overview/strategies/gearbox-strategy.mdx | 55 | Our external bot will be launched and it will be responsible for maintaining two main goals: | flag | Legacy page, historical description ("Our external bot will be launched...") — likely fine to leave as a historical record; flagged for archive-vs-delete judgment rather than rewrite. |

### 2.14 Broken markup
Empty headings `^#+\s*$`, and text wrapped in `<br>` instead of a heading `<br\s*/?>[A-Za-z]`. Mechanic — applies to all pages, classified `fix`.

Empty headings: 2. `<br>`-as-heading: 4.

| Path | Line | Excerpt | Class | Note |
|---|---|---|---|---|
| core-vaults/architecture/index.mdx | 47 | #### | fix |  |
| mellow-alm/overview/index.mdx | 40 | ### | fix |  |
| core-vaults/core-deployments.mdx | 434 | <br />Instances | fix |  |
| security.mdx | 110 | <br />DVV<br /> | fix |  |
| mellow-alm/overview/governance-parameters.mdx | 9 | <table><thead><tr><th width="209">Parameter</th><th width="335">Description</th><th width="150">Unit</th><th>Default value</th></tr></thead><tbody><tr | fix |  |
| mellow-alm/overview/governance-parameters.mdx | 9 | <table><thead><tr><th width="209">Parameter</th><th width="335">Description</th><th width="150">Unit</th><th>Default value</th></tr></thead><tbody><tr | fix |  |

### 2.15 Naming drift — "Mellow Protocol" occurrences
Total: 5 — all on alm-legacy pages.

| Path | Line | Excerpt | Class | Note |
|---|---|---|---|---|
| mellow-alm/overview/architecture.mdx | 74 | There are several levels of management in the Mellow Protocol: | flag | Naming-drift count — "Mellow Protocol" is the legacy ALM-era brand name; all 5 occurrences are on alm-legacy pages (title/prose), which is internally consistent for that generation. Flagged for Ilya's awareness only, no action needed on legacy pages. |
| mellow-alm/overview/definitions.mdx | 81 | Governance is a Mellow Protocol DAO multi-signature wallet that can alter parameters common for all [Vaults](definitions#vault). | flag | Naming-drift count — "Mellow Protocol" is the legacy ALM-era brand name; all 5 occurrences are on alm-legacy pages (title/prose), which is internally consistent for that generation. Flagged for Ilya's awareness only, no action needed on legacy pages. |
| mellow-alm/overview/mellow-contracts-addresses/mellow-protocol-addresses-mainnet.mdx | 2 | title: "Mellow Protocol Addresses (Mainnet)" | flag | Naming-drift count — "Mellow Protocol" is the legacy ALM-era brand name; all 5 occurrences are on alm-legacy pages (title/prose), which is internally consistent for that generation. Flagged for Ilya's awareness only, no action needed on legacy pages. |
| mellow-alm/overview/mellow-contracts-addresses/mellow-protocol-addresses-polygon.mdx | 2 | title: "Mellow Protocol Addresses (Polygon)" | flag | Naming-drift count — "Mellow Protocol" is the legacy ALM-era brand name; all 5 occurrences are on alm-legacy pages (title/prose), which is internally consistent for that generation. Flagged for Ilya's awareness only, no action needed on legacy pages. |
| mellow-alm/overview/strategies/gearbox-strategy.mdx | 23 | * This procedure is subject to some losses for long-time users because every 2 weeks all funds go through adding and removing liquidity in the Curve p | flag | Naming-drift count — "Mellow Protocol" is the legacy ALM-era brand name; all 5 occurrences are on alm-legacy pages (title/prose), which is internally consistent for that generation. Flagged for Ilya's awareness only, no action needed on legacy pages. |

### 2.16 Legacy/first-person patterns found only inside code (contract identifiers, variable names)
Per instructions: legacy-vocab / first-person / promotional-vocab / claim-risk matches that occur purely inside a fenced code block or inline code span naming a real identifier are excluded from the categories above and listed here instead.

| Category | Path | Line | Excerpt | Context |
|---|---|---|---|---|
| first-person | core-vaults/architecture/hooks/basicredeemhook.mdx | 28 | * Iterates over subvaults (via `subvaultAt(i)`) | inline-code-span |
| first-person | core-vaults/architecture/hooks/basicredeemhook.mdx | 45 | * `subvault[i].balanceOf(asset)` for each subvault | inline-code-span |
| legacy-vocab | core-vaults/architecture/index.mdx | 463 | symbioticVault,            // address of the symbiotic vault | fenced-code-block |
| first-person | core-vaults/architecture/permissions/bitmaskverifier.mdx | 60 | 4. Each `data[i]` masked by `bitmask[96+i]` | inline-code-span |
| first-person | core-vaults/architecture/permissions/protocols/ownedcustomverifier.mdx | 41 | * Grants each `roles[i]` to `holders[i]` | inline-code-span |
| first-person | mellow-alm-toolkit/oracles/index.mdx | 69 | a\_t=\sum\_{i=0}^{t}\log\_{1.0001}p\_i, | fenced-code-block |
| first-person | mellow-alm-toolkit/oracles/index.mdx | 72 | where `$p\_i$` is a price at the end of block `$i$`, limited to blocks that recorded at least one swap transaction. For each block with timestamp `$t\ | inline-code-span |
| first-person | mellow-alm-toolkit/oracles/index.mdx | 81 | \overline{p}_i=1.0001^{(a_{t\_{i\}}-{a\_{t\_{i+1\}}})/(t\_{i}-t\_{i+1})}, i \in\[0, k-1) | fenced-code-block |
| first-person | mellow-alm-toolkit/oracles/index.mdx | 87 | d\_i=\|\overline{p}_i/\overline{p}_{i+1}-1\|, i \in \[0, k-2) | fenced-code-block |
| first-person | mellow-alm/overview/api.mdx | 197 | function validatorsAddress(uint256 i) external returns (address) | fenced-code-block |
| first-person | mellow-alm/overview/api.mdx | 206 | \| `i`  \| uint256 \| The number of address \| | inline-code-span |
| legacy-vocab | points/defi-points-integration-instructions.mdx | 275 | * To fallback points if the contract owns LRTs but there's no shareholder | fenced-code-block |
| legacy-vocab | points/defi-points-integration-instructions.mdx | 277 | * ex: LRTs deposited in SY but SY not used in YT / LP | fenced-code-block |
| legacy-vocab | points/defi-points-integration-instructions.mdx | 286 | * Get LRT holders e.g. UniswapV3 Pool, Pendle SY | fenced-code-block |
| first-person | resources/mellow-lrt-depreciated/emergency-withdrawal-guide-advanced.mdx | 20 | 3. Along with your requested lp amount (can be obtained from withdrawalRequest) copy an array from baseTvl() and for each value from it make the follo | inline-code-span |


## 3. Summary count table

Counts reflect the classification applied above (fix = mechanical, no judgment; rewrite = clearly in-scope but needs new copy; flag = needs a human decision, or is a flag-only category per instructions). Legacy-generation editorial-only hits that are informational-only (rule doesn't apply there) are counted under "n/a (legacy, informational)".

| Category | Total hits | fix | rewrite | flag | n/a (legacy, informational) |
|---|---|---|---|---|---|
| Legacy vocabulary | 425 | 0 | 10 | 34 | 381 |
| First person | 149 | 0 | 2 | 0 | 147 |
| Em dash | 92 | 92 | 0 | 0 | 0 |
| on-chain | 15 | 15 | 0 | 0 | 0 |
| Curly quotes | 61 | 61 | 0 | 0 | 0 |
| Emoji | 1734 | 1734 | 0 | 0 | 0 |
| revert(s|ed)? | 430 | 0 | ~8 confirmed + more unreviewed in the giant files | ~42 | 0 |
| ERC-4626 | 17 | 0 | 0 | 10 | 7 |
| Copper | 5 | 0 | 3 | 0 | 0 |
| asset management / asset manager (flag only) | 11 | 0 | 0 | 11 | 0 |
| compliant/compliance/KYC/AML (flag only) | 43 | 0 | 0 | 43 | 0 |
| Promotional/AI-voice vocabulary | 15 | 0 | 2 | 13 | 0 |
| Stale time-bound copy | 2 | 0 | 1 | 1 | 0 |
| Broken markup (empty heading + <br> heading) | 6 | 6 | 0 | 0 | 0 |
| Naming drift ("Mellow Protocol") | 5 | 0 | 0 | 5 | 0 |
| Identifiers found only in code (2.16) | 15 | 0 | 0 | 0 | 15 |

Note on the `revert` row: 42 current/shared-technical + legacy-non-giant hits were individually reviewed and classified (34 exempt "fix:none" because they name an actual Solidity error, 8 genuine prose "rewrite" candidates, 1 "flag" for the USDT third-party-behavior line). The 380 hits inside the two giant alm-legacy API/spec files were sampled (confirmed to follow the same "X reverts with ERRORCODE" exempt pattern) but not individually reviewed line-by-line — see Section 2.7.

## 4. Needs human decision

Reproduced from the task's Section 6/3 spec. Not resolved here — audit-only.

### 4.1 Andrey (technical/factual)

- **Which audit reports belong to which architecture.** `security.mdx` (current-gen, high-traffic) is actually already structured with `#### MultiVault`, `#### Interoperable Vault`, and `#### Core Vault` H4 headings (lines 28, 40, 52) — but the **first `<CardGroup>` (lines 8–26: StateMind, Sherlock, MixBytes "Simple-LRT and DVV", ChainSecurity) has no heading at all**, sitting directly under the intro sentence "Mellow LRT smart contracts has been thoroughly audited..." — so on a current-gen page, unlabeled legacy "Mellow LRT" audits appear first and most prominently, before the labeled Core Vault section. Needs a decision: add an explicit heading (e.g. "Mellow LRT (Legacy)") to that first block and consider re-ordering so Core Vault audits lead.
- **MixBytes SyncRedeemQueue report** — exists and is already correctly placed under the `#### Core Vault` heading: `security.mdx:101` (`Mixbytes_202607`, "Mellow Finance SyncRedeemQueue Security Audit Report"). No action needed here, included for completeness.
- **Each Nethermind report under the Core Vault group** — nine Nethermind cards sit under `#### Core Vault` (`security.mdx` lines 59–107: NM_Core-Vaults_2025081, NM_2509, NM_0735, NM_0758, NM_0798, NM_0812, NM_0703, NM_0682, NM_0930, NM_0956, NM_0891) using opaque internal report codenames as both title and (nearly identical) body text ("Nethermind audit" repeated). Confirm these are all correctly scoped to Core Vaults and consider whether each should carry a more descriptive title (what each audit actually covered) instead of just an internal code.
- **The "DVV" section uses `<br />DVV<br />` instead of a real heading** (`security.mdx:110`) — already caught mechanically in Section 2.14 (broken markup), flagged again here because it's the one part of the audit-report structure that doesn't match the MultiVault/Interoperable Vault/Core Vault H4 pattern; worth fixing as part of any restructure of this page, not just as a heading-syntax fix.
- **Restaking case study on `/core-vaults/architecture`** — `core-vaults/architecture/index.mdx` lines ~380–472 contain a worked example built entirely around a Symbiotic/EigenLayer restaking subvault (see Section 2.1 rows for this file). Location noted; not rewritten here. Needs a replacement example using a non-restaking use case (e.g. stablecoin/RWA allocation, matching the site's current-gen framing).
- **"More than 60 operation-level onchain permissions" figure** — found once: `core-vaults-and-other-approaches.mdx` line 54, "60+ onchain permission types scoped by strategy, asset, operator, action." Confirm this figure is current/accurate.
- **Canonical integration list and live-vs-supported status per integration.** DeFi integration lists appear in at least three places with slightly different membership: `core-vaults-and-other-approaches.mdx:109` ("Aave, Morpho, Euler, Fluid, Spark, Ethena, Gearbox, Curve, Uniswap, Cowswap, Pendle, Symbiotic, EigenLayer, and others") vs. `mellow-vaults-overview.mdx:101` ("Live DeFi integrations include Aave, Morpho, Euler, Fluid, Gearbox, Curve, Uniswap, Cowswap, Pendle, Symbiotic, and EigenLayer" — no Spark or Ethena) vs. `core-vaults/architecture/index.mdx:361-388`'s protocol list for the restaking case study, which ends with a bare `* Hyperliquid` bullet (line 388) with no description. **Hyperliquid appears nowhere else in the corpus** (confirmed via full-text search) — not in `mellow-vaults-overview.mdx`'s integration list, not elsewhere — so it's inconsistent whether Hyperliquid is a live integration at all, and if so what kind (DeFi protocol vs. CEX-like venue, since it sits between the DeFi list and the "CEXes via Copper/Ceffu" heading at line 390). Needs a single canonical, live-vs-supported-tagged integration list.
- **Fee terminology conflict (management fee vs. the four defined fee types).** `core-vaults-integration-guide.mdx:1021` states "Fees in Mellow Core Vaults are paid in vault shares... The `FeeManager` contract calculates and deducts fees automatically." `core-vaults/architecture/managers/feemanager.mdx` was not grep-flagged by any category but should be cross-checked against this line and against `strategy-vault/overview.mdx:36-37` ("Platform fee: 1% annually... Performance fee: 10%...") for consistent fee-type naming across pages.
- **Whether the Points program is live.** `points/overview.mdx`, `points/points-in-symbiotic-pre-deposit-contracts.mdx`, and `points/defi-points-integration-instructions.mdx` all use present/future tense program language (e.g. points/overview.mdx:6 "Mellow believes early adopters play a significant role... through the points program, we want...") with no visible end-date or "program has ended" notice found via grep. `strategy-vault/rewards.mdx:33` (current-gen) separately promises "boosted Mellow Points for the first 4 weeks after launch (starting October 01, 2025)" — that window has already elapsed relative to today's date (2026-09-16), see Section 2.13. Needs a live/dead determination for the whole Points program and a correction to the stale strategy-vault/rewards.mdx window regardless.

### 4.2 Ilya (editorial/positioning)

- **Every occurrence of "onchain asset management" / "asset management" / "asset manager" as a descriptor of what Mellow is/does** — listed at full detail in Section 2.10 (11 occurrences). Notably `index.mdx:3` (site description, feeds `llms.txt`) and `mellow-vaults-overview.mdx:8` both use "Mellow is vault infrastructure for onchain asset management" as the primary tagline. Not changed here per instructions.
- **Archive-vs-delete for legacy trees** — open, not decided here. Applies to `restaking-vaults/`, `resources/mellow-lrt-depreciated/`, `dvsteth-vault/`, `points/`, `mellow-alm/`, `mellow-alm-toolkit/`.
- **Nav restructure need** — `docs.json` currently has "Mellow ALM" and "Points" as top-level nav groups, sitting outside the existing "Legacy" subgroup under Resources (which already nests Restaking Vaults + DVstETH Vault). See Section 1 for the full discrepancy note. Open decision on whether Mellow ALM and Points should move under Legacy.

### 4.3 Legal

Every `compliant`/`compliance`/`KYC`/`AML` occurrence is listed at full detail with surrounding context in Section 2.11 (43 occurrences, 40 current-gen). At a glance, the large majority read as **capability/configuration language** — "compliance rules are applied at the vault level," "configurable hooks for KYC/KYB providers," "enforce different compliance perimeters" (e.g. `core-vaults-for-rwa-allocation.mdx:81-88`, `core-vaults-for-stablecoins.mdx:98-106`, `vault-infrastructure-for-fintech-earn-products.mdx:46-56`) — i.e. Mellow provides configurable hooks that a curator/partner configures, not a claim that the vault itself is compliant. A smaller set reads closer to a direct claim and may warrant closer legal review: `core-vaults/architecture/modules/tokenizedsharemanager.mdx:7` ("fully compliant with the ERC20 standard" — technical standard, likely low risk), `core-vaults/architecture/modules/basemodule.mdx:49` ("confirms compliance" — ERC721 receiver hook, technical), and `core-vaults/architecture/index.mdx:280` ("Whitelisting for implementing KYC & compliance features" — Mellow providing the mechanism, not a compliance claim about itself). Full list and context in Section 2.11; no changes made.

## 5. Other observations (not actioned)

- **`quickstart.mdx` is not referenced anywhere in `docs.json` navigation** — current-generation content, orphaned from the nav tree, reachable only by direct URL. Likely an oversight; worth adding to the Overview group or removing.
- **`quickstart.mdx:46`** uses a placeholder support email `support@yourcompany.com` in an example — reads like unfinished template boilerplate rather than real Mellow contact info.
- **Integration-list inconsistency and Hyperliquid** already covered under Needs-human-decision (Andrey) above — flagging again here only because it's also a plain content-consistency issue independent of the legacy-vocab angle.
- **`core-vaults/architecture/index.mdx`** carries a disproportionate share of hits across almost every category (11 emoji-decorated headings, 5 Copper hits, the restaking case study, several curly quotes and em dashes) — it is by far the least consistency-pass-ready current-gen page in the corpus and would benefit from being tackled first in any follow-up editing pass.
- **`core-vaults-integration-guide.mdx`** alone accounts for 48 of the 92 em-dash hits and a large share of the on-chain hits — a single find/replace pass on this one file would resolve over half the em-dash count site-wide.
- **Mixed heading emoji convention**: `core-vaults/architecture/index.mdx` uses emoji prefixes on H4 headings (`#### 📥 Deposit Queue`) as a deliberate visual/navigation convention, not incidental — removing them per the no-emoji mechanic will need a design decision on what (if anything) replaces that visual scan-ability.
- Two apparent **template/generic strings**: `core-vaults/architecture/vaults/subvault.mdx:40` uses `"Mellow"` as an example `name_` value (fine, just noting it's a placeholder) and `quickstart.mdx`'s example contact (noted above).
