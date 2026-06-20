problem link: https://tryhackme.com/room/surfer
# Surfer - TryHackMe Writeup

## Room Information

* Platform: TryHackMe
* Room: Surfer
* Difficulty: Easy
* Topic: Server-Side Request Forgery (SSRF)

## Objective

Access an internal webpage and retrieve the flag.

## Enumeration

After opening the target website, I suspected there might be a web vulnerability, so I began intercepting requests using Burp Suite.

I logged into the application using the credentials:

Username: admin

Password: admin

After logging in, I discovered a message mentioning an internal page:

```text
/internal/admin.php
```

Attempting to access the page directly resulted in an error indicating that the page was only accessible locally.

## Identifying the Vulnerability

While exploring the application, I found an "Export PDF" feature.

After generating a PDF, I noticed the following message:

```text
Report generated for http://127.0.0.1/server-info.php
```

This suggested that the application was fetching URLs on the server side.

## Exploitation

I intercepted the Export PDF request using Burp Suite and sent it to Repeater.

Inside the request, I found a URL parameter containing:

```text
http://127.0.0.1/server-info.php
```

The value was URL encoded, so I decoded it and modified it to:

```text
http://127.0.0.1/internal/admin.php
```

I then resent the request.

Because the server itself made the request, it was able to access the internal page that was unavailable to external users.

## Flag Retrieval

The modified request successfully generated a PDF containing the contents of `/internal/admin.php`.

Opening the generated PDF revealed the flag, completing the room.

## Key Takeaways

* Learned how Server-Side Request Forgery (SSRF) works.
* Used Burp Suite Repeater to modify requests.
* Identified an internal-only resource.
* Leveraged SSRF to access localhost resources and obtain the flag.
