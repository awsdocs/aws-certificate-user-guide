# ACM Console Does Not Display "Create record in Route 53" Button<a name="troubleshooting-route53"></a>

If you select Amazon Route 53 as your DNS provider, AWS Certificate Manager can interact directly with it to validation your domain ownership\. Under some circumstances, the console's **Create record in Route 53** button may not be available when you expect it\. If this happens, check for the following possible causes\.
+ You are not using Route 53 as your DNS provider\.
+ You are logged into ACM and Route 53 through different accounts\.
+ You lack IAM permissions to create records in a zone hosted by Route 53\.
+ You or someone else has already validated the domain\.
+ The domain is not publicly addressable\.