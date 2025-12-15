# Section 13 – Advanced Amazon S3

This section focuses on S3 storage classes, lifecycle rules, and cost optimization scenarios commonly tested in the DVA-C02 exam.

---

## S3 Storage Classes (Conceptual)

S3 provides multiple storage classes for different access patterns.

- **Standard**: frequent access, highest cost
- **Standard-IA**: infrequent access, cheaper storage, retrieval cost
- **Intelligent-Tiering**: auto moves objects based on usage
- **One Zone-IA**: single AZ, cheaper, no redundancy
- **Glacier tiers**: archive storage, very low cost, slow retrieval

Key idea:  
👉 **Lower cost = slower retrieval + less availability**

---

## Moving Between Storage Classes

Objects can transition between storage classes over time.

- Frequently accessed → Standard
- Infrequently accessed → Standard-IA
- Archive data → Glacier / Deep Archive

Transitions are usually automated using **Lifecycle Rules**.

---

## S3 Lifecycle Rules

Lifecycle rules automate object management based on **object age**.

### Transition Actions

Move objects to another storage class after X days since creation.

Examples:
- Move to Standard-IA after 60 days
- Move to Glacier after 6 months

Important:
- Transition is based on **creation time**
- Not based on whether the object is still being accessed

---

### Expiration Actions

Automatically delete objects after a specified time.

Common uses:
- Delete access logs
- Delete old versions (when versioning is enabled)
- Delete incomplete multipart uploads

---

## Rule Scope

Lifecycle rules can apply to:
- Entire bucket
- Specific prefix (e.g. `/thumbnails/`)
- Objects with specific tags

---

## Typical Exam Scenarios

- Source images stored in **Standard**, later archived to Glacier
- Thumbnails stored in **One Zone-IA** and deleted after some time
- **Versioning + lifecycle rules** used to recover deleted objects

---

## S3 Analytics

- Helps decide when to transition objects
- Works for Standard → Standard-IA only
- Generates CSV reports
- Used to optimize lifecycle rules

---

## Exam Takeaways

- Lifecycle rules are based on **object age**
- Glacier is cheap because retrieval is slow
- Intelligent-Tiering is best when access pattern is unknown
- Versioning + lifecycle rules is a common exam pattern
