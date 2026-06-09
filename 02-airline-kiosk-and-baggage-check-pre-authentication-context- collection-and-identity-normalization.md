<!-- PageHeader="Airline Kiosk & Baggage Check — PreAuthentication, Context Collection & Identity Normalization" -->

# 4.1.1 Airline Kiosk & Baggage Check — Pre-Authentication, Context Collection & Identity Normalization  
**Part Two of "Enterprise-Trust Architecture: Identity as the Foundational Security Boundary"**  
**Author:** Patrick Slayden  
**Audience:** IT Professionals Transitioning Into Zero Trust Roles**  
**Classification:** Architectural White Paper**  
**Published:** 2026-06-06  
**Last Updated:** 2026-06-08  
**Version:** 3.5  

---

## 4.1.1.1 Executive Summary
---

In the **EnterpriseTrust Architecture** model, the airport journey begins long before TSA, and Section 4.1.1 marks the true architectural starting point—the moment a request first interacts with the enterprise. This stage is **not authentication** and **not authorization**; it is **context collection**: **identity lookup**, **attribute updates**, **device posture signals**, **entitlement changes**, and **environmental inputs** that shape the request before it reaches the first real enforcement point, the **Edge Gateway (TSA)**.

But this early phase performs a second, equally critical function: **Identity Normalization**, the creation of the enterprise’s **boarding pass**. Whatever identity the user brings—**Entra ID**, **federation**, **workload identity**, anything—is converted into the **single token format** the enterprise understands through the **Internal STS**, the service most organizations rely on but rarely describe clearly.

This paper expands Section 4.1.1 into a full, standalone narrative that recaps the broader **EnterpriseTrust Architecture**, introduces the **Trust Boundary Compass™**, and dives deep into the mechanics of **pre authentication**: what it is, what it is not, and why confusing this preparatory stage with trust inevitably leads to architectural breakdown.

---

## 4.1.1.2 Context: Where We Are on the Enterprise-Trust Journey
---

In the **EnterpriseTrust Architecture** journey, the airport metaphor frames **identity** as the foundational security boundary, and at this stage the traveler is still on the **public side** of the terminal.

### 4.1.1.2a External Network → Arriving at the Airport
---

You begin as an **unknown identity**—anyone can walk into the non-secure area of an airport, the enterprise equivalent of an **unauthenticated external network**.

The environment doesn’t know who you are, why you’re there, or what you intend to do. Maybe you’re flying, maybe you’re picking someone up, maybe you’re just grabbing lunch. **No trust decision** has been made yet.

---

## 4.1.1.3 Airline Kiosk & Baggage Check → PreAuthentication & Context Collection
---

The first meaningful interaction occurs when you approach the **airline kiosk** or counter. That moment signals **intent**: you want access to the secure side of the airport.

This is where **pre authentication** and **context collection** begin—your **identity is looked up**, **attributes are refreshed**, **device posture is gathered**, and **entitlements are checked**, all before the request moves toward the **Edge Gateway (TSA)**. This is the transition point where the enterprise finally has something to work with: **not trust, but context**.

---

## 4.1.1.4 The Trust Boundary Compass™
---

Before we go any deeper into pre-authentication, we have to introduce the **Trust Boundary Compass™**—the model that evaluates every request as it moves through the **EnterpriseTrust Architecture** environment.

The Compass defines **four pillars**, each visually separated for clarity and uniformity:

**Identity** — Who or what is making the request  
Spanning human identities, workload identities, service principals, managed identities, federated identities, and the claims, attributes, and entitlements attached to them.

**Device** — What the request is coming from  
Evaluating compliance, attestation, OS version, risk posture, token binding, and overall configuration and health, because a valid identity on a non-compliant device is not trusted.

**Network** — Where the request is coming from  
Focusing on network path, segmentation, service mesh identity, east-west authentication, and boundary transitions, EnterpriseTrust Architecture rejects the old “trusted network” model entirely.

**Workload** — What the request is trying to do  
Evaluating workload identity, API behavior, service-to-service calls, token exchange, multihop identity propagation, and workload posture or drift.

Pre-authentication is the first moment the Compass activates, but **none of these pillars grant trust** yet—they only collect **context** so the **Edge Gateway (TSA)** can make an informed decision.

---

## 4.1.1.5 Why the Compass Matters Here
---

The **Trust Boundary Compass** shows up before Section 4.1.1.5 for a reason: you need the **map** before you start the trip.

Pre authentication—the **airline kiosk moment**—is where the Compass first wakes up and starts doing its thing: **Identity** asks whether you even exist, **Device** wants to know what you’re using, **Network** checks where you wandered in from, and **Workload** wants to know what you’re trying to do.

But—and this is the part people love to get wrong—**none of these pillars grant trust** yet. They’re just collecting **context** so the first real enforcement point, the **Edge Gateway (TSA)**, can make an informed decision.

Section 4.1.1.6 is simply where the Compass actually comes online and starts judging your shoes, your bags, and your life choices—usually right before it tells you to throw away the donut you just bought.

Putting the Compass earlier gives the reader the **evaluation model** before they enter the pre-authentication zone, so they understand exactly what’s happening when the Compass snaps awake in 4.1.1.6.

---

## 4.1.1.6 Airline Kiosk & Baggage Check → PreAuthentication & Context Collection
---

**Pre authentication**—the airline kiosk zone of the EnterpriseTrust Architecture journey—is often misunderstood, so it’s worth stating plainly: this stage is **preparation, not permission**.

When you walk up to TSA, you don’t hand them your online reservation, your credit card, or your Entra ID login. You present a **boarding pass** — either the printed version generated by the kiosk or the mobile version that has already been bound to your phone.

When a request steps up to the “kiosk”, the enterprise isn’t trusting it; it’s collecting the **raw materials** needed for someone or something else to make the real call. **Identity is looked up**, **device posture is evaluated**, **attributes and entitlements are refreshed**, and all of that context is packaged into the enterprise’s version of a boarding pass: the **normalized identity token**.

That token is the format every downstream system understands, and it’s generated here—not at the Edge Gateway, and not during authentication proper.

The kiosk zone’s entire job is to **gather context**, **normalize identity**, and hand the request off to the first true enforcement point with everything it needs. **No access is granted. No trust is implied.** This is simply the prep work before the real decision happens.

**Takeaway:** **Pre authentication does not grant access.** It gathers the **identity** and **device context** required for the **Edge Gateway** to make an informed decision—nothing more.

---

## 4.1.1.7 The Kiosk → Output Medium (Not the STS)
---

TSA never evaluates your upstream identity provider. TSA evaluates the **token the STS issued**.

The kiosk is **not the STS**, and it’s definitely not the **identity authority**—it’s just the **output medium**. All the kiosk does is print the paper boarding pass that the enterprise already issued; the same way the airline website or mobile app simply delivers the digital version to your phone. Both are external delivery mechanisms, nothing more.

The actual identity authority is the **IdP—Entra ID** in our world—the system that stores your identity, validates your credentials, enforces MFA, maintains your attributes, and determines your account state.

The **Internal STS**, sitting safely inside your trust boundary, is the engine that takes that upstream identity and transforms it into the **single, standardized enterprise token**: it validates the identity the IdP proved, applies policy, normalizes claims, signs the token, enforces schema and lifetime, manages trust anchors, and handles legacy compatibility.

Some enterprises use:

• **PingFederate**  
• **ForgeRock**  
• **Keycloak**  
• **Home-grown STS**  
• **Legacy STS with transformation plugins**

It doesn’t matter which one you use.

What matters is: **The STS must be inside your trust boundary.**

Because only you control:

• **signing keys**  
• **claim schema**  
• **token lifetime**  
• **trust anchors**  
• **backward compatibility**  
• **legacy integration**

The Internal STS gives you **identity sovereignty**.

---

## 4.1.1.7a So What Does the STS Actually Do?
---

The **STS** doesn’t decide who you are; it **converts who you are** into the **one boarding pass format** the entire airport understands.

That’s why you end up with **two boarding passes**—one printed, one mobile—each representing a different **binding model**.

Wait?!? Two boarding passes? Yes.

The printed pass is **identity only**: standardized, TSA trusted, physical, and **not device-bound**.

The mobile pass is **identity plus device**: cryptographically bound to your phone, non-transferable, possession proof, and dynamically refreshed.

Both are identity because you proved who you are to get them, but only one carries **device binding**.

This distinction is the backbone of **EnterpriseTrust Architecture**, and it’s why **token normalization** is mandatory: your environment contains many identity formats:

• cloud services  
• on-prem services  
• legacy AD  
• LDAP  
• OID  
• SOAP services  
• mainframe endpoints  
• API gateways  
• service mesh  
• microservices  
• containers  

…and converts them into **one internal token format** so the enterprise can function as a unified system—the **“one boarding pass for the whole airport”** model.

**Note on the Metaphor:**  
The airport example uses two boarding passes—a printed pass and a mobile pass—to show the difference between **identity alone** and **identity bound to a device**. This helps illustrate why **device posture** and **token binding** matter. However, in the enterprise, you do not receive two tokens. You receive **one**: the **normalized enterprise token** issued by the Internal STS.

---

## 4.1.1.8 Identity Lookup at the Counter (Do You Even Exist?)
---

When you walk up to the airline kiosk, the very first thing it does is **look you up**. Before it prints anything, updates anything, or offers you anything, the system checks your **name**, **reservation**, **flight**, **seat**, **baggage allowance**, and **status**.

This is the airline verifying that you **exist** in its system. If you don’t? Nothing else proceeds.

EnterpriseTrust Architecture works exactly the same way: **no identity record = no flow**.

This is the **IdP lookup**, the foundational **“Do you exist?”** moment.

**Entra ID Example**

When a user enters their UPN at the kiosk, Entra ID performs a **directory lookup** to confirm the identity exists and retrieve **baseline attributes** such as account status, tenant membership, and assigned licenses.

**Compass Update:**  
Identity: Directory object found; baseline attributes loaded  
Device: Unknown  
Network: Source IP + geolocation captured  
Workload: Interactive login intent detected  

Identity exists—but **it is not trusted**

## 4.1.1.8a Human vs. Non-Human Entry Paths
---

Humans can exist in the **“public area”** without identity. **Non-human identities cannot.**

APIs, workloads, and AI agents never casually appear inside an enterprise. They don’t wander the lobby. They don’t browse the gift shop.

They only “arrive” when they present:

• **a client credential**  
• **a workload identity**  
• **a managed identity**  
• **a service principal**  
• **an agent persona**

Where humans begin with **zero context**, non-human identities begin with **declared intent**.

Both follow the same architectural stages, but their entry conditions differ:

• **Humans:** Public area → kiosk → TSA → secure zone  
• **Non-humans:** Identity assertion → pre authentication → gateway → secure zone  

---

## 4.1.1.9 Checking Bags → Attribute & Context Updates
---

Checking a bag doesn’t authenticate you—it simply **updates your attributes**. Your baggage count changes, your weight changes (not on the good side), your fees change, and your reservation metadata updates.

Nothing about that interaction makes you more trusted; it just enriches the airline’s understanding of your **state**.

EnterpriseTrust Architecture treats these signals the same way: a **new device**, **new claims**, **new workload metadata**, or **new entitlements** are **telemetry, not trust**. They update the identity object, but they don’t move you any closer to the secure side of the airport.

**Entra ID Example**

When a device registers with Entra ID or updates its compliance posture, the directory records new attributes such as **device ID**, **compliance state**, **OS version**, and **attestation status**. These updates do not authenticate the user — they simply enrich the identity object with fresh context.

**Compass Update:**  
Identity: Attributes enriched  
Device: Compliance posture collected  
Network: Logged  
Workload: Unchanged  

**Context increases—trust does not.**

---

## 4.1.1.10 Seat Upgrades → Attribute Changes That Affect Later Access
---

Upgrading your seat—whether at home, on your phone, or at the counter—does **not bypass TSA**. You don’t get to stroll past the line because you’re in 2A instead of 32B.

A seat upgrade simply changes what you’re **entitled** to after you authenticate.

EnterpriseTrust Architecture handles privilege changes the same way: **PIM elevation**, **new RBAC roles**, **updated claims**, or **new privileges** modify what you can do later, not what you can bypass now.

**Entra ID Example**

When a user activates a PIM role, Entra ID updates the identity's **effective privileges** and issues **time-bound role metadata**. This does not bypass Conditional Access or device compliance checks — it only changes what the user may be entitled to after authentication.

**Compass Update:**  
Identity: Privilege metadata updated  
Device: Unchanged  
Network: Logged  
Workload: Unchanged  

**Attributes change—trust does not.**

---

## 4.1.1.11 Adding a Second Device → Context Change, Not Trust
---

If work gives you a laptop and later adds a tablet, your identity now has **two devices** instead of one. That’s a **state change, not trust**.

EnterpriseTrust Architecture records each device’s **ID**, **compliance posture**, **attestation state**, **OS version**, and **risk baseline**.

More devices simply mean **more context** for the system to evaluate later—not a shortcut through the checkpoint.

**Entra ID Example**

When a user enrolls a second device into Entra ID, the directory simply adds another **device object** linked to the identity. Each device has its own **compliance posture**, **risk score**, and **attestation state**.

**Compass Update:**  
Identity: Multiple device objects linked  
Device: New device posture collected  
Network: Logged  
Workload: Unchanged  

**More devices = more context, not more trust.**

---

## 4.1.1.12 Buying a Donut Before TSA → Context That Doesn’t Cross the Boundary
---

You can buy a donut right in front of TSA. You can hold it, admire it, cherish it, and enjoy every warm, sugary moment.

But the second you step up to the checkpoint, TSA makes you **throw it away**.

This isn’t personal. It’s not TSA “taking your donut.” It’s **boundary policy enforcement**.

They don’t care that you bought it inside the airport, that you paid for it 30 seconds ago, or that it looks harmless. The rule isn’t about you—it’s about **protecting the system**.

EnterpriseTrust Architecture works the same way: **device posture**, **token binding**, **risk score**, **compliance**, and **claims** are evaluated **at the boundary**, not before it.

And yes, sometimes it feels obsessive. But it only takes **one donut bomb** to make things go sideways.

**Entra ID Example**

A user may browse internal public-facing resources or interact with pre-auth endpoints before authentication, but none of that activity carries forward into the trust decision.

**Compass Update:**  
Identity: No change  
Device: No change  
Network: Logged  
Workload: Pre-auth activity ignored  

**Pre-auth behavior is not trust input.**

---

## 4.1.1.13 Pre Authentication Summary (Enterprise Trust Model)
---

Before TSA, **nothing is trust**. Before TSA, **nothing is access**. Before TSA, **nothing crosses the boundary**.

Everything that happens before the **Edge Gateway (TSA)** is pure **context collection** — the system learning about the request, not granting anything to the request.

The pre-authentication phase gathers:

• **identity lookup** — “Who are you supposed to be?”  
• **attribute updates** — “What changed about you since last time?”  
• **device changes** — “Is the device still healthy, compliant, and recognizable?”  
• **entitlement changes** — “What are you allowed to do now versus yesterday?”  
• **environmental signals** — “Where are you coming from, and what’s happening around you?”  
• **behavioral patterns** — “Is this normal for you, or is something off?”  

All of this **enriches the request**. None of this **grants access**. None of this **is trust**. None of this **crosses the boundary**.

**Entra ID Example**

Entra ID collects **identity attributes**, **device posture**, **risk indicators**, and **entitlement metadata** during pre-authentication, but none of these signals grant access on their own. Entra ID assembles the full **Compass snapshot** “Preparing for TSA”.

**Compass Update:**  
Identity: Fully enriched  
Device: Posture collected  
Network: Path recorded  
Workload: Intent identified  

---

## 4.1.1.14 End-to-End Takeaway
---

The **Edge Gateway (TSA)** is the **first real enforcement point**. Pre-authentication **prepares** the request; the Gateway **decides** what actually gets through.

---

## 4.1.1.15 Final Note: Donuts & Boundaries
---

If you want your donut, finish it before TSA — because the **Gateway doesn’t negotiate with pastries**.

---

## 4.1.1.16 Next in the Series
---

The next installment covers **The TSA Checkpoint (Edge Gateway)**, where pre-authentication ends and the first real trust decision is made.

---

## 4.1.1.17 Sources (Pre Authentication & Context Collection)
---

• **NIST SP 800 63 3** — Digital Identity Guidelines  
• **NIST SP 800 207** — Zero Trust Architecture  
• **NIST SP 800 204B** — Attribute Based Access Control  
• **Microsoft Entra ID** Conditional Access & Device Compliance  
• **Azure AD PIM** (Privileged Identity Management)  
• **OpenID Connect Core**  
• **TSA Security Screening Procedures**  
• **OWASP API Security Top 10**
.
