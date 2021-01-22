# cfn-amazon-es-fgac
Testing Amazon Elasticsearch with fine grained access control using Cloudformation

The es.yaml file present here creates a Fine Grain Access Control(FGAC) enabled Amazon ES domain using the CloudFormation template. For FGAC we need to ensure the three settings are set to true as pre-requisites which are EncryptionAtRestOptions, NodeToNodeEncryptionOptions and HTTPS. Further, the important parameter for FGAC is AdvancedSecurityOptions which contains InternalUserDatabaseEnabled set to true and MasterUserOptions. MasterUserOptions can either be an IAM ARN or a internal-user as in the template. Internal user needs a username and password whereas if the master-user is an IAM ARN you need to specify the ARN of the IAM user acting as the master.  
