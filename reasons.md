The killer argument: **the client app knows the intersection today, but they don't control what happens after.**

## The Real Value: Lifecycle Management

When the client app computes "female engineers in US" and grants access to document A, that's a **point-in-time decision**. But permissions are alive:

**What happens when a user leaves group:gender__female?**

- If permissions are stored in the client app's DB: they must scan every document, every role, every policy that references that group, and update accordingly
- If stored in OpenFGA: remove one tuple (`user:alice` → `member` → `group:gender__female`), and every `Check` across every document, every role, instantly reflects it

**What happens when role:campaign_42 is revoked from document A?**

- One tuple delete. Every user loses access immediately. No fan-out.

**What happens when a new microservice needs to check permissions?**

- Without OpenFGA: the client must replicate their permission data and logic to every service, keep them in sync
- With OpenFGA: every service calls `Check` against one source of truth

## The Pitch

> "You own the business logic — deciding *who should* have access. We own the authorization infrastructure — answering *who does* have access, consistently, across every service, in real time, as things change. You write the grants. We enforce them. Neither of us does the other's job."

The value isn't intersection. It's **centralized enforcement with decentralized decision-making.**
