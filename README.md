#api_pagination_headers [![Build Status](https://travis-ci.org/richardkall/api_pagination_headers.svg)](http://travis-ci.org/richardkall/api_pagination_headers) [![Dependency Status](https://gemnasium.com/richardkall/api_pagination_headers.png)](https://gemnasium.com/richardkall/api_pagination_headers)

api_pagination_headers is a gem that adds pagination info to a `Link` response header. This is considered to be the [best practice](http://www.vinaysahni.com/best-practices-for-a-pragmatic-restful-api#pagination) for REST APIs and is currently used by [GitHub](http://developer.github.com/v3/#pagination). It also adds the total number of records into a `Total-Count` header.

Example:

```
Link: <http://example.com/posts?page=2&per_page=10>; rel="next", <http://example.com/posts?page=2&per_page=10>; rel="last"
Total-Count: 11
```

##Installation

Include in your Gemfile:

* Rails 4

```ruby
gem 'api_pagination_headers'
```

* Rails 3.2

```ruby
gem 'api_pagination_headers', branch: 'rails-3-2'
```

Depending on [Kaminari](https://github.com/amatsuda/kaminari) or [will_paginate](https://github.com/mislav/will_paginate) at the moment. Support for [rails-api](https://github.com/rails-api/rails-api) is built in.

##Usage

In the controller:

* Rails 4

```ruby
class PostsController < ApplicationController
  after_action only: [:index] { set_pagination_headers(:posts) }

  def index
    @posts = Post.all.paginate(per_page: params[:per_page], page: params[:page])
    respond_with @posts
  end
end
```

* Rails 3.2

```ruby
class PostsController < ApplicationController
  after_filter only: [:index] { set_pagination_headers(:posts) }

  def index
    @posts = Post.paginate(per_page: params[:per_page], page: params[:page])
    respond_with @posts
  end
end
```

##Configuration

You can change the `Total-Count` header title by configuring the gem.

```ruby
ApiPaginationHeaders.configure do |config|
  config.total_count_header = 'X-Total-Count'
end
```

##License
Copyright © 2014 [Richard Käll](http://richardkall.se). Licensed under the MIT license.
