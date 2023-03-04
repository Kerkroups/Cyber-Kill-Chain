## AWS IAM POLICY  
**Get the Profile of Current Logged in user**: ```aws sts get-caller-identity```  

**List of IAM Users**: ```aws iam list-users```  

**List the IAM groups that the specified IAM user belongs to**: ```aws iam list-groups-for-user --user-name user-name```  

**List all manages policies that are attached to the specified IAM user**: ```aws iam list-attached-user-policies --user-name user-name```  

**Lists the names of the inline policies embedded in the specified IAM user**: ```aws iam list-user-policies --user-name user-name```  

