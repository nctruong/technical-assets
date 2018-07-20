---
layout: page
title: HAMLC
permalink: /bash/
---

### .coffee
```javascript
html = JST['dashboard/evaluations/comment_imgpreview']({ image: image })
```
image returned from server as a json: 
```ruby
{ id: image.id, original_url: image.original_url } 
```

### .hamlc
```javascript
.js-item-photo.c-extra-photo
  .c-evaluation__imagepreview-item{ style: "background-image: url('#{@image['original_url']}');" }
    .js-btn-delete-photo.c-btn-delete-extra-photo.btn.btn-primary{'data-id': "#{@image['id']}"}
      %i.fa.fa-trash-o
```

Pay attention that hamlc uses the global syntax with "@".

