# Sources, assumptions and open decisions

Reviewed 2026-10-01. Product capabilities are vendor statements; exact project applicability still requires the part, SDK and ECU configuration.

1. [NXP S32K5 product page](https://www.nxp.com/products/S32K5): lists Neutron NPU and eIQ Auto enablement. Do not infer that every graph/operator or runtime path is qualified for the project's safety allocation.
2. [NXP and COMPREDICT virtual-sensor demonstration](https://www.nxp.com/company/about-nxp/smarter-world-blog/BL-NXP-EDGE-AI-COMPREDICT-INNOVATION): describes INT8 wheel-force model execution on R52 in the S32K5 VDK. This is feasibility evidence, not measured production ECU/NPU latency.
3. [NXP eIQ Auto ML SDK](https://www.nxp.com/design/design-center/software/eiq-auto-ml-sw-kit%3AEIQ-AUTO): toolchain entry point. Pin actual installed versions and supported operators during platform assessment.
4. [Original project proposal](../VMC_Edge_AI_Cloud_Edge_Collaboration_Proposal.md): retained September 24 discussion and wider use-case inventory. This package refines deployment and safety details.

ISO 26262, AI safety assurance, cybersecurity and software-update obligations must be mapped by the project owners to the applicable editions and market requirements. This draft makes no standards-conformance or certification claim.

## Open decisions before implementation

| Decision | Owner / required evidence |
| --- | --- |
| Exact S32K5 part, silicon/VDK availability, runtime and operator support | ECU/NXP integration team; compile and target benchmark |
| Partition, task periods, budgets and shared-resource isolation | Software/safety architecture owners |
| NXP three class names, confidence semantics and interface | NXP interface review |
| Released DBC/SIDL mapping and source ages | Interface owners; current files |
| State correction limits and permitted driving modes | VMC function/safety owners; sensitivity and hazard analysis |
| Ground truth and held-out campaign | Test/ML team; instrumented vehicles |
| Gateway OTA transport, slot storage and trust anchor | Cybersecurity/OTA owners |
| Dataset authorization and publication scope | Data/repository owner |

No current software model or DBC was reverse engineered for this package. Architecture is a proposal aligned to the earlier project discussion, not a statement of implemented SWCs.
