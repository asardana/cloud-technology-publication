In my earlier posts on Cloud Storage and AWS S3, we discussed different storage types offered by the cloud providers and the Simple Storage Service (S3) by Amazon for storing the objects in the cloud. In this post, we’ll look at how to manage the access to buckets and objects stored in the AWS S3.

Buckets and objects created in AWS S3 are private by default with read and write access granted only to the owner who created the resources. Resource owner can grant access permission to other entities by creating policies that could be attached to buckets, objects or users.

Depending upon whether the policy is attached to a user or a resource, policy options can be broadly categorized as either the user based policies or resource based policies.

Resource Based Policy
Resource based policies are applied to buckets and the objects contained within the bucket. There are 2 types of resource based policy – Bucket Policy and S3 Access Control List (ACL)

S3 Bucket Policy

S3 bucket policies are attached to buckets only and determine which principals are allowed or denied the actions on the bucket. The permissions granted via. the bucket policy also applies to all the objects within the bucket.

Bucket Policy includes following information-

Version – Version element specifies the policy language version. The most recent version that is currently available for use is 2012-10-17.
Statement.Effect – Effect element under statement specifies whether the statement will result in an allow or deny.
Statement.Principal – Principal element under statement specifies the user entity to which the policy applies.
Statement.Action – Action element under statement specifies what set of actions are allowed or denied on the S3 resource.
Statement.Resource – Resource element under statement specifies the S3 resource to which the policy statement applies.
S3 Bucket policies provides a simple way to manage access to bucket allowing cross account access without the IAM roles. S3 supports bucket policy of up to 20 kb. Bucket policy provides a better visibility in knowing who can access the specific S3 bucket to which the policy is attached.

S3 ACL

S3 ACL is a sub-resource that’s attached to every S3 bucket and object granting full access to the owner who created the resource as a default ACL policy. ACL policy identifies which users and groups are granted access and the type of access.

S3 ACL allows to manage the access at both the bucket and object level. ACL policy is the only way to manage S3 access at the object level. ACL is also suitable in those cases where the bucket policy or IAM policy grows large and exceeds the size limits imposed by them.

User Policy (IAM Policy)
User policies are applied via. IAM to users, groups, or roles to specify what actions are allowed or denied on what S3 resources. User policy is similar to bucket policy specifying the statements that identify the S3 resources and the corresponding actions that are allowed or denied to the users to which is policy is applied. Unlike bucket policy which has a principal defined in the policy itself, there is no principal defined in the user policy since the principal is the user, group or role to which the policy is attached.

IAM user policy is better suited for use cases where you want better visibility into what a specific user is allowed to do in the AWS environment including the S3 resources. It is also easier to manage the S3 access when there are a lot of buckets and its hard to manage the access by defining bucket policy for each S3 bucket.

IAM policy has a limit of 2kb for users, 5kb for groups and 10 kb for roles.

S3 Authorization with multiple policies
Whenever an AWS principal issues a request to S3, the authorization decision depends on the union of all the bucket policies, S3 ACL policies and IAM policies. The decision logic follows the principle of least-privilege by defaulting the resource access to deny. Deny always supersedes Allow effect.

Share this:
