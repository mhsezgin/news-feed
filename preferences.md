# News Preferences

The scheduled agent reads this file every run and uses it to score and filter items.
You can edit it directly, or ask Claude Code conversationally ("less crypto, more EU AI regulation")
and it will update this file.

## Always include
- US Supreme Court rulings (full opinions, not just headlines)
- EU AI Act / DMA / DSA developments and enforcement actions
- Significant constitutional/court rulings in EU member states
- **Privacy & cybersecurity law**: regulator actions (FTC, CNIL, ICO, state AGs), major lawsuits, GDPR/CCPA enforcement, data breach litigation, cross-border data transfer rulings, surveillance law changes
- **Major cybersecurity incidents with legal/regulatory angle**: state-actor breaches, supply-chain attacks affecting policy, anything driving new regulation
- **Major AI company news**: Anthropic / OpenAI / Google DeepMind / xAI / Meta AI / Mistral — funding rounds, leadership moves, safety incidents, policy announcements, big partnerships
- **AI breakthrough news**: new SOTA results, capability jumps, important research papers (NeurIPS / ICML / arXiv flagged), major model releases, novel evaluations
- **New York personal injury law**: high-profile or precedent-setting NY personal injury cases (notable verdicts/settlements, appellate rulings, Court of Appeals decisions), and significant legal/regulatory changes affecting personal injury practice in NY (CPLR amendments, statute of limitations changes, insurance/no-fault reforms, scaffold law / Labor Law §240, comparative negligence rulings)

## Down-weight or skip
- Crypto price movements (regulation is fine; price action isn't)
- Celebrity tech CEO drama unless there's a real policy or legal angle
- US election horse-race coverage (substantive policy is fine; polling is not)
- Apple/Google/Samsung product rumor blogs

## Tone
- Prefer analysis over breaking-news regurgitation
- A 3-day-old in-depth piece beats a 2-hour-old wire summary
- When choosing between two items on the same story, pick the one with the most legal/policy depth

## Format
- Prefix every item title with a broad topic tag in square brackets, e.g. `[legal tech] ...`, `[law] ...`, `[ai] ...`, `[global politics] ...`, `[cybersecurity] ...`. Keep tags short (1-3 words), lowercase, and consistent across runs so similar items group visually in the reader.
