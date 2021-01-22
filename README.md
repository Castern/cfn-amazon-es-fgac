# cfn-amazon-es-fgac
Testing Amazon Elasticsearch with fine grained access control using Cloudformation

The [es.yaml] (https://github.com/Castern/cfn-amazon-es-fgac/blob/main/es.yaml) file present here creates a Fine Grain Access Control(FGAC) enabled Amazon ES domain using the CloudFormation template. 

For FGAC we need to ensure the following three settings are set to true as [pre-requisites] (https://docs.aws.amazon.com/elasticsearch-service/latest/developerguide/fgac.html#fgac-enabling) which are:
  *EncryptionAtRestOptions,* 
  *NodeToNodeEncryptionOptions* and 
  *HTTPS*. 
  
Further, the important parameter for FGAC is **(AdvancedSecurityOptions) [https://docs.aws.amazon.com/AWSCloudFormation/latest/UserGuide/aws-properties-elasticsearch-domain-advancedsecurityoptionsinput.html]** and needs to be **Enabled: true**
Now, Amazon ES provides two ways of enabling FGAC. One is through using a IAM user as a Master-user and other is through having a basic auth. 
 - If you need to take the IAM way then set the *InternalUserDatabaseEnabled* to *false*  and only have the parameter *MasterUserARN: "IAM User ARN"* under the *MasterUserOptions* field. I have commented the same in the [es.yaml] (https://github.com/Castern/cfn-amazon-es-fgac/blob/main/es.yaml) file. 
 
 - If you need to take the basic auth (username and password) approach set the *InternalUserDatabaseEnabled* to *true* and have the *MasterUserName: "any-name"* and the *MasterUserPassword: "xxx"* Please have at least one lower case, one upper case, one digit and one special character for the password else the CFN template will rollback. However, the failure message is easily seen on the CFN console under events. 
 
