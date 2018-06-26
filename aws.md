---
layout: page
title: Amazon Webservice
permalink: /aws/
---

## Keep original file name for downloading 
- `content_disposition` for :put and :post
- `response_content_disposition` for :get (public url)
#### GET
```ruby
object = Aws::S3::Resource.new.bucket(bucket_name).object(key)
object.presigned_url(:put, response_content_disposition: %{attachment; filename="#{file_content.original_filename}"})
object.presigned_url(:get, expires_in: expires_in, response_content_disposition: "attachment; filename='#{attachment.name}'")
```
#### PUT
```ruby
bucket.put_object(
      key: self.path, body: file_content, acl: 'public-read',
      content_disposition: %{attachment; filename="#{file_content.original_filename}"})
```