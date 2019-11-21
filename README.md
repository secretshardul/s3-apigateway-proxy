# s3-apigateway-proxy
Simplified explaination of [AWS tutorial](https://docs.aws.amazon.com/apigateway/latest/developerguide/integrating-api-with-aws-services-s3.html) on API gateway as S3 proxy.

## structure
1. /: GET gives list of all buckets
2. /{folder}: GET PUT DELETE for buckets
3. /{filder}/{item}: GET PUT DELETE HEAD for items in bucket

## steps
1. Create IAM role for API gateway. Then attach s3fullaccess policy to it.
2. Add resources and methods as mentioned above. 
3. During creation

  **integration type**: AWS service>s3  
  **HTTP method**: PUT GET DELETE(accordingly)  
  **Path override** for each method depends on its resource. {folder}/{item} will be mapped to {bucket}/{object}:    
  ```
  1. Resource /: /
  2. Resource /{folder}: {bucket} 
  3. Resource /{folder}/{item}: {bucket}/{object}
  ```
4. Path mappings
 
object ->	method.request.path.item		 (only for resource #3)              
bucket ->	method.request.path.folder   (for resource #2 and #3)       

5. Header mappings(resource #2 and #3)   
Most s3 parameters like meta are passed through headers.   

x-amz-acl ->	'authenticated-read'	      	 
Content-Type -> 	method.request.header.Content-Type      

## working
Body can have text or binary. URL specifies bucket and object name. To enable binary add */* in settings>binary media. If IAM was used to authenticate API then add authorization>AWS signature
```
https://xn2nuhcvxd.execute-api.us-east-1.amazonaws.com/dev/demobucket/bible.jpeg
```

      
