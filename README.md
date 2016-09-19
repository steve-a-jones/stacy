# Stacy

Stacy allows creating web-sites that are served from [Amazon S3](https://aws.amazon.com/s3/) cloud service as if they are static web-sites, while having the site content managed in [Contentful](https://www.contentful.com/) CMS.

Stacy exposes an endpoint via [Amazon API Gateway](https://aws.amazon.com/api-gateway/) service, which is automatically called by a Contentful's webhook for every Entry and Asset *publish* and *unpublish* event. The endpoint then places the events into an [Amazon Kinesis](https://aws.amazon.com/kinesis/) stream, from which they are picked up by an [AWS Lambda](https://aws.amazon.com/lambda/) function. When the Lambda function receives a *publish* event for an asset (such as an image file), the asset file is uploaded into the web-site's bucket in S3. On an *unpublish* event the asset is removed from S3. When there is a *publish* for an entry, the corresponding HTML page is compiled using [Handlebars](http://handlebarsjs.com) templates, pre-compiled and packaged together with the Lambda function, and is uploaded into the web-site's bucket. When the page is *unublished*, the HTML file is removed from the bucket.
