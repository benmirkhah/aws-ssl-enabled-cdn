# aws-ssl-enabled-cdn
SSL Enabled domain hosting / redirecting on AWS CDN
---------------------------------------------------

One usecase is SSL enabled static site hosting for any new domains.
Another is redirecting SSL enabled traffic from one domain to other locations.

1. Register the new domain name (ie. somedomain.com) with AWS Route53.
2. Create a new bucket for hosting static site www.somedomain.com on AWS S3. See permissions below.
3. Create a new index.html file and place it in the newly created bucket, make it public.
4. Register a new SSL certificate for the new domain name with AWS Certificate Manager.
5. Create a new CDN distribution for the new domain on AWS Cloudfront using the newly issued SSL certificate.
6. Create a DNS A record for www.somedomain.com to alias newly created Cloudfront CDN address.
7. Create a DNS Pointer record for root level somedomain.com to point to www.somedomain.com
8. Wait for CDN propagation, check if non-secure http;//somedomain.com redirects to SSL enabled https://www.somedomain.com
9. If SSL enabled domain redirection is the goal provide redirect map on the S3’s www.somedomain.com folder, see below.


S3 Bucket permissions
---------------------

{
    "Version": "2012-10-17",
    "Statement": [
        {
            "Sid": "AddPerm",
            "Effect": "Allow",
            "Principal": {
                "AWS": "*"
            },
            "Action": "s3:GetObject",
            "Resource": "arn:aws:s3:::www.somedomain.com/*"
        }
    ]
}

S3 Bucket redirecting
---------------------
Usecase: redirect all incoming requests for any path to a single page

<RoutingRules>
  <RoutingRule>
    <Condition>
      <HttpErrorCodeReturnedEquals>403</HttpErrorCodeReturnedEquals >
    </Condition>
    <Redirect>
      <Protocol>https</Protocol>
      <HostName>www.redirect.com</HostName>
      <ReplaceKeyWith>redirect/me/here/please/thank-you</ReplaceKeyWith>
      <HttpRedirectCode>302</HttpRedirectCode>
    </Redirect>
  </RoutingRule>
</RoutingRules>

