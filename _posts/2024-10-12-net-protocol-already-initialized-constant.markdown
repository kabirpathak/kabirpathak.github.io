---
layout: post
title: "Fixing the “Already Initialized Constant Net::ProtocRetryError” Warning in Ruby"
tldr: 
modified: 2024-10-12 03:05:24 +0530
category: non-tech
tags: [net-http, net-protocol]
featured: true
rating: 5
author: kabir 
image: assets/images/tech/net-protocol-already-initialized-constant.png
---
If you’ve encountered the following warning in your Ruby code:

/ruby-2.x.x/lib/ruby/2.x.x/net/protocol.rb:66: warning:
already initialized constant Net::ProtocRetryError
then you’re dealing with a conflict between different versions of Ruby’s net-protocol library. This issue occurs when multiple versions of the net-http library are loaded simultaneously:

The standard library (stdlib) version: net/http
The gemified version: net-http
Its really not complex, but quite interesting. Let’s dive in.

Understanding the Issue
In my case, the faraday-net_http gem, which adapts net/http for Faraday, was loading net/http from the Ruby stdlib.

Upon bundle open faraday-net_http :

```ruby
lib/faraday/adapter/net_http.rb:8

require 'net/http' # this causes the issue
```

However, in your case, it might be a different gem that triggers the conflict, depending on which libraries or dependencies your application is using.

Starting with Ruby 2.5, parts of the standard library began to be gemified, and by Ruby 3.0, net-http and net-protocol were available as standalone gems.

And this is where our issue lies — prior to version 3.0, Ruby by default loads the stdlib version of net/http, which internally requires the stdlib version of net/protocol.

Meanwhile, net-imap, which was updated to declare net-protocol as a dependency, requires the gemified version of net-protocol. This leads to a conflict where both the stdlib and gemified versions of net-protocol are loaded at the same time.

The warning about “already initialized constant Net::ProtocRetryError" happens when multiple versions of the net-protocol library are loaded simultaneously. This typically occurs when:

net/http from Ruby's stdlib is required.
net-imap gem, which now depends on the gemified version of net-protocol, is also loaded.
The Solution
To resolve the conflict, I explicitly added the gemified net-http to my Gemfile:

gem 'net-http'
This forces Bundler to load the gemified version of net-http (and, by extension, the gemified version of net-protocol), ensuring that the correct version is loaded first. Here's how this solves the problem:

Before: The standard library version of net/http loaded the stdlib version of net-protocol, and then net-imap loaded the gemified version of net-protocol, causing the "already initialized constant" warning.
After: Bundler now loads the gemified version of net-http, ensuring that the gemified net-protocol is loaded first, preventing the conflict and eliminating the warning.
Why This Works
By explicitly specifying net-http in the Gemfile, Bundler is directed to load the gem version rather than the standard library version of net/http. Since the gemified version includes all the necessary dependencies (including net-protocol), this resolves any conflicts between the stdlib and gem versions.

Keep in Mind
Pinning the net-http gem in the Gemfile will also add its dependent uri gem to your lockfile. So, make sure that your Bundler version is compatible with the uri version associated with the latest or chosen version of net-http you're adding to your Gemfile. Also, ensure that your Bundler version is consistent across all environments (development as well as production).

We faced the following issue while deploying to the production environment after pinning net-http:

You have already activated uri 0.10.0, but your Gemfile requires uri 0.13.1.
Since uri is a default gem, you can either remove your dependency on it or try
updating to a newer version of bundler that supports uri as a default gem.
Since this is just a temporary solution while upgrading to Ruby 3, we chose to pin the gem uri to version 0.10.0:

gem 'net-http' 
gem 'uri', '0.10.0'
Conclusion
If you’re facing the “already initialized constant Net::ProtocRetryError” warning, it’s likely due to a conflict between the stdlib and gemified versions of net-protocol. By adding net-http to your Gemfile, you can ensure that Bundler loads the correct gemified version, resolving the issue and keeping your application running smoothly.

While this fix resolves the issue temporarily, the long-term solution is to upgrade to Ruby 3, where Bundler better manages gemified libraries and avoids such conflicts.

If you wish to investigate furthur, here’s the original thread: https://github.com/ruby/net-imap/issues/16#issuecomment-803086765

I hope you found this post helpful, or at the very least interesting! Your feedback is always welcome, as I’m committed to improving with every piece I write. See you next week!
