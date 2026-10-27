# Week 9 — Custom Domain with Route 53 and HTTPS with ACM

**Topic:** Domain Management with Route 53 and SSL/TLS Certificate Handling with AWS Certificate Manager
**Original session date:** August 2, 2025

## Goals
- [x] Register a domain and connect it to AWS Route 53 by updating its nameservers
- [x] Create a Route 53 hosted zone for the domain
- [x] Serve the S3 static website through a CloudFront distribution
- [x] Request a free SSL/TLS certificate in AWS Certificate Manager (ACM) and validate it with DNS in Route 53
- [x] Attach the certificate and custom domain to CloudFront
- [x] Confirm the site loads over HTTPS using the custom domain

## What I did
The task sheet suggests registering a free domain on Freenom, but Freenom
no longer offers free domains, so I registered a low-cost domain with
**Namecheap** instead. (www.flytospace.online)

1. **Route 53 hosted zone:** created a public hosted zone for the domain.
   Route 53 generated four nameservers in the zone's NS record.
2. **Nameservers:** in Namecheap, switched the domain from the default
   DNS to **Custom DNS** and entered the four Route 53 nameservers, so
   Route 53 became responsible for the domain's DNS records.
3. **CloudFront:** used the CloudFront distribution that serves my S3
   static website (from Week 8) and added my domain as an alternate
   domain name.
4. **ACM certificate:** requested a public certificate in **us-east-1
   (N. Virginia)**, the region CloudFront requires, and chose **DNS
   validation**. ACM gave me a CNAME record to prove I control the
   domain, which I created in my hosted zone using the "Create records
   in Route 53" button. The certificate status changed to **Issued**.
5. **Attach the certificate:** selected the issued certificate on the
   CloudFront distribution.
6. **DNS records:** created **alias A records** in Route 53 pointing the
   domain names to the CloudFront distribution.
7. **Test:** opened the site using my custom domain and confirmed it
   loaded over **HTTPS** with a valid certificate.

| Setting | Value used |
|---------|-----------|
| Registrar | Namecheap |
| Domain | (www.flytospace.online) |
| Certificate region | us-east-1 (N. Virginia) |
| Certificate domain names | (flytospace.online) |
| Validation method | DNS (CNAME record in Route 53) |

## Issues encountered
- **Freenom no longer available.** The task refers to a free Freenom
  domain, but the service no longer gives them out, so I used Namecheap
  and paid for a domain instead.
- **The root domain failed while `www` worked.** The `www` version of
  the domain loaded correctly, but the root domain (without `www`)
  showed `DNS_PROBE_FINISHED_NXDOMAIN` in the browser, which means the
  name could not be resolved. I checked:
  - the root domain had its own alias A record, since the record for
    `www` does not cover the root name
  - the record was correct, by querying one of my Route 53 nameservers
    directly with `nslookup`, which returned IP addresses
  - the problem was not in my browser, because clearing the browser and
    DNS caches and testing on mobile data made no difference
  Because Route 53 was returning the right answer, I concluded the
  failure was an old "domain does not exist" result cached by DNS
  resolvers from before the record existed.

## Key takeaways
- A Route 53 **hosted zone** holds the DNS records for a domain, but it
  only takes effect once the registrar's nameservers point to the four
  nameservers Route 53 gives you.
- ACM public certificates are free. Certificates used with CloudFront
  must be requested in **us-east-1**, whatever region the rest of the
  resources are in.
- **DNS validation** works by adding a CNAME record to the domain, and
  because the domain is already in Route 53, ACM can create it for you.
- An **alias A record** is how a domain points at CloudFront. A root
  domain can't use a CNAME, and `www.domain.com` and `domain.com` are
  separate names that each need their own record.
- The domain and the certificate both need to cover the names you want
  visitors to use, and CloudFront needs each name listed as an
  alternate domain name.
- DNS answers are cached, including "does not exist" answers, so a
  change can look broken for a while. Querying a nameserver directly
  with `nslookup` shows what the authoritative answer is.

## Cleanup
After completing and documenting this task, I removed the resources I
created to avoid unexpected charges, since Route 53 hosted zones are
not part of the Free Tier. Resources removed: 
(Route 53 hosted zone, the CloudFront distribution, the S3 bucket and
its files, and the ACM certificate). Turn-off auto-renew on Namecheap

## Screenshots / Evidence
**Route 53 hosted zone with its records**
<img width="1897" height="868" alt="Screenshot 2026-10-27 082934" src="https://github.com/user-attachments/assets/d39d0f4b-688a-417b-b82e-eeb71dabf422" />

**Namecheap custom DNS pointing to the Route 53 nameservers**
<img width="1882" height="967" alt="Screenshot 2026-10-27 083213" src="https://github.com/user-attachments/assets/20f45ac4-9755-4d96-99c6-972aad2c16a7" />

**Validated SSL certificate in AWS Certificate Manager (status: Issued)**
<img width="1876" height="656" alt="Screenshot 2026-10-27 083325" src="https://github.com/user-attachments/assets/c30e9a8d-eed3-452e-9ea8-ccbb585fde26" />

**CloudFront settings with the custom domain and certificate**
<img width="1890" height="812" alt="Screenshot 2026-10-27 083420" src="https://github.com/user-attachments/assets/08dbed9b-d00a-426a-92d3-b355798c4c71" />

**Website loading over HTTPS on the custom domain**
<img width="1907" height="961" alt="Screenshot 2026-10-27 072532" src="https://github.com/user-attachments/assets/3043ba9c-4952-402c-a3c3-c0c46cfca7c1" />
