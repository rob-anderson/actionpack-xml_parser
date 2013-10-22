actionpack-xml\_parser
======================

A XML parameters parser for Action Pack (removed from core in Rails 4.0)

It makes Rails automagically parse xml into the params hash, so you can write nice format-agnostic code in your controllers.

Why the fork?
-------------

The standard gem doesn't like namespace prefixes very much.  In fact if you feed it namespaced xml, it will barf an ActionDispatch::ParamsParser::ParseError (Undefined prefix) all over your production log.

And who can blame it?  "Namespace all the things!" may be a valid, even necessary, approach if you are implementing a SOAP service or (God help you) WSS.  But if your RESTful Rails API is serene enough to accept html, json and xml then it probably doesn't need namespacing.

And it's your API, right?  So you just specify namespace-free XML and you are good to go.

But if hypothetically a third party company decided to build a client for your API, and if this hypothetical company built their client using some GUI-based Java IDE on Windows about which you know very little, and if this IDE generated client code at the touch of a button, which produced XML festooned with spurious ns2: prefixes like baubles on a Christmas tree, then you would have a problem.

You might speak to the third party company, and point out kindly and reasonably that ns2: prefixes are nowhere to be found in your API document, and are also absent from the numerous XML examples with which you thoughtfully illustrated said document, and therefore arguably have no place in the transactions they keep sending you.

But they might respond that they don't undertand how their IDE works, they just press this button see, and anyway this all used to work when your API was hosted with a Rails 2 back end.  And then they would start to dribble and rub their hands on their knees in a fashion that invariably precedes the words 'change request'.

So regretfully you would have to fork and patch the upstream gem, so that if you supply it with namespaced xml it will just pinch its nose between finger and thumb and look the other way with a pained expression, much as Rails 2 used to do.

In practice this means applying a ham-fisted regex to delete the namespace prefixes from the xml before attempting to parse it to a hash.

So in summary this is a dirty and shameful hack which probably should not be used by anyone other than me.  I leave it here merely so that you can shake your head and wonder at the folly of Man.


Installation
------------

Include this gem into your Gemfile:

```ruby
gem 'actionpack-xml_parser', :git => 'git://github.com/rob-anderson/actionpack-xml_parser.git'
```

Then, add `ActionDispatch::XmlParamsParser` middleware after `ActionDispatch::ParamsParser`
in `config/application.rb`:

```ruby
config.middleware.insert_after ActionDispatch::ParamsParser, ActionDispatch::XmlParamsParser
```
