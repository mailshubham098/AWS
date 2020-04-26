> #### [Amazon CloudFront](https://docs.aws.amazon.com/AmazonCloudFront/latest/DeveloperGuide/Introduction.html)

- Amazon CloudFront is a web service that speeds up distribution of your static and dynamic web content, such as .html, .css, .js, and image files, to your users. CloudFront delivers your content through a worldwide network of data centers called edge locations. When a user requests content that you're serving with CloudFront, the user is routed to the edge location that provides the lowest latency (time delay), so that content is delivered with the best possible performance.

- You specify origin servers, like an Amazon S3 bucket or your own HTTP server etc
- You create a CloudFront distribution, which tells CloudFront which origin servers to get your files from when users request the files through your web site or application
- CloudFront assigns a domain name to your new distribution
- CloudFront sends your distribution's configuration (but not your content) to all of its edge locations—collections of servers in geographically dispersed data centers where CloudFront caches copies of your objects.
- By default, each object stays in an edge location for 24 hours before it expires. The minimum expiration time is 0 seconds; there isn't a maximum expiration time limit.

Usecases
- Accelerate Static Website Content Delivery
- Serve On-Demand or Live Streaming Video
- Encrypt Specific Fields Throughout System Processing
- Customize at the Edge
- Serve Private Content by using Lambda@Edge Customizations

- Edge cache -> Regional Cache -> Origin server
- Regional edge caches have feature parity with edge locations. For example, a cache invalidation request removes an object from both edge caches and regional edge caches before it expires. The next time a viewer requests the object, CloudFront returns to the origin to fetch the latest version of the object.
- Proxy methods PUT/POST/PATCH/OPTIONS/DELETE go directly to the origin from the edge locations and do not proxy through the Regional Edge Caches.
- Dynamic content, as determined at request time (cache-behavior configured to forward all headers), does not flow through regional edge caches, but goes directly to the origin.

- Charge for serving objects from edge locations.
- Charge for submitting data to your origin.

- The maximum size of a response body that CloudFront will return to the viewer is 20 GB. This includes chunked transfer responses that don't specify the Content-Length header value


Signed URLs & Signed cookies
- CloudFront signed URLs and signed cookies provide the same basic functionality: they allow you to control who can access your content. If you want to serve private content through CloudFront and you're trying to decide whether to use signed URLs or signed cookies, consider the following:
- Use signed URLs for the following cases:
  - You want to use an RTMP distribution. Signed cookies aren't supported for RTMP distributions.
  - You want to restrict access to individual files, for example, an installation download for your application.
  - Your users are using a client (for example, a custom HTTP client) that doesn't support cookies.

- Use signed cookies for the following cases:
  - You want to provide access to multiple restricted files, for example, all of the files for a video in HLS format or all of the files in the subscribers' area of a website.
  - You don't want to change your current URLs.


Restricting Access to Amazon S3 Content by Using an Origin Access Identity
- To restrict access to content that you serve from Amazon S3 buckets, you create CloudFront signed URLs or signed cookies to limit access to files in your Amazon S3 bucket, and then you create a special CloudFront user called an origin access identity (OAI) and associate it with your distribution. Then you configure permissions so that CloudFront can use the OAI to access and serve files to your users, but users can't use a direct URL to the S3 bucket to access a file there.
