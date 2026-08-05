# File Uploads

**Status:** Active  
**Confidence:** High  
**Last review:** 2026-08-05  
**Primary consumers:** Architect, Tech Lead, Security & Quality Auditor

Upload flows, temporary and permanent storage, scanning, size limits, and access control.

## Core Flow Variants

| Variant | Description | Prefer when |
|---------|-------------|-------------|
| **Direct-to-app** | Client posts file to application server; server stores or forwards | Small files, simple apps, low volume |
| **Direct-to-storage (presigned)** | Client obtains a short-lived signed URL and uploads directly to object storage | Large files, high volume, offloading bandwidth from app servers |
| **Chunked / resumable** | File split into parts; upload can resume after interruption | Large files, unreliable networks, mobile clients |

## Preference Guidance

**Prefer direct-to-storage (presigned) when**
- Files are larger than a few megabytes or upload volume is material.
- Application servers should not be occupied with large binary transfer.
- Object storage (S3-compatible or equivalent) is already part of the architecture.

**Prefer direct-to-app when**
- Files are small, volume is low, and operational simplicity outweighs bandwidth concerns.
- Immediate server-side processing or transformation is required before storage.

**Prefer chunked / resumable when**
- Expected file size is large (video, datasets) or clients are on unstable networks.
- User experience of failed mid-upload is costly.

## Essential Decision Points

1. **Size & type limits**  
   - Hard limits must be enforced server-side (and preferably also at the storage layer).  
   - Allowed MIME types / extensions must be an allow-list, not a deny-list.

2. **Virus / malware scanning**  
   - Required for any user-generated content that will be re-served to other users or executed.  
   - Scanning can be synchronous (block until clean) or asynchronous (quarantine then promote). Document the choice and residual risk window.

3. **Storage location & access**  
   - Private by default; public only when the product explicitly requires it.  
   - Access to private objects must go through authorised, short-lived URLs or a controlled proxy.

4. **Metadata & ownership**  
   - Every uploaded object must be associated with its uploader, tenant, and purpose.  
   - Soft-delete and retention policies should be defined.

5. **Processing pipeline**  
   - Thumbnails, transcoding, text extraction, etc. should be asynchronous where possible.  
   - Failure of processing must not leave the system in an inconsistent visible state without recovery.

6. **Quota & abuse**  
   - Per-user or per-tenant storage quotas prevent unbounded cost and DoS.

## Anti-Patterns

- Accepting any file type and serving it with a user-controlled Content-Type (XSS / content-sniffing risk).
- Storing uploads on the application server filesystem in multi-instance deployments.
- Long-lived public URLs for private content.
- No size limit or an extremely high limit with no quota.
- Synchronous virus scanning on the request path for large files without timeout and UX handling.

## Recording Requirements

In the Architecture Blueprint:

- Chosen upload variant (direct-to-app / presigned / chunked)
- Size, type, and quota limits
- Scanning approach and residual risk
- Storage privacy model and access pattern
- Link to this file and the criteria used

## Related Patterns

- [forms.md](forms.md) — forms that include file fields
- [authentication.md](authentication.md) / [authorization.md](authorization.md) — who may upload and who may access
- [multi-tenancy.md](multi-tenancy.md) — tenant-scoped storage
- knowledge/technologies/ — concrete storage and scanning services
