---
layout: post
title: "Paperclip image upload via URL"
date: 2011-11-21T17:56:00+05:30
comments: false
categories:
 - paperclip
 - rails 3
 - imagemagick
---

Upload image via paperclip via passing URL instead of file upload 

```ruby
# Consider Print instance with image as file attachment
class Print < ActiveRecord::Base
  has_attached_file :image
  def upload_image(url)
    begin
      io = open(URI.escape(url))
      if io
        def io.original_filename; base_uri.path.split('/').last; end
        io.original_filename.blank? ? nil : io
        p.image = io
      end
      p.save(false)
    rescue Exception => e
      logger.info "EXCEPTION# #{e.message}"
    end
  end
end
```

Text code from console
```
p = Print.new
url = "http://ocdevel.com/sites/ocdevel.com/files/images/rails.png"
p.upload_image(url)
```
