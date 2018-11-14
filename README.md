# omg_image

Let's start with sample of what this gem can do:

[![Sample](https://github.com/igorkasyanchuk/omg_image/blob/master/docs/sample2.png?raw=true)](https://github.com/igorkasyanchuk/omg_image/blob/master/docs/sample2.png?raw=true)

If you need to generate complex images, previews, charts or basically represent any HTML snippet - this gem could help.

It's using a Chrome(headless) to convert any HTML to PNG. 

If you want to try, you just need a 5 min to see how it works. Gem is comming with an examples which you can modify and use in your real app.

## Usage

- add this gem to Gemfile
- make sure you have chrome installed
- rails g omg
- edit sample entity if you want `app/omg/simple.html.erb`
- open any view, for example app/views/home/index.html and put:

```erb
  <%= image_tag OmgImage::Processor.new('entity', key: 'xxx', title: "OMG,<br/>this looks interesting!", tags: ['This', 'is', 'a', 'sample'], description: "Change me please", size: '600,300').cached_or_new %>
  <br/>
  <%= image_tag OmgImage::Processor.new('entity', title: "OMG,<br/>this looks interesting!", description: "Small version", size: '400,200').cached_or_new(regenerate: true) %>
  <br/>
  <%= image_tag OmgImage::Processor.new('square', title: "Real-time generation", size: '200,200').cached_or_new %>
```
- refresh page and see images
- edit/create new previews and use them in any place of your app.

Also, for example you can do it on generate images directly in the models:

```ruby
class Post < ApplicationRecord
  has_one_attached :preview
  # just an example
  def Post.create_new_preview
    processor = ::OmgImage::Processor.new('entity', title: "OMG,<br/>this is created from model", description: "Small version", size: '400,200')
    processor.with_screenshot do |screenshot|
      post = Post.new
      post.preview.attach(io: File.open(screenshot.path), filename: "image.png", content_type: "image/png")
      post.save!
    end
  end
end
```

To create a new template - basically create a new file in `app/omg/<name>.html.erb`. Put any HTML/CSS into it. And use as described above.

## Requirements

- Rails App 5+ and Active Storage
- Google chrome (headless)

## Installation

Add this line to your application's Gemfile:

```ruby
gem 'omg_image'
```

And then execute:
```bash
$ bundle
```

Or install it yourself as:
```bash
$ gem install omg_image
```

## Features

- you can generate images with any complexity (you just need to know html/css)
- store cached versions of preview by `key` and you can obtain them by `OmgImage::Image.find_by(key: key)`
- generate images in background jobs or console applications
- caching for previews by `key`
- ability to refresh preview by `key`

## Google Chrome Installation

- `sudo apt install gdebi-core`
- `wget https://dl.google.com/linux/direct/google-chrome-stable_current_amd64.deb`
- `sudo gdebi google-chrome-stable_current_amd64.deb`
- verify chrome is installed `google-chrome --version`

## More about Chrome

- https://linuxize.com/post/how-to-install-google-chrome-web-browser-on-ubuntu-18-04/

## Issues

- if you process too many requests and because of timeouts dead processes may appear. To kill them `parents_of_dead_kids=$(ps -ef | grep [d]efunct | awk '{print $3}' | sort | uniq | egrep -v '^1$'); echo "$parents_of_dead_kids" | xargs kill`

## TODO

- ability to configure app (path to chrome for example)
- more samples (with layout)
- wiki pages
- ability to preview templates directly by URL

## Contributing

You are welcome to contribute.

## Inspirations

I noticed that articles without previews on dev.to site when you sending a link in skype or sharing in FB have nice preview, so I've implemented similar :)

## License

The gem is available as open source under the terms of the [MIT License](https://opensource.org/licenses/MIT).
