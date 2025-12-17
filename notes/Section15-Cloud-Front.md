# CloudFront Overview

🌍 **CloudFront is AWS’s Content Delivery Network (CDN)**  
👉 In the exam, whenever you see **CDN**, think **CloudFront**.

CloudFront improves read performance and user experience by caching content at global edge locations and serving users from the nearest location.

---

🚀 **Core Benefits**
- ⚡ Cache content close to users (edge locations)
- 🌐 Low latency for global users
- 🧠 Reduced load on origins (S3, EC2, ALB)
- 📈 Improved availability and scalability
- 🛡️ Built-in protection against large-scale attacks
- 🔐 Native integration with **AWS Shield** and **AWS WAF**

---

📍 **Edge Locations (Points of Presence)**
Edge locations are AWS global locations where CloudFront caches content.

Key ideas:
- 🧭 Hundreds of edge locations worldwide
- 🔁 Users are automatically routed to the nearest edge
- 📦 Cached content is served directly from the edge
- ⬇️ Cache miss → fetch from origin → store locally

---

🔄 **How CloudFront Works (High Level)**
Client sends a request to CloudFront:

- 🧑‍💻 Request goes to nearest edge location
- 🗂️ Edge checks local cache
- ✅ Cache hit → instant response
- ❌ Cache miss → request forwarded to origin
- 🕒 Response cached at edge with a TTL
- 🔁 Future requests served from cache

---

🗄️ **Origins (Backends)**

📦 **S3 Bucket Origin**
- Most common CloudFront origin
- Used for static file distribution
- Content cached at the edge
- ❌ S3 bucket should NOT be public
- 🔐 Secured using **Origin Access Control (OAC)**
- 🪪 Bucket policy allows access only from CloudFront

> ⭐ Recommended architecture for static websites

---

🏗️ **VPC Origin**
- Applications hosted in private subnets
- Examples:
  - ⚖️ Application Load Balancer
  - 🌐 Network Load Balancer
  - 🖥️ EC2 instances
- 🔒 CloudFront connects privately to VPC resources

---

🌐 **Custom Origin (HTTP)**
- Any public HTTP/HTTPS backend
- 🪣 Includes S3 static website endpoints  
  (must enable static website hosting)
- Can be inside or outside AWS

---

🔐 **CloudFront + S3 Secure Architecture**
- 👥 Users access CloudFront, not S3 directly
- 🔗 CloudFront retrieves content from S3 via AWS private network
- 🛡️ OAC + S3 bucket policy block public access
- 🌍 Content distributed globally via edge locations

---

🚨 **DDoS Protection**

CloudFront provides **natural DDoS protection** because traffic is distributed across global edge locations instead of hitting a single origin.

🛡️ **AWS Shield**
- Automatically enabled (Shield Standard)
- Protects:
  - CloudFront
  - Route 53
  - ALB / NLB
- Absorbs large-scale traffic attacks

🔥 **AWS WAF**
- Web Application Firewall
- Protects against application-layer attacks:
  - 🚫 IP blocking
  - 🌍 Geo-blocking
  - 🚦 Rate limiting
  - 💉 SQL injection
  - 🧪 XSS
- Attaches to:
  - CloudFront
  - ALB
  - API Gateway

> 🧠 **Exam memory phrase:**  
> **CloudFront + AWS Shield + AWS WAF = DDoS protection stack**

---

⚖️ **CloudFront vs S3 Cross-Region Replication**

🌍 **CloudFront**
- Global edge network
- 🕒 Temporary caching (TTL)
- 📁 Best for static content
- 🌎 Worldwide distribution
- 📦 Single origin bucket

---

🔁 **S3 Cross-Region Replication**
- Full bucket replication between regions
- ⏱️ Near real-time updates
- ❌ No caching
- 📖 Read-only replicas
- 🧯 Disaster recovery / regional access

---

📝 **Exam Essentials**
- 🧠 CDN = CloudFront
- 📍 Edge locations cache content close to users
- 📁 Static global content → CloudFront
- 🔐 Private S3 access → OAC
- 🚨 DDoS protection → CloudFront + Shield
- 🔍 Application-layer filtering → AWS WAF
- 🔁 Cross-region data replication → S3 CRR
