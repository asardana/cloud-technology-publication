Identity and Access Management (IAM) is widely used in most of the enterprises to authenticate and authorize the users to grant access to applications and systems that supports various functions within the organization. It is one of the basic components when it comes to enterprise security and defense in-depth principles that organizations adopt to protect its assets and customer data.

There has been an exponential rise in the cloud computing adoption in recent years including the financial services companies which traditionally have been quite conservative in moving the data and computing outside its own data centers. There are various compliance and regulatory implications that needs to be addressed before an organization makes a move to cloud computing services provided outside its own data centers.

At a very high level, AWS IAM is a web service that helps in providing secure access to all the services in the cloud . It helps in ensuring that only the known and authorized users are able to access the computing resources and the data stored in the cloud.

Let’s look at some of the basic IAM terminology and concepts that helps in allowing secure access to users and resources within the AWS cloud.

Users
Users refers to the individuals within the organizations or entity requiring access to resources like EC2, S3 and DynamoDB. User Id maps to each individual person or entity to provide an identity before granting access to various resources within AWS. Users can have security credentials (user id and password) to access AWS management console and/or access key to allow programmatic access to various AWS services.

Groups
Group is a logical collection of users such that access policies can be applied to the group than at the user level. There is a many-many relationship between the group and users i.e. a group can have many users and a user can belong to many groups. Group is not an identity of the user and only helps in assigning common permissions to multiple users belonging to the same group.

Role
Role is a collection of policies that could be assumed by a user or a service to get permissions to other AWS resources.

Role does not have any credentials assigned to it, however AWS will generate temporary credentials when an authenticated user assumes a specific role  for which it has been granted a permission. IAM role provides delegated access to users and resources allowing them to use other AWS resources temporarily. It also helps in granting cross account access to users created within one AWS account and might need an access to resources available in a different AWS account.

Permissions and Policies
Permissions can be assigned to users, groups and the roles created within the AWS account. It limits the tasks that can be performed on various AWS resources and hence providing a very fine-grained control over the resource access and the actions that can be performed. IAM follows the principle of least privileges i.e. no resource permissions are granted by default.

Permissions are defined in a JSON format called as policy document.Policy document has a version and one or more statements defining the permissions. Some of the common attributes defined with a statement are – action/operation allowed, resource details using amazon resource name (arn), effect to either allow or deny access to resources and the Principal defining who is allowed access to other AWS resources defined in the policy.

Important IAM Concepts
IAM does not apply to a specific region within AWS i.e. IAM is global
Root user is a default user which is created with a new account and has the complete administrative access. It’s a best practice to set-up MFA (multi factor authentication) on the root account.
Users can have both the access key id/access key and user id/password created for specific use within the IAM realm. Access key Id cannot be used to access AWS console. It’s meant to be used for AWS resource access via CLI or APIs.
IAM provides Active Directory Federation to the users who are first authenticated by existing Active Directory infrastructure within the enterprise. Once authenticated user’s browser posts the authentication response to the AWS SAML endpoint which then uses AssumeRoleWithSAML API to create temporary security credentials. Finally the user is redirected to the signed-in AWS management console.
IAM provides Web Identity Federation for the applications to first authenticate with the identity providers like Facebook, Google etc. After the authentication is successful,  identity providers return an access token which is then used to get the temporary security credentials (access key id and security access key) by making a call to AssumeRoleWithWebIdentity.
