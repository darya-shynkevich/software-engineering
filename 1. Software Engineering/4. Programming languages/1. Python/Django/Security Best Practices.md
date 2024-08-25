Django’s security features include:
1. Cross-site scripting (XSS) protection.
2. Cross-site request forgery (CSRF) protection.
3. SQL injection protection.
4. Clickjacking protection.
5. Support for TLS/HTTPS/HSTS, including secure cookies.
6. Automatic HTML escaping.
7. An expat parser hardened against XML bomb attacks.
8.  Hardened JSON, YAML, and XML serialization/deserialization tools.
9. Secure password storage, using the PBKDF2 algorithm with a SHA256 hash by default. That said, please upgrade per Section 28.29: Upgrade Password Hasher to Argon2

### Use Secure Cookies

```Python
SESSION_COOKIE_SECURE = True 
CSRF_COOKIE_SECURE = True
```

### Use HTTP Strict Transport Security (HSTS)


### Handle User-Uploaded Files Carefully

Security experts recommend the use of content delivery networks (CDNs): they serve as a place to store potentially dangerous files.

### Never Display Sequential Primary Keys

1. They inform potential rivals or hackers of your volume
2. By displaying these values we make it trivial to exploit insecure direct object references
3. We also provide targets for XSS attacks

**Solution:**
1. Lookup by Slug
2. UUIDs

To summarize: If you want to hide your sequential identifiers, don’t rely on obfus- cation.


