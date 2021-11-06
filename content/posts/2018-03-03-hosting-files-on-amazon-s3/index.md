+++
title= "Hosting Media or Files on Amazon S3"
date = 2018-03-03
tags = [ "aws", "hosting" ]
+++

As I've mentioned in previous posts, I'm a big fan of [Jekyll](https://jekyllrb.com/) for creating static websites. The one obvious shortcoming of Jekyll is the entire site must be regenerated when adding even a single post. Lately I've been experimenting with [Micro.blog](http://micro.blog) for personal posts not appropriate here. It has an iOS app for making quick updates from your phone, and hooks into Twitter and Facebook for automatic cross-posting to your social feeds. Yes, I could post directly on Twitter or Facebook, but I like hosting my content independently. That said, [Micro.blog](http://micro.blog) does have one shortcoming in my use. To post anything more than just a simple photo, it must be hosted elsewhere and linked. Of course, I could post my media on any number of free services available, but I want to ensure the links don't break if their URL scheme changes, or they happen to go under. For this reason, I decided to host my files on Amazon's S3 service. 

During my research, I found many posts out there on hosting content using Amazon S3, but none had a process that worked specifically for me. I've documented here the steps I followed, mainly for my benefit, but I'm hoping you will also find it useful.

First off, to use Amazon Web Services, you need an Amazon account. Assuming most everyone has an account, I'm not covering those steps here. Nonetheless, if you don't have an account, you need to set one up before going further.

1. Go to the [Amazon Web Services](https://aws.amazon.com) page and sign into the AWS console using your account[^1].
2. Once logged in, under __Services__, select __S3__.
3. On the __S3__ page, click on the button labelled __Create Bucket__.

   {{< image src="AWS_S3_Create_Bucket.png" alt="AWS S3 Create Bucket" position="left" style="border-radius: 4px;" >}}

4. Enter a bucket name and select an appropriate region for hosting, then click __Next__. In this example, I have used `files.bftsystems.ca.`  Any name could do, but this makes it easy for me to remember what URL links to this bucket.

   {{< image src="AWS_S3_Bucket_Name.png" alt="AWS S3 Bucket Name" position="left" style="border-radius: 4px; width: 75%;" >}}

5. You will be prompted for properties to associate with this bucket. As this will be used for simple hosting of public files, no special properties are needed. Just click __Next__ to accept the defaults.
6. You will now be prompted to select permissions for this bucket. The only setting to be changed are the public permissions. Change this to __Grant public read access to this bucket__ and click __Next__.

   {{< image src="AWS_S3_Public_Permissions.png" alt="AWS S3 Public Permissions" position="left" style="border-radius: 4px; width: 75%;" >}}

7. Lastly, a summary of the bucket settings will be shown. Confirm everything, then click __Create bucket__.
8. Once the bucket is created, it will appear on the S3 page. Click the bucket name in the list to open it.

   {{< image src="AWS_S3_Bucket_List.png" alt="AWS S3 Bucket List" position="left" style="border-radius: 4px; width: 75%;" >}}

9. When the bucket opens, click the __Upload__ button to begin adding files. There are many tools available to simplify uploading of files to S3[^2], but for the purposes of this tutorial, I'll cover the steps to upload using the website.

   {{< image src="AWS_S3_Upload.png" alt="AWS S3 Upload" position="left" style="border-radius: 4px;" >}}

10. When prompted, click the __Add files__ link to open a file browser and select the files to upload, or drag and drop the files to the browser window. Once all the files to upload are in the list, click __Next__ to begin the upload process.
11. Once again, when prompted for permissions, select __Grant public read access to these object(s)__ to allow access from the web.
12. As no special properties are required, you can safely click the __Upload__ button at this point to skip the remaining steps.
13. When the upload completes, the files will appear in the bucket list (pun intended), but they are not yet publicly accessible. To enable this, select the files, then click the __Make public__ option under the __More__ button. You will see a warning the objects will now be made public. Click the __Make public__ button to proceed.

    {{< image src="AWS_S3_Make_Public.png" alt="AWS S3 Make Public" position="left" style="border-radius: 4px;" >}}

Once the steps above are completed, all the files uploaded will be accessible using the Amazon S3 URL at __files.bftsystems.ca.s3.amazonaws.com__. That works, but I would prefer the URL be a sub-domain of my site. This can be done by adding a __CNAME__ record for my domain pointing to the URL of the S3 bucket. How this is done will vary depending on your web host, but here is the screenshot from my web host console.

   {{< image src="AWS_Create_CNAME_Record.png" alt="AWS Create CNAME Record" position="left" style="border-radius: 4px; width: 75%; margin-left: 2em;" >}}

Once this DNS record has been added, all the files I uploaded can now be linked at the URL __files.bftsystems.ca__, totally transparent to my readers.

I have one last recommendation. Although Amazon S3 rates are exceedingly low, they do charge by usage[^3]. To avoid web crawlers indexing my S3 buckets and racking up charges, I always add a `robots.txt` file with the following text to ensure the bucket is not indexed (at least by web spiders that follow the rules).

    User-agent: *
    Disallow: /

I hope you found this post useful, or at least interesting. If you would like to contact me or provide feedback, use the [Contact](/contact) page, or message me on [Twitter](http://twitter.com/bftsystems).

[^1]: As a reminder, your Amazon credentials will have full root access to all your AWS resources, so use a strong password. I also recommend using two-factor authentication for added security. See the steps [here](https://docs.aws.amazon.com/IAM/latest/UserGuide/id_credentials_mfa_enable_virtual.html) to enable Multi-factor authentication in AWS.
[^2]: I personally recommend [Transmit](https://www.panic.com/transmit) by Panic. It is available for both macOS and iOS.
[^3]: As Amazon S3 is a usage based service, I also enable Billing Alerts and PDF invoices by email to keep a close eye on usage. See the steps [here](https://docs.aws.amazon.com/AmazonCloudWatch/latest/monitoring/monitor_estimated_charges_with_cloudwatch.html) to enable these features.