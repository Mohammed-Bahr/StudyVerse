---
created: 2026-04-10 17:19
tags:
  - DevOps
  - Docker
  - DockerHub
  - Registry
source: Docker Docs, GeeksforGeeks
---

# Docker: Docker Hub & Registry

## What is Docker Hub?

**Docker Hub** is a cloud-based repository service where users can:
- **Push** their container images
- **Pull** container images from others
- Find and share container images anytime, anywhere via the internet

![](https://media.geeksforgeeks.org/wp-content/uploads/20230419170724/Docker-hub-registry.webp)

---

## Docker Hub Image Categories

Docker Hub organizes images into categories based on trust levels and curation:

### 1. Docker Official Images

| Aspect | Details |
|--------|---------|
| **What** | Curated and maintained by Docker and official upstream sources |
| **Badge** | Special "Official" badge on Docker Hub |
| **Examples** | `ubuntu`, `nginx`, `python`, `mysql`, `node`, `alpine` |

**Characteristics:**
- Follow Dockerfile best practices
- Regularly updated and security-patched
- Clear documentation
- Minimal CVEs (security vulnerabilities)

**Best for:** Production use, base images for your applications

---

### 2. Docker Verified Publisher Images

| Aspect | Details |
|--------|---------|
| **What** | High-quality images from commercial software vendors verified by Docker |
| **Badge** | "Verified Publisher" badge |
| **Examples** | Enterprise software vendors (Microsoft, Oracle, etc.) |

**Characteristics:**
- Publisher identity verified by Docker
- Enterprise-grade infrastructure (99.9% uptime)
- Exempt from Docker Hub rate limits
- Security scanning with Docker Scout
- Priority search ranking

**Best for:** Enterprise software, trusted commercial distributions

---

### 3. Docker-Sponsored Open Source (OSS) Images

| Aspect | Details |
|--------|---------|
| **What** | Images from open-source projects sponsored by Docker |
| **Badge** | Special OSS badge |

**Characteristics:**
- Maintained by active open-source communities
- Verified as trusted and secure

**Best for:** Open-source tools and frameworks

---

### 4. Community Images

| Aspect | Details |
|--------|---------|
| **What** | Images uploaded by individual users or unverified organizations |
| **Badge** | None |

**Characteristics:**
- No verification or curation
- Variable quality and security
- May be outdated or contain vulnerabilities

**Best for:** Testing only; **use with caution in production**

---

> [!warning] Security Note
> Always prefer **Official** or **Verified Publisher** images for production. Community images may contain security vulnerabilities or malware.

---

## Summary Table: Trust Levels

| Category | Badge | CLI Filter | Trust Level |
|----------|-------|-------------|--------------|
| Official Images | Official | `--filter is-official=true` | ⭐⭐⭐⭐⭐ Highest |
| Verified Publisher | Verified | No CLI filter | ⭐⭐⭐⭐⭐ High |
| Sponsored OSS | OSS | No CLI filter | ⭐⭐⭐⭐ High |
| Community | None | Default search | ⭐⭐ Variable |

---

## Docker Hardened Images (DHI)

**Docker Hardened Images (DHI)** are enterprise-grade Docker images provided by Docker as part of a **paid subscription**.

They are designed to be:
- **More secure**
- **More minimal**
- **Compliant with strict security standards**

Compared to normal Docker images, DHI images:
- Contain fewer packages
- Have a smaller attack surface
- Are regularly scanned and patched
- Are suitable for production and regulated environments

---

### Types of Docker Hardened Images

#### 1. Development (Dev) Images

| Aspect | Details |
|--------|---------|
| **Purpose** | Used during development and build stages |
| **Includes** | Build tools, shells, package managers, debugging tools |

**When to use:**
- Local development
- CI/CD pipelines
- Building and testing applications

> [!warning] Not for Production
> Dev images contain many tools that increase security risk. Don't use in production.

---

#### 2. Runtime Images

| Aspect | Details |
|--------|---------|
| **Purpose** | Used in production environments |
| **Includes** | Only what is required to run the application |
| **Excludes** | No compilers, no package managers, often no shell |

**Why this matters:**
- Smaller image size
- Faster startup
- Much lower attack surface
- If attacker gets inside, very few tools to exploit

**Example:**

```dockerfile
# Instead of:
FROM node:18

# Use:
FROM docker.io/docker/dhi-node:18-runtime
```

---

#### 3. FIPS Variants

| Aspect | Details |
|--------|---------|
| **Purpose** | Environments requiring strict cryptographic compliance |
| **FIPS** | Federal Information Processing Standards (U.S. government security standard) |

**Who needs this:**
- Banks and financial institutions
- Government systems
- Military or defense contractors

If your organization requires **FIPS 140 compliance**, use FIPS-enabled images.

---

#### 4. STIG-Ready Images

| Aspect | Details |
|--------|---------|
| **Purpose** | Government and highly regulated systems |
| **STIG** | Security Technical Implementation Guide |

**Characteristics:**
- Pre-configured to pass security audits
- Align with government and defense security rules
- Reduce effort for compliance scanning

**Typical users:** Government agencies, defense systems, highly regulated enterprises

---

### Quick Comparison: DHI Types

| Type | Use Case | Tools Included | Security Level |
|------|----------|----------------|----------------|
| Dev | Development / CI | Many | Medium |
| Runtime | Production | Minimal | High |
| FIPS | Crypto-regulated systems | Minimal | Very High |
| STIG | Government / Military | Extremely minimal | Maximum |

---

### Do You Need Docker Hardened Images?

**❌ Probably NOT if you are:**
- Learning Docker
- Building personal projects
- Working in small startups

**✅ You SHOULD consider them if:**
- You work in a large enterprise
- You handle sensitive data
- You must meet legal or security compliance requirements

---

## Filtering Images from Docker CLI

The `docker search` command allows filtering Docker Hub images directly from the command line.

### Available Filters

| Filter | Type | Description |
|--------|------|-------------|
| `stars` | int | Minimum number of stars (popularity rating) |
| `is-official` | boolean | `true` = Official images only |
| `is-automated` | boolean | `true` = Automated builds (deprecated) |

---

### Practical Examples

#### Filter by Official Status Only

```bash
docker search ubuntu --filter "is-official=true"
```

Returns only Docker Official Images for Ubuntu.

---

#### Filter by Star Rating

```bash
docker search nginx --filter "stars=100"
```

Returns images with 100 or more stars.

---

#### Combine Multiple Filters (AND Logic)

```bash
docker search python --filter "is-official=true" --filter "stars=1000"
```

Returns official Python images with at least 1000 stars.

---

#### Limit Results

```bash
docker search mysql --limit 5
```

Returns only the top 5 results.

---

#### Show Full Descriptions

```bash
docker search redis --no-trunc
```

Prevents truncation of image descriptions.

---

#### Custom Format Output

```bash
docker search node --format "table {{.Name}}\t{{.StarCount}}\t{{.IsOfficial}}"
```

Displays results in a custom table format.

---

## Choosing the Right Image

When selecting an image, consider:

### 1. Is it stable?
- Use Official or Verified Publisher images
- Check last update date
- Review download count and stars

### 2. Does the size fit your needs?

| Image | Size | Use Case |
|-------|------|----------|
| `ubuntu:22.04` | ~77MB | General purpose |
| `alpine` | ~5MB | Minimal, production |
| `node:18` | ~900MB | Full Node.js environment |
| `node:18-alpine` | ~170MB | Minimal Node.js |

### 3. Do you need all the features?
- Use `-alpine` variants for smaller images
- Use `slim` variants for moderately reduced size
- Use DHI for enterprise compliance

---

## Key Takeaways

- **Docker Hub** = cloud registry for finding and sharing images
- Use **Official Images** or **Verified Publisher** for production
- **Community images** are fine for testing but risky for production
- **DHI (Docker Hardened Images)** are for enterprise and compliance
- Always check image stability, size, and last update date
- Use `docker search --filter is-official=true` to find trustworthy images

---

## Related Notes

- [[Docker 1 - Introduction & Architecture]]
- [[Docker 2 - Images & Dockerfiles]]
- [[Docker 3 - Containers & CLI]]
- [[Docker 7 - Networking]]
- [[CI-CD]] — Automated image builds and pushes
- [[Security Best Practices]]