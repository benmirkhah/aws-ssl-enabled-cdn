# aws-ssl-enabled-cdn
SSL Enabled domain hosting / redirecting on AWS CDN
---------------------------------------------------

One usecase is SSL enabled static site hosting for any new domains.
Another is redirecting SSL enabled traffic from one domain to other locations.

1. Register the new domain name (ie. somedomain.com) with AWS Route53.
2. Create a new bucket for hosting static site www.somedomain.com on AWS S3.
3. Create a new index.html file and place it in the newly created bucket, make file public.
4. Register a new SSL cert for the new domain name with AWS Certificate Manager.
5. Create a new CDN distribution for the new domain on AWS Cloudfront using the newly issues SSL cert.
6. Create a DNS A record for www.somedomain.com to alias the new Cloudfront CDN url.
7. Create a DNS Pointer record for root level somedomain.com and point it to www.somedomain.com
8. Wait for the network propagation 
9. Check to see if typing non secure http;//somedomain.com redirects you to the SSN enabled https://www.somedomain.com
10. If SSL enabled domain redirection is the goal provide redirect map on the S3 www.somedomain.com folder
