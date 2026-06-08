<!-- PageHeader="Airline Kiosk & Baggage Check > Pre-Authentication & Context Collection" -->

# 4.1.1 Airline Kiosk & Baggage Check > Pre-Authentication & Context Collection  
**Part Two of "Enterprise-Trust Architecture: Identity as the Foundational Security Boundary"**  
**Author:** Patrick Slayden  
**Audience:** IT Professionals Transitioning Into Zero Trust Roles**  
**Classification:** Architectural White Paper**  
**Published:** 2026-06-06  
**Last Updated:** 2026-06-08  
**Version:** 3.0  

---

## 4.1 Executive Summary

In the Enterprise-Trust model, the airport journey begins long before TSA. Section 4.1.1 is where the architecture truly starts: the moment a request first interacts with the enterprise. This stage is not authentication and not authorization. It is **context collection** — identity lookup, attribute updates, device signals, and entitlement changes that prepare the request for the first real enforcement point: the Identity Gateway (TSA).

This paper expands that section into a full, standalone narrative. It recaps the broader Enterprise-Trust model, introduces the **Trust Boundary Compass™**, and then dives deep into the mechanics of pre-authentication: what it is, what it is not, and why confusing it with trust leads to architectural breakdown.

---

## 4.2 Context: Where We Are in the Enterprise-Trust Journey

In the main Enterprise-Trust paper, the airport metaphor frames identity as the foundational security boundary. The journey so far:

### 4.2.1 External Network > Arriving at the Airport

You begin as an unknown identity. Humans can freely enter the public side of the airport — the enterprise equivalent of an unauthenticated external network. The airport doesn't know who you are or why you're there. Maybe you're flying. Maybe you're picking someone up. Maybe you're just grabbing lunch. No trust decision has been made yet.

### 4.2.2 Airline Kiosk & Baggage Check > Pre-Authentication & Context Collection

The first meaningful interaction occurs when you approach an airline kiosk or counter. That moment signals intent: you want access to the secure side of the airport. This is where pre-authentication begins.

---

## 4.3 The Trust Boundary Compass™  
### *How Enterprise-Trust Evaluates Identity Across the Journey*

Before diving deeper into pre-authentication, we must introduce the **Trust Boundary Compass™** — the model that explains how Enterprise-Trust evaluates every request as it moves through the environment.

The Compass defines **four pillars** that every request is evaluated against.

---

### 4.3.1 Identity — The Primary Boundary

Identity answers the question: **Who or what is making the request?**

This includes:

- human identities  
- workload identities  
- service principals  
- managed identities  
- federated identities  
- claims, attributes, and entitlements  

Identity is the core of Enterprise-Trust. Every other pillar is context around identity.

---

### 4.3.2 Device — The Posture of the Request

Device answers: **What is the request coming from?**

Enterprise-Trust evaluates:

- device compliance  
- attestation  
- OS version  
- risk posture  
- token binding  
- configuration and health  

A valid identity on a non-compliant device is **not trusted**.

---

### 4.3.3 Network — The Path, Not the Trust

Network answers: **Where is the request coming from?**

Not in the old “trusted network” sense — Enterprise-Trust rejects that idea — but in terms of:

- network path  
- segmentation  
- service mesh identity  
- east-west authentication  
- boundary transitions  

Network is **context**, not trust.

---

### 4.3.4 Workload — The Intent of the Request

Workload answers: **What is the request trying to do?**

Enterprise-Trust evaluates:

- workload identity  
- API behavior  
- service-to-service calls  
- token exchange  
- multi-hop identity propagation  
- workload posture and drift  

Workloads behave like passengers: they move, they change, and they must be continuously evaluated.

---

## 4.3.5 Why the Compass Matters Here

Pre-authentication (the airline kiosk) is where the Compass first activates:

- **Identity** → Do you exist?  
- **Device** → What are you using?  
- **Network** → Where are you coming from?  
- **Workload** → What are you trying to do?  

But — and this is the key — none of these pillars grant trust at this stage.

They only collect context so the first real enforcement point — the Identity Gateway (TSA) — can make a decision.

**The Compass lives in Section 3 because you need the map before you start the trip. 4.1.1 is where the Compass actually comes online and starts judging your shoes, your bags, and your life choices — usually right before it tells you to throw away the donut you just bought.**

This placement gives the reader the evaluation model before they enter the pre-authentication zone, so they understand what the Compass is doing when it wakes up in 4.1.1.

---

## 4.4 Pre-Authentication & Context Collection (The Kiosk Zone)

**Takeaway:** Pre-authentication does not grant access. It collects identity and device context so the Identity Gateway can make a real decision. Nothing more.

---

### 4.4.1 Identity Lookup at the Counter (Do You Even Exist?)

**Traveler Experience:**  
You walk up to the airline kiosk. Before anything else happens, the system checks:

- your name  
- your reservation  
- your flight  
- your seat  
- your baggage allowance  
- your status  

This is the airline verifying you exist in their system. If you don't? Nothing else proceeds.  
No bags. No upgrades. No boarding pass. You're done.

**Same in Enterprise-Trust:**  
**No identity record = no flow.**

#### Entra ID Example

When a user enters their UPN at the kiosk, Entra ID performs a directory lookup to confirm the identity exists and retrieve baseline attributes such as account status, tenant membership, and assigned licenses. No token is issued and no trust is granted — this is simply the identity equivalent of the airline verifying your reservation. If the identity doesn't exist, the flow ends immediately.

#### Compass Update

- **Identity:** Directory object found; baseline attributes loaded  
- **Device:** Unknown  
- **Network:** Source IP + geolocation captured  
- **Workload:** Interactive login intent detected  

Identity exists — but is not trusted.

---

### 4.4.2 Checking Bags > Attribute & Context Updates

**Traveler Experience:**  
Checking a bag doesn't authenticate you. It updates your attributes:

- baggage count  
- weight  
- fees  
- reservation metadata  

If you show up with a second bag that costs extra, your state changes. Not malicious. Not suspicious. Just context.

Enterprise-Trust treats this the same way:

- new device  
- new claims  
- new workload metadata  
- new entitlements  

These are **telemetry**, not trust.

#### Entra ID Example

When a device registers with Entra ID or updates its compliance posture, the directory records new attributes such as device ID, compliance state, OS version, and attestation status. These updates do not authenticate the user — they simply enrich the identity object with fresh context.

#### Compass Update

- **Identity:** Known; attributes enriched  
- **Device:** Compliance posture collected  
- **Network:** Logged  
- **Workload:** Login intent unchanged  

Context increases — trust does not.

---

### 4.4.3 Seat Upgrades > Attribute Changes That Affect Later Access

**Traveler Experience:**  
Upgrading your seat — at home, on your phone, or at the counter — is another attribute change.

You're not bypassing TSA. You're not getting special treatment at the checkpoint. You're simply changing what you're entitled to **after** you authenticate.

Just like:

- PIM elevation  
- new RBAC roles  
- updated claims  
- new privileges  

#### Entra ID Example

When a user activates a PIM role, Entra ID updates the identity's effective privileges and issues time-bound role metadata. This does not bypass Conditional Access or device compliance checks — it only changes what the user may be entitled to after authentication.

#### Compass Update

- **Identity:** Privilege metadata updated  
- **Device:** Posture unchanged  
- **Network:** Logged  
- **Workload:** Intent unchanged  

Attributes change — trust does not.

---

### 4.4.4 Adding a Second Device > Context Change, Not Trust

**Traveler Experience:**  
If work gives you a laptop and later adds a Windows tablet, your identity now has two devices instead of one.

That's a **state change**, not trust.

Enterprise-Trust records:

- device ID  
- compliance posture  
- attestation  
- OS version  
- risk baseline  

#### Entra ID Example

When a user enrolls a second device into Entra ID, the directory simply adds another device object linked to the identity. Each device has its own compliance posture, risk score, and attestation state.

#### Compass Update

- **Identity:** Multiple device objects linked  
- **Device:** New device posture collected  
- **Network:** Logged  
- **Workload:** Intent unchanged  

More devices = more context, not more trust.

---

### 4.4.5 Buying a Donut Before TSA > Context That Doesn't Cross the Boundary

**Traveler Experience:**  
You can buy a donut right in front of TSA. You can hold it. You can eat it. You can enjoy it.

But the moment you walk up to the checkpoint, they make you throw it away.

This is not malicious. It's not personal. It's not TSA “taking your donut.”  
It's **boundary policy enforcement**.

TSA doesn't care:

- that you bought it inside the airport  
- that you paid for it 30 seconds ago  
- that it looks harmless  

Because the rule isn't about you. It's about protecting the entire system.

Enterprise-Trust works the same way:

- device posture  
- token binding  
- risk score  
- compliance  
- claims  

These rules apply **at the boundary**, not before it.

And yes — sometimes it feels obsessive.  
But it only takes **one donut bomb** to make things go sideways.

#### Entra ID Example

A user may browse internal public-facing resources or interact with pre-auth endpoints before authentication, but none of that activity carries forward into the trust decision.

#### Compass Update

- **Identity:** No change  
- **Device:** No change  
- **Network:** Logged  
- **Workload:** Pre-auth activity ignored  

Pre-auth behavior is not trust input.

---

### 4.4.6 Pre-Authentication Summary

Everything before TSA is **context collection**:

- identity lookup  
- attribute updates  
- device changes  
- entitlement changes  
- environmental signals  
- behavioral patterns  

None of this grants access.  
None of this is trust.  
None of this crosses the boundary.

### **The Identity Gateway (TSA) is the first real enforcement point.**

Pre-authentication prepares the request.  
The Gateway decides what actually gets through.

**If you want your donut, finish it before TSA — because the Gateway doesn't negotiate with pastries.**

#### Entra ID Example

Entra ID collects identity attributes, device posture, risk indicators, and entitlement metadata during pre-authentication, but none of these signals grant access on their own.

#### Compass Update

- **Identity:** Fully enriched  
- **Device:** Posture collected  
- **Network:** Path recorded  
- **Workload:** Intent identified  

The Compass is complete — but unused until TSA.

---

## 4.7 End-to-End Example: Entra ID Pre-Authentication Flow Using the Trust Boundary Compass™

### 4.7.1 Arrival at the Airport > External Network (Unknown Identity)

A user opens their laptop at home and navigates to a corporate application.

**Compass Update:**

- Identity: Unknown  
- Device: Unknown  
- Network: Home ISP  
- Workload: Unknown intent  

---

### 4.7.2 Approaching the Kiosk > Hitting the Pre-Auth Endpoint

The user enters their UPN into the login page.

**Compass Update:**

- Identity: UPN provided  
- Device: Browser user-agent + device ID (if registered)  
- Network: Source IP + geolocation  
- Workload: Interactive login intent  

---

### 4.7.3 Identity Lookup > "Do You Even Exist?"

Entra ID checks the directory.

**Compass Update:**

- Identity: Directory object found  
- Device: Unknown  
- Network: Logged  
- Workload: Login intent confirmed  

---

### 4.7.4 Device Posture Collection > "Checking Your Bags"

Device sends compliance + attestation.

**Compass Update:**

- Identity: Known  
- Device: Compliance posture collected  
- Network: Logged  
- Workload: Login intent  

---

### 4.7.5 Risk Evaluation > "Security Flags on Your Luggage"

Identity Protection evaluates risk.

**Compass Update:**

- Identity: Risk score attached  
- Device: Compliance + attestation  
- Network: Path evaluated  
- Workload: Intent unchanged  

---

### 4.7.6 Attribute Updates > "Seat Upgrades & Metadata"

PIM, RBAC, claims, MFA updates.

**Compass Update:**

- Identity: Privilege metadata updated  
- Device: Posture unchanged  
- Network: Logged  
- Workload: Intent unchanged  

---

### 4.7.7 Pre-Auth Activity > "Buying a Donut Before TSA"

Pre-auth activity is ignored.

**Compass Update:**

- Identity: No change  
- Device: No change  
- Network: Logged  
- Workload: Pre-auth activity ignored  

---

### 4.7.8 Compass Assembly > "Preparing for TSA"

Entra ID assembles the full Compass snapshot.

**Compass Update:**

- Identity: Fully enriched  
- Device: Posture collected  
- Network: Path recorded  
- Workload: Intent identified  

---

### 4.7.9 Approaching TSA > The Identity Gateway

The request reaches the first real enforcement point.

---

## 4.8 End-to-End Takeaway

Pre-authentication prepares the request.  
The Identity Gateway decides what gets through.

---

## 4.9 Final Note: Donuts & Boundaries

**If you want your donut, finish it before TSA — because the Gateway doesn't negotiate with pastries.**

---

## 4.10 Next in the Series

The next installment in this series will cover **4.2 — The TSA Checkpoint (Identity Gateway)**, where pre-authentication ends and the first real trust decision is made.

---

## 4.11 Sources (Section 4.1.1 — Pre-Authentication & Context Collection)

These sources support the identity lookup, attribute updates, device-signal handling, and boundary-enforcement concepts in this section:

1. NIST SP 800-63-3 — Digital Identity Guidelines  
2. NIST SP 800-207 — Zero Trust Architecture  
3. NIST SP 800-204B — Attribute-Based Access Control  
4. Microsoft Entra ID Conditional Access & Device Compliance  
5. Azure AD Privileged Identity Management (PIM)  
6. OpenID Connect Core  
7. TSA Security Screening Procedures  
8. OWASP API Security Top 10  
