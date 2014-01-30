# Paperclip Meta 

[![Gem Version](https://badge.fury.io/rb/paperclip-meta.png)](http://badge.fury.io/rb/paperclip-meta)
[![Build Status](https://travis-ci.org/teeparham/paperclip-meta.png)](https://travis-ci.org/teeparham/paperclip-meta)

Add width, height, and size to paperclip images.

Paperclip Meta gets image dimensions after post_process_styles using paperclips own Geometry.from_file.
This should make paperclip-meta storage independent.

### Setup

Add paperclip-meta to Gemfile:

```ruby
gem 'paperclip-meta'
```

Create migration to add a *_meta column:

```ruby
class AddMetaToAvatar < ActiveRecord::Migration
  def self.up
    add_column :users, :avatar_meta, :text
  end

  def self.down
    remove_column :users, :avatar_meta
  end
end
```

Rebuild all thumbnails to fill meta column if you already have some attachments.

Now you can use meta-magic:

```
  <%= image_tag @user.avatar.url, :size => @user.avatar.image_size %>
  <%= image_tag @user.avatar.url(:medium), :size => @user.avatar.image_size(:medium) %>
  <%= image_tag @user.avatar.url(:thumb), :size => @user.avatar.image_size(:thumb) %>
```

### Internals

The meta column is simple hash:

```ruby
:style => {
  :width => 100,
  :height => 100,
  :size => 42000
}
```

This hash will be marshaled and base64 encoded before writing to model attribute.

Meta methods provided:
  - width `@user.avatar.width(:thumb)`
  - height `@user.avatar.height(:medium)`
  - size `@user.avatar.size`

You can pass the image style to these methods. If a style is not passed, default_style will be used.
