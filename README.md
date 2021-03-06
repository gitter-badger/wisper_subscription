# WisperSubscription

This Gem contains a class, `WisperSubscription`, which encapsulates an extremly
simple container to record call parameters for messages, and query methods for
whether a defined message was sent. This is a refinement of a class
[originally developed](https://github.com/jdickey/new_poc/blob/c255cbd/spec/support/broadcast_success_tester.rb)
to record results of very specific [Wisper](https://github.com/krisleech/wisper)
broadcast messages to support testing of broadcasters of such messages. While
[restructuring that app](https://github.com/jdickey/new_poc/issues/54), the
existing `BroadcastSuccessTester` was identified as a dependency of the testing
for [*multiple*](https://github.com/jdickey/new_poc/issues/173) to-be-decoupled
classes. Including that test-support class in so many redundant places would have
been an engraved invitation to sync-related bugs (and egregious waste), and so
this `WisperSubscription` class/Gem was created.

## Installation

Add this line to your application's Gemfile:

```ruby
gem 'wisper_subscription'
```

And then execute:

    $ bundle

Or install it yourself as:

    $ gem install wisper_subscription

## Usage

Remember that this was originally developed to support testing Wisper message
broadcast from an object. So you might have an RSpec file (Minitest would be
similar) that had excerpts [similar to](https://github.com/jdickey/new_poc/blob/c255cbd/spec/actions/index_users_spec.rb):

```ruby
describe SomeWisperPublishingClass do
  let(:subscriber) { WisperSubscription.new }
  let(:command_params) do
    # ...
  end
  let(:command) { described_class.new command_params }

  before :each do
    # some setup
    subscriber.define_message :success
    subscriber.define_Message :failure
    command.subscribe subscriber
    command.do_something_that_broadcasts_a_success
  end

  it 'was successful' do
    payload = subscriber.payload_for(:success).to_a.first
    expect(payload).to be_a WhatYouExpectOnSuccess # as opposed to nil
  end

  # ...
end
```

You get the idea. If not, open an issue or ask on our Gitter channel.

## Contributing

1. Fork it ( https://github.com/jdickey/wisper_subscription/fork )
1. Create your feature branch (`git checkout -b my-new-feature`)
1. Ensure that your changes are completely covered by *passing* specs, and comply with the [Ruby Style Guide](https://github.com/bbatsov/ruby-style-guide) as enforced by [RuboCop](https://github.com/bbatsov/rubocop). To verify this, run `bundle exec rake`, noting and repairing any lapses in coverage or style violations;
1. Commit your changes (`git commit -a`). Please *do not* use a single-line commit message (`git commit -am "some message"`). A good commit message notes what was changed and why in sufficient detail that a relative newcomer to the code can understand your reasoning and your code;
1. Push to the branch (`git push origin my-new-feature`)
1. Create a new Pull Request. Describe at some length the rationale for your new feature; your implementation strategy at a higher level than each individual commit message; anything future maintainers should be aware of; and so on. *If this is a modification to existing code, reference the open issue being addressed*.
1. Don't be discouraged if the PR generates a discussion that leads to further refinement of your PR through additional commits. These should *generally* be discussed in comments on the PR itself; discussion in the Gitter room (see below) may also be useful;
1. If you've comments, questions, or just want to talk through your ideas, come hang out in the project's [room on Gitter](https://gitter.im/jdickey/wisper_subscription). Ask away!
