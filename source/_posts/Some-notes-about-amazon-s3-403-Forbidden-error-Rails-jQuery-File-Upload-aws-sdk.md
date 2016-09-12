---
title: Some notes about amazon s3 403 (Forbidden) error. Rails + jQuery-File-Upload + aws-sdk
permalink: untitled-6
id: 10
updated: '2015-01-27 19:05:15'
date: 2015-01-27 19:04:04
tags:
---

# Failed to load resource: the server responded with a status of 403 (Forbidden) 
Do not include the follow line in the form: `%input{:type => :hidden, :name => :success_action_redirect, :value => root_url}` 

### It seems you can not send the above information to amazon. 

### OK, finally, I realize that the main problem is the policy document. Let's look at the following example: 

<!--more-->

*The following code is in the form view:* 
```haml upload.html.haml
%form#file_upload(action="http://YOUR_BUCKET.s3.amazonaws.com" method="post" enctype="multipart/form-data") 
-# order is important! 
-# also, the things that are not filled in right now \*will\* be filled in soon. See below. 
%input{:type => :hidden, :name => :key} 
%input{:type => :hidden, :name => "AWSAccessKeyId"} 
%input{:type => :hidden, :name => :acl, :value => 'public-read'} 
%input{:type => :hidden, :name => :success_action_status, :value => "200"} 
%input{:type => :hidden, :name => :policy} 
%input{:type => :hidden, :name => :signature} 

.fileupload-content 
	.fileupload-progress 
.file-upload 
	%label.fileinput-button 
  %span Upload Document 
  %input{:type => :file, :name => :file} 
``` 
*And the following code is in your controller which mean to generate the policy document and the signature.* 
```ruby signed_urls_controller.rb
class SignedUrlsController < ApplicationController 
	def index 
    s3 = AWS::S3::new 
    bucket = s3.buckets['YOUR_BUCKET'] 
    signed_data = bucket.presigned_post 
    render json: signed_data.fields.merge(key: "uploads/#{SecureRandom.uuid}/#{params[:doc][:title]}") 
  end 
end
``` 
**Then the error 403 will raise.** However, if you modify the controller like this: 
```ruby signed_urls_controller.rb
class SignedUrlsController < ApplicationController 
	def index 
  	s3 = AWS::S3::new 
    bucket = s3.buckets['YOUR_BUCKET'] 
    signed_data = bucket.presigned_post(acl: 'public-read', success_action_status: '200') 
    render json: signed_data.fields.merge(key: "uploads/#{SecureRandom.uuid}/#{params[:doc][:title]}") 
  end 
end 
```
**Then the error disappear.** So, basically. There are two conclusions we can make here: 
+ The form must include four basic fields, including 'AWSAccessKeyId', 'policy', 'signature' and 'key'. All of them should have value, which can be assigned eithere directly in the form or through a javascript.
+ If there is an extra field other than the above four, say 'acl', in the form, then we must also set this property in the 'presigned_post' function. This is because in the policy, there must be the same information we set in the form. This is how security is ensured.