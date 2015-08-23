# Rockflow

What is rockflow? Well with rockflow you are able to define workflows (yes even parallel workflows) by writing simple small steps and aggregating them in your flow (the bigger picture... you know)

Let's start the tour!

## Installation

Add this line to your application's Gemfile:

```ruby
gem 'rockflow'
```

And then execute:

    $ bundle

Or install it yourself as:

    $ gem install rockflow

## Usage

To write your own flow you need two ingredients: the **flow** and your **steps** lets start by looking at the flow.

##### Flow
```ruby
# app/workflows/awesome_flow.rb
class AwesomeFlow < Rockflow::Flow
    def setup
        rock RockStep1
        rock RockStep2
        rock RockStep3, after: [RockStep1, RockStep2]
    end
end
```
Easy right? Notice that **RockStep1** and **RockStep2** will be executed parallel (so beware of thread safety and stuff). **RockStep3** will only execute if **RockStep1** and **RockStep2** are finished.

##### Steps
Look at my steps ... my steps are amazing...
```ruby
# app/steps/rock_step1.rb
class RockStep1 < Rockflow::Step
    def it_up
        puts "Iam RockStep1 and i am adding something to the payload"
        add_payload :a, 2
    end
end
```
```ruby
# app/steps/rock_step2.rb
class RockStep2 < Rockflow::Step
    def it_up
        puts "Iam RockStep2 and i am adding something to the payload"
        add_payload :b, 2
    end
end
```

```ruby
# app/steps/rock_step3.rb
class RockStep3 < Rockflow::Step
     def it_up
        puts "Iam RockStep3 and i am aggregating something from the payload"
        result = payload[:a] + payload[:b] + payload[:c]
        puts "Result: #{result}"
    end
end
```

## Development

After checking out the repo, run `bin/setup` to install dependencies. Then, run `bin/console` for an interactive prompt that will allow you to experiment.

To install this gem onto your local machine, run `bundle exec rake install`. To release a new version, update the version number in `version.rb`, and then run `bundle exec rake release` to create a git tag for the version, push git commits and tags, and push the `.gem` file to [rubygems.org](https://rubygems.org).

## Contributing

1. Fork it ( https://github.com/[my-github-username]/rockflow/fork )
2. Create your feature branch (`git checkout -b my-new-feature`)
3. Commit your changes (`git commit -am 'Add some feature'`)
4. Push to the branch (`git push origin my-new-feature`)
5. Create a new Pull Request
