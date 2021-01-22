# cfn-amazon-es-fgac
Testing Amazon Elasticsearch with fine grained access control using Cloudformation

The [es.yaml](https://github.com/Castern/cfn-amazon-es-fgac/blob/main/es.yaml) file present here creates a Fine Grain Access Control (**FGAC**) enabled Amazon ES domain using the CloudFormation template. 

For FGAC we need to ensure the following three settings are set to true as [pre-requisites](https://docs.aws.amazon.com/elasticsearch-service/latest/developerguide/fgac.html#fgac-enabling) which are:
  - *[EncryptionAtRestOptions](https://docs.aws.amazon.com/AWSCloudFormation/latest/UserGuide/aws-properties-elasticsearch-domain-encryptionatrestoptions.html),* 
  - *[NodeToNodeEncryptionOptions](https://docs.aws.amazon.com/AWSCloudFormation/latest/UserGuide/aws-properties-elasticsearch-domain-nodetonodeencryptionoptions.html)* and 
  - *[HTTPS](https://docs.aws.amazon.com/AWSCloudFormation/latest/UserGuide/aws-properties-elasticsearch-domain-domainendpointoptions.html#cfn-elasticsearch-domain-domainendpointoptions-enforcehttps)*. 
  
Further, the important parameter for FGAC is **[AdvancedSecurityOptions][https://docs.aws.amazon.com/AWSCloudFormation/latest/UserGuide/aws-properties-elasticsearch-domain-advancedsecurityoptionsinput.html]** and needs to be **Enabled: true**
Now, Amazon ES provides two ways of enabling FGAC. One is through using a IAM user as a Master-user and other is through having a basic auth. 
 - If you need to take the IAM way then set the *[InternalUserDatabaseEnabled](https://docs.aws.amazon.com/AWSCloudFormation/latest/UserGuide/aws-properties-elasticsearch-domain-advancedsecurityoptionsinput.html#cfn-elasticsearch-domain-advancedsecurityoptionsinput-internaluserdatabaseenabled)* to *false*  and only have the parameter *[MasterUserARN: "IAM User ARN"](https://docs.aws.amazon.com/AWSCloudFormation/latest/UserGuide/aws-properties-elasticsearch-domain-masteruseroptions.html#cfn-elasticsearch-domain-masteruseroptions-masteruserarn)* under the *[MasterUserOptions](https://docs.aws.amazon.com/AWSCloudFormation/latest/UserGuide/aws-properties-elasticsearch-domain-masteruseroptions.html)* field. I have commented the same in the [es.yaml](https://github.com/Castern/cfn-amazon-es-fgac/blob/main/es.yaml) file since I use the basic auth.
 
 - If you need to take the basic auth (username and password) approach set the *[InternalUserDatabaseEnabled](https://docs.aws.amazon.com/AWSCloudFormation/latest/UserGuide/aws-properties-elasticsearch-domain-advancedsecurityoptionsinput.html#cfn-elasticsearch-domain-advancedsecurityoptionsinput-internaluserdatabaseenabled)* to *true* and have the *[MasterUserName: "any-name"](https://docs.aws.amazon.com/AWSCloudFormation/latest/UserGuide/aws-properties-elasticsearch-domain-masteruseroptions.html#cfn-elasticsearch-domain-masteruseroptions-masterusername)* and the *[MasterUserPassword: "xxx"](https://docs.aws.amazon.com/AWSCloudFormation/latest/UserGuide/aws-properties-elasticsearch-domain-masteruseroptions.html#cfn-elasticsearch-domain-masteruseroptions-masteruserpassword)* Please have at least one lower case, one upper case, one digit and one special character for the password else the CFN template will rollback. However, the failure message is easily seen on the CFN console under events. 
 
 
You may create an IAM user to trigger this CFN template. Make sure the IAM user has permission over creating ES and CFN resources. Further, configure your AWS CLI to use this IAM user and then use the following command to run the es.yaml through cloudformation that will spin up a Amazon ES FGAC enabled domain for you. 

*aws --region us-west-2 cloudformation create-stack --stack-name give-any-stack-name-here --template-body file://es.yaml *

Please note the --template-body argument must be specified as a file URI else it returns unsupported structure/format error. 
 
