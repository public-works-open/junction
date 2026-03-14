# Social Commons — Conversation Handoff Document

This document contains sufficient context to continue an ongoing design conversation about a decentralised social infrastructure project. No marketing framing. Technical and conceptual detail only.

---

## The Idea in One Paragraph

A decentralised protocol whose fundamental primitive is a **bilateral, physically-attested relationship between two verified humans**. Not a profile. Not a post. The relationship. The system creates a global graph of real human connections, anchored to physical co-presence, on top of which applications can be built that inherit social trust rather than building it from scratch. The goal is open, composable social infrastructure owned by no single entity — analogous to what Unix/TCP-IP is to computing or what Ethereum is to programmable money.

---

## Architecture

### Two Layers

**1. Trust Anchor Layer**
- Records physical attestations: proof that two real humans were in the same physical location at the same time, both running genuine software on genuine hardware
- Immutable once recorded
- Entry point into the network — you enter by being physically vouched for by someone already in it
- Does NOT need to be where relationships form, only where humanity is verified

**2. Relationship Layer**
- Records the ongoing texture of connection: interactions, shared communities, collaboration, time, digital communication
- Dynamic, richer, more fluid
- Relationships can form and deepen here without physical attestation
- Physical attestation upgrades a relationship from **unanchored** to **anchored**
- Applications decide what trust level they require; not everything needs anchored relationships

### Key Design Decision (recently resolved)

Physical attestation is an **onboarding mechanism and trust anchor**, not a prerequisite for forming relationships. Once a person has a verified node in the graph (via at least one physical attestation), they can form relationships digitally. Unanchored relationships are visible in the graph and real; anchored ones carry higher trust. This resolves the growth bottleneck — the network doesn't require physical meetings for every connection, just for initial node verification.

---

## The Core Primitive

The **attestation object** — what gets recorded when two people physically meet and both initiate the handshake:

- Proximity mechanism signal (see below)
- Timestamp (tight synchronisation across both devices required)
- GPS coordinates (independently recorded on each device)
- Environmental witness data: ambient audio hash and/or ambient light hash — unique acoustic/visual fingerprint of that moment and place
- Cryptographic signatures from both parties' keypairs
- Device attestation: both parties running genuine app on genuine hardware (TPM/Secure Enclave)
- Bilateral: both parties must initiate, neither can unilaterally create an attestation

---

## Physical Proximity Mechanisms

Three options analysed, each with different tradeoffs:

### NFC
- Max range ~4cm, physics-enforced
- Requires deliberate mutual action — maps to handshake gesture
- Momentary, no persistent channel
- Relay attack possible but requires expensive hardware and coordination; detectable via timing + location inconsistency
- Gold standard philosophically; most honest physical signal
- Limitation: ~50-60% global smartphone penetration currently, lower in Sub-Saharan Africa/South Asia; trending toward universal in 5-7 years
- **Preferred primary mechanism**

### Ultrasonic Handshake
- Works on essentially all smartphones (speaker + microphone universal)
- Range ~1-2m, limited by audio physics
- Requires deliberate initiation from both parties
- Signal is audible — the handshake makes a sound, social signal present
- Slightly more automatable than NFC for sophisticated attackers
- Can incorporate ambient audio environment as co-presence witness
- **Preferred fallback / universal compatibility mechanism**

### Optical Mutual Handshake
- Works on any phone with camera and screen
- Rapidly changing visual patterns, time-locked, non-replayable
- Range limited by camera optics (~0.5m)
- Poetically consistent with "I see you" framing
- Most vulnerable to sophisticated attack (cameras/screens are software-controllable)
- Can incorporate ambient visual/light conditions as witness
- **Third option; more caution required in design**

**Current recommendation:** NFC as primary + ultrasonic as universal fallback. BLE explicitly rejected — range too large (10m+), no natural social gesture, passive enough to automate.

---

## Environmental Witness Data

The attestation records a hash of the ambient environment at moment of handshake:
- Ambient audio fingerprint (both devices must agree within tolerance)
- Ambient light/visual conditions (both devices)
- Makes each attestation unrepeatable — that exact environment never exists again

**Limitation:** Synthetic environmental data can be generated. Generative AI will make this easier over time. Therefore environmental data is **additional friction, not primary security**. It raises the cost of fake attestations today but has a shelf life.

---

## Security Model

The primary threat is **scale attacks** — coordinated injection of thousands of fake relationships to pollute the graph. Individual fakes are tolerable.

### Layered Defence (no single mechanism sufficient)

1. **Physical proximity mechanism** — requires real hardware in real locations; expensive to fake at scale
2. **Environmental witness** — raises cost today; degrades as generative AI improves
3. **Graph mathematics** — real human social networks have statistically specific properties: degree distribution, clustering coefficients, geographic and temporal coherence. Fake graphs look different. Anomalous subgraphs detectable automatically.
4. **Device attestation** — TPM/Secure Enclave attestation that genuine app is running on genuine unmodified hardware
5. **Social layer** — real communities notice strangers; reputation in a social graph is only valuable if it can be spent in the real world where the network lives; fake identity can't do that

**Fundamental:** The security model is ultimately social, not technical. Technical mechanisms raise the cost of attacks. Social embedding is the final defence. This is appropriate for a system whose primitive is human trust.

**Attack surface advantage over Ethereum:** Ethereum attacks are purely computational; this system's attack surface requires physical presence in the real world. Cost-benefit for attackers is less clear than in financial systems.

---

## Hardest Unsolved Problems

### Key Recovery
Losing keypair = losing social history. Proposed solution: **social recovery** — a threshold of most-trusted connections collectively attest to a new key. Cryptography for doing this securely and privately (without revealing the social graph) is genuinely hard. zkSNARKs likely part of the answer.

### Privacy
The relationship graph is extraordinarily sensitive — more so than financial history. Zero knowledge proofs likely necessary: prove you have a certain depth of social connection without revealing who those connections are. Full design TBD.

### Governance
DAO-like but weighted by social graph depth rather than token holdings. Decisions require broad consensus across many weakly-connected clusters — mirrors healthy human governance. Specific mechanism not yet designed.

---

## The Two Founders (Background)

**User (referred to as "the user"):**
- BSc Physics & Applied Mathematics, University of Galway (First Class Honours)
- MPhil Machine Learning & Machine Intelligence, Cambridge (2019-2020); thesis on Multimodal Representation Learning with Contrastive Predictive Coding
- Co-Founder, Fragmynt — Web3 prediction markets, backed by Accel & Entrepreneur First (~$500k)
- Project Developer, Tools for the Commons — blockchain/modular governance for network cities, São Paulo
- Lead Software Engineer, Sony R&D Brussels (current)
- Profile: infrastructure thinker, ML/blockchain/systems depth, conceptual articulator

**Potential cofounder ("the friend"):**
- University of Rochester, BS Computer Science
- Microsoft Software Engineer → SWE II, Seattle (2020-2023)
- Founded Tribbel — IRL social app (~2 years, Mar 2023-Apr 2025)
- Brief stints: Henry AI (YC S24), Cassi CPO/Head of Product
- Currently: Co-Founder & CTO, stealth startup (Oct 2025-present) — continuation of the IRL social app
- Egyptian, based in Cairo; connected across African and Arab networks; multilingual content creator
- Profile: product builder, community connector, embedded across multiple cultures, natural network node

**Relationship:** Met via YC cofounder matching. Have not met in person yet. User is visiting Cairo for 3 days during Ramadan. Meeting is personal-first, exploratory for venture.

**The friend's existing app** is framed as: the first proof of concept of the trust anchor layer. It already does the hardest thing — gets real humans to show up in physical space and encodes that digitally. It is the genesis node, not a separate product.

---

## Conversation Strategy for Cairo Meeting

Do not lead with the idea. Build relationship first. Let the friend describe his startup and listen for the emotional reality underneath. Then ask: "Have you ever thought about what would need to be true for something like this to become infrastructure rather than a product?" Tell the story, not the pitch. Let it feel discovered together.

---

## Build Sequence (agreed)

1. Write the whitepaper first — philosophically coherent document before code. First social act of the network. Attracts the right people.
2. Find ~10 people who immediately get it.
3. Build the smallest possible proof of concept: two phones, attestation mechanism, relationship recorded on a simple shared ledger. Nothing more.
4. Stop. Watch what those 10 people want to build on top.
5. Let layer two be informed by layer one usage. Resist building layer two before layer one is solid.
6. Open the protocol.

**Timeline benchmarks:** Ethereum launched 2015, DeFi emerged 2020, mainstream cultural relevance 2021. Six years from launch to meaningful ecosystem. A decade+ to self-sustaining network is the realistic expectation.

**First community target:** Cairo. The friend's existing users. African Leadership Academy network as early nodes across the continent.

---

## Key Analogies Used (for framing, not for pitching)

- **Unix:** didn't try to be every application; provided stable, open, composable primitives; owned by the commons; outlasted proprietary competitors
- **Ethereum:** proved you can bootstrap global financial infrastructure through social consensus; whitepaper came before code; attracted right people first; DAO fork showed healthy decentralised social governance
- **Cathedral:** built over generations; technically extraordinary and communally meaningful; owned by community, not builders; designed to endure

---

## What Is Still Open / Needs Further Design

- Exact fields and encoding of the attestation object
- Whether attestation strength is tiered by mechanism (NFC > ultrasonic > optical) or uniform
- Privacy architecture — ZK proof design
- Key recovery cryptography
- Governance mechanism specifics
- Whitepaper — not yet written
- Name — not decided
