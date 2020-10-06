# unique-identifier

Provide a unique identifier string for a mongoid collection in Ruby.

## Installation

Add this line to your application's Gemfile:

    gem "unique-identifier", github: 'patriotphoenix/unique-identifier'

And then execute:

    $ bundle

## Usage

Just `include` the modules that your Rails models are concerned with. Requires `unique-identifier` to run which adds a new `mongoid` field to your model that is called `:identifier` that contains a unique and random 9 (default) character / digit string for every instance of said model that has this `unique-identifier` concern enabled. This gem requires it since `:id` for Mongoid is too long for most circumstances. At 9 characters, over 7.6 TRILLION unique identifiers will be available, however the gem allows you to customize the length of the unique identifier.

### Configuration

To configure this gem, please create a new file inside `config/initializers/unique-identifier.rb` and consider the following template: 

```ruby
UniqueIdentifier.configure do |config| 
  config.prefix = nil # (default is nil) or String such as "myapp_"
  config.length = 9 # integer value only, default is 9
end #/def
```

### QuickTotals::Sync

This gem requires **one** method to be defined in _every_ model that includes the Rails Concern `QuickTotals::Sync` called `qt_relationships`. The `qt_lookups` method is optional and should only be used when needed.

```ruby
class Person 
  include Mongoid::Document
  include UniqueIdentifier
end #/class

jack_id = nil

jack = Person.new
if jack.save
  jack_id = jack.identifier
  puts jack_id
end #/if

jack = Person.where(identifier: jack_id).first
puts jack
# => #<Person identifier: "ABC123DEF" _id: BSON::ObjectId('')>
```