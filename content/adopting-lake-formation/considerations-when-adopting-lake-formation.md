# Considerations when adopting Lake Formation

[AWS Lake Formation](https://aws.amazon.com/lake-formation/) centralizes permissions management of your data and makes it easier to share data across your organization and with external customers. With the AWS Glue Data Catalog as the central point of metadata for data in AWS data sources such as Amazon S3, you can manage fine-grained data lake access permissions using familiar database-like features of Lake Formation. Lake Formation manages the metadata permissions of tables in Data Catalog and the corresponding data permissions for S3 in one place. When you set up permissions for users, you define Grant/Revoke style policies for the databases and tables and register the data location with Lake Formation.  Further, Lake Formation provides LF-Tags to scale permissions policies and provides CloudTrail logging for auditing and reports. 

We recommend you to consider the below process while you are evaluating and choosing to adopt Lake Formation. They are:

1. Take inventory of your data stores. Lake Formation provides data governance for data on S3, while extending it to Amazon Redshift data shares and extending the Data Catalog to federate into external hive metastores.
2. Review your current data access management model. What is your current data access model? Is it Apache Ranger, a combination of S3 Bucket Policies, IAM policies and Glue Resource policies?
3. Review what analytics engines and AWS services you use for your data analysis and ETL pipelines. Validate that your query engines are supported with the granularity of permissions (example- table level support, column level, or row level, support for Glue Views) that you need. AWS analytics services that integrate with Lake Formation are detailed in [this documentation page](https://docs.aws.amazon.com/lake-formation/latest/dg/service-integrations.html). 
4. Are you using Open source transactional table formats such as Apache Iceberg, Apache Hudi or Linux Foundation’s Deltalake? You can look at what levels of Lake Formation fine grained permissions are supported for each table format by each integrated analytics service in [our documentation](https://docs.aws.amazon.com/lake-formation/latest/dg/working-with-services.html). 
5. What permissions model suits your data platform?  Do you want to keep your existing model or do you want to review Lake formation? Many customers who choose Lake Formation do so because they want to adopt permissions based on tagging (that is,  LF-Tags ) or want to adopt a model that is more centralized with the metadata in Glue Data Catalog or want to integrate tightly with AWS analytics services. You could continue to use your existing permissions model if it provides constructs and policy mechanisms not yet supported by Lake Formation (for example, Attribute Based Access Control). 
6. What is your account topology?  You may consider splitting monolothic AWS accounts to smaller accounts and leverage Lake Formation’s cross account sharing capabilities. This provides benefits like reducing single points of failures, allows teams or organizations to own and manage their data processing infrastructure, and scale their usage of AWS services.
7. Do you want to apply Lake Formation permissions to all resources, a subset of resources, all users, or a subset of users. Please see [Lake Formation Adoption Modes](lake-formation-adoption-modes.md)
8. Lastly, if you are moving from a different authorization engine, how are you model the permissions to your users? Lake Formation can create policies against AWS IAM users and roles, AWS QuickSight Authors, and AWS Identity Center users and groups. If you need identity based policies, then you will need to integrate with AWS Identity Center using a feature called [Trusted Identity Propogation](https://docs.aws.amazon.com/singlesignon/latest/userguide/trustedidentitypropagation-overview.html). At the time of writing this, not all AWS services support Identity center so please refer to the [public docs](https://docs.aws.amazon.com/singlesignon/latest/userguide/trustedidentitypropagation-integrations.html). 


Once all of these points have been considered, the general process to adopt Lake Formation can be found in detail in the [Lake Formation public documentation](https://docs.aws.amazon.com/lake-formation/latest/dg/upgrade-glue-lake-formation.html). 