# Keep original file name for download using `content_disposition` and `response_content_disposition` attributes
```
.presigned_url(:put, response_content_disposition: %{attachment; filename="#{file_content.original_filename}"})
```
```
.bucket.put_object(
      key: self.path, body: file_content, acl: 'public-read',
      content_disposition: %{attachment; filename="#{file_content.original_filename}"})
```
