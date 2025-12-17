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

## 📣 S3 Event Notifications – Key Exam Points

### Purpose of S3 Event Notifications
S3 Event Notifications allow Amazon S3 to **react to events** (such as object creation or deletion) by notifying or triggering other AWS services.

Common events include:
- ObjectCreated
- ObjectRemoved
- ObjectRestore
- Replication

Typical use case:
Automatically trigger actions when something happens in S3 (e.g. generate image thumbnails).

---

### Native Destinations Supported by S3
Amazon S3 can directly send event notifications to:
- **SNS**
- **SQS**
- **Lambda**

These are the default and most commonly tested destinations.

---

### IAM Permission Model (Very Important)
S3 Event Notifications **do NOT use IAM roles on S3**.

Instead:
- Permissions are granted using **resource-based policies on the target service**

Examples:
- SNS → SNS resource policy allows S3 to publish
- SQS → SQS resource policy allows S3 to send messages
- Lambda → Lambda resource policy allows S3 to invoke the function

This permission model is similar to an S3 bucket policy.

---

### S3 Events and Amazon EventBridge
All S3 events are automatically sent to **Amazon EventBridge**.

EventBridge provides:
- Advanced filtering using JSON rules
- Multiple destinations at the same time
- Event archive and replay
- More reliable delivery

Use EventBridge when:
- Complex filtering is required
- Multiple AWS services must be triggered
- Event replay or long-term reliability is needed

---

### Typical Exam Use Case
**Automatically generate thumbnails when images are uploaded to S3**

Common design:
- Event: `ObjectCreated`
- Filter: `.jpg`
- Destination: **Lambda**
- Lambda processes the image and creates thumbnails

## S3 Performance – Key Takeaways

### 1. Automatic Scaling and Baseline Performance
Amazon S3 automatically scales to handle very high request rates without manual provisioning.  
Typical time to first byte latency is around **100–200 ms**.

---

### 2. Performance Is Measured *Per Prefix*
S3 performance limits apply **per second per prefix**:
- Up to **3,500 PUT / COPY / POST / DELETE** requests per second per prefix
- Up to **5,500 GET / HEAD** requests per second per prefix  

A *prefix* is the path between the bucket name and the object name.  
There is **no limit on the number of prefixes**, so spreading objects across prefixes increases total throughput.

---

### 3. Multi-Part Upload for Faster Uploads
Multi-part upload splits large files into smaller parts and uploads them in parallel:
- **Recommended** for files larger than **100 MB**
- **Required** for files larger than **5 GB**  

This maximizes bandwidth usage and significantly speeds up uploads.

---

### 4. S3 Transfer Acceleration for Long-Distance Transfers
S3 Transfer Acceleration improves upload and download speed by:
- Uploading data to the nearest AWS **Edge Location**
- Transferring data to the target S3 bucket over the **AWS private network**

It is especially useful for cross-region or cross-continent transfers and is compatible with multi-part upload.

---

### 5. S3 Byte-Range Fetches for Faster and Partial Downloads
Byte-range fetches allow clients to request specific byte ranges of an object:
- Enables **parallel GET requests**
- Improves download speed
- Provides better resilience by retrying only failed ranges
- Can retrieve **partial data** (e.g., file headers) without downloading the entire object
