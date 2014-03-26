---
layout: post
title: "Stop Validating Email Addresses With Complicated Regular Expressions"
date: 2012-09-06 10:33
categories: [programming, ruby, rails]

hackernews: http://news.ycombinator.com/item?id=5763327
---

Just stop, guys. It's a waste of your time and your effort. Put down your Google search for an [email regular expression](http://www.google.com/search?q=email+regex), take a step back, and breathe. There's a famous quote that goes:

> Some people, when confronted with a problem, think, "I know, I'll use regular expressions." Now they have two problems.
>
> — [Jamie Zawinksi](http://regex.info/blog/2006-09-15/247)

Here's a fairly common code sample from Rails Applications with some sort of authentication system:

{% highlight ruby linenos=table linespans=line %}
class User < ActiveRecord::Base
  # This regex is from https://github.com/plataformatec/devise, the most
  # popular Rails authentication library
  validates_format_of :email, :with => /\A[^@]+@([^@\.]+\.)+[^@\.]+\z/
end
{% endhighlight %}

This seems fairly simple (unless you don't know Regex), but it can get way worse...

{% highlight ruby linenos=table linespans=line %}
class User < ActiveRecord::Base
  validates_format_of :email, :with => /^(|(([A-Za-z0-9]+_+)|([A-Za-z0-9]+\-+)|([A-Za-z0-9]+\.+)|([A-Za-z0-9]+\++))*[A-Za-z0-9]+@((\w+\-+)|(\w+\.))*\w{1,63}\.[a-zA-Z]{2,6})$/i
end
{% endhighlight %}

Or even worse still...

{% highlight ruby linenos=table linespans=line %}
class User < ActiveRecord::Base
  validates :email, :with => EmailAddressValidator
end

class EmailAddressValidator < ActiveModel::Validator
  EMAIL_ADDRESS_QTEXT           = Regexp.new '[^\\x0d\\x22\\x5c\\x80-\\xff]', nil, 'n'
  EMAIL_ADDRESS_DTEXT           = Regexp.new '[^\\x0d\\x5b-\\x5d\\x80-\\xff]', nil, 'n'
  EMAIL_ADDRESS_ATOM            = Regexp.new '[^\\x00-\\x20\\x22\\x28\\x29\\x2c\\x2e\\x3a-\\x3c\\x3e\\x40\\x5b-\\x5d\\x7f-\\xff]+', nil, 'n'
  EMAIL_ADDRESS_QUOTED_PAIR     = Regexp.new '\\x5c[\\x00-\\x7f]', nil, 'n'
  EMAIL_ADDRESS_DOMAIN_LITERAL  = Regexp.new "\\x5b(?:#{EMAIL_ADDRESS_DTEXT}|#{EMAIL_ADDRESS_QUOTED_PAIR})*\\x5d", nil, 'n'
  EMAIL_ADDRESS_QUOTED_STRING   = Regexp.new "\\x22(?:#{EMAIL_ADDRESS_QTEXT}|#{EMAIL_ADDRESS_QUOTED_PAIR})*\\x22", nil, 'n'
  EMAIL_ADDRESS_DOMAIN_REF      = EMAIL_ADDRESS_ATOM
  EMAIL_ADDRESS_SUB_DOMAIN      = "(?:#{EMAIL_ADDRESS_DOMAIN_REF}|#{EMAIL_ADDRESS_DOMAIN_LITERAL})"
  EMAIL_ADDRESS_WORD            = "(?:#{EMAIL_ADDRESS_ATOM}|#{EMAIL_ADDRESS_QUOTED_STRING})"
  EMAIL_ADDRESS_DOMAIN          = "#{EMAIL_ADDRESS_SUB_DOMAIN}(?:\\x2e#{EMAIL_ADDRESS_SUB_DOMAIN})*"
  EMAIL_ADDRESS_LOCAL_PART      = "#{EMAIL_ADDRESS_WORD}(?:\\x2e#{EMAIL_ADDRESS_WORD})*"
  EMAIL_ADDRESS_SPEC            = "#{EMAIL_ADDRESS_LOCAL_PART}\\x40#{EMAIL_ADDRESS_DOMAIN}"
  EMAIL_ADDRESS_PATTERN         = Regexp.new "#{EMAIL_ADDRESS_SPEC}", nil, 'n'
  EMAIL_ADDRESS_EXACT_PATTERN   = Regexp.new "\\A#{EMAIL_ADDRESS_SPEC}\\z", nil, 'n'

  def validate(record)
    unless record.email =~ EMAIL_ADDRESS_EXACT_PATTERN
      record.errors[:email] << 'is invalid'
    end
  end
end
{% endhighlight %}

Yeesh. Is something that complex really necessary? If you actually check the Google query I linked above, people have been writing (or trying to write) [RFC-compliant](http://tools.ietf.org/html/rfc2822) regular expressions to parse email addresses for years. They can get ridiculously convoluted as in the case above and, according to the specification, are often too strict anyway.

Sections [3.2.4](http://tools.ietf.org/html/rfc2822#section-3.2.4) and [3.4.1](http://tools.ietf.org/html/rfc2822#section-3.4.1) of the RFC go into the requirements on how an email address needs to be formatted and, well, there's not much you can't do in your email address when quotes or backslashes are involved. The local string (the part of the email address that comes before the @) can contain the following characters:

{% highlight text %}
! $ & * - = ^ ` | ~ # % ' + / ? _ { }
{% endhighlight %}

But guess what? You can use pretty much any character you want if you escape it by surrounding it in quotes. For example, `"Look at all these spaces!"@example.com` is a valid email address. Nice.

For this reason, for a time I began running any email address against the following regular expression instead:

{% highlight ruby %}
class User < ActiveRecord::Base
  validates_format_of :email, :with => /@/
end
{% endhighlight %}

Simple, right? Email addresses have to have an @ symbol. This is often the most I do and, when paired with a confirmation field for the email address on your registration form, can alleviate most problems with user error. But what if I told you there were a way to determine whether or not an email is valid without resorting to regular expressions at all? It's surprisingly easy, and you're probably already doing it anyway.

## Just send them an email already

No, I'm not joking. Just send your users an email. The activation email is a practice that's been in use for years, but it's often paired with complex validations that the email is formatted correctly. If you're going to send an activation email to users, why bother using a gigantic regular expression?

Think about it this way: I register for your website under the email address `qwiufaisjdbvaadsjghb@gmail.com`. C'mon. That's probably going to bounce off of the illustrious mail daemon, but the formatting is fine; it's a valid email address. To fix this problem, you implement an activation system where, after registering, I am sent an email with a link I must click. This is to verify that I actually own that email address before my account is activated. At this point, why keep parsing email addresses for their format? The result of sending an email to a badly formatted email address would be the same: it'll get bounced. If your user enters a bad email address, they won't get the activation email and they'll try to register again if they really care about using your site. It's that simple.

So eschew your fancy regular expressions already. If you really want to do checking of email addresses right on the signup page, include a confirmation field so they have to type it twice. Enterprising individuals will just copy and paste, but what it comes down to is this: if your user enters a bad email address, you shouldn't make it more of a problem for yourself than you have to. A complex regex validation on the email address doesn't introduce an additional solution, it introduces an additional problem. If you really, really want to make sure people are typing in an actual email address, just use the `/@/` regular expression and call it done. Feeling ambitious? Then check for the dot too: `/.+@.+\..+/i`. Anything more is overkill.

_UPDATE: As several users in the comments have also pointed out, many email address regexes on the web will show tagged emails (i.e. `email+tag@example.com`) as invalid. Lots of people use tags in their email addresses while registering as a pair with their email service's filtering systems. Keep that in mind if you don't wish to heed the above advice._

_Additionally, you could (and should) take a look at [Kicksend's mail checker](https://github.com/Kicksend/mailcheck) to do some client-side validations in the form of typo fix suggestions._
