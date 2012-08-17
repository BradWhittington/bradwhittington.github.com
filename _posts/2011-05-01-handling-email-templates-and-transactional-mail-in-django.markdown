---
author: bradwhittington
date: '2011-05-01 23:24:51'
layout: post
slug: handling-email-templates-and-transactional-mail-in-django
status: publish
title: Handling email templates and transactional mail in Django
wordpress_id: '24'
tag: [Development, Django, Email, PostageApp, Python, SES, templates, templating, transactional email]
---

There is a common pattern when building web applications, of
needing a simple way to handle email to your users. Depending on
how big a hammer you need, this may start at using
[Django](http://djangoproject.com/)'s built-in
[send\_mail()](http://docs.djangoproject.com/en/dev/topics/email/#send-mail)
command using a template string for the subject and content, right
up to needing a solution for your design team to own the look and
feel of your emails, in a simple manner. Inspired by a post on
stackoverflow
("[Creating email templates with Django](http://stackoverflow.com/questions/2809547/creating-email-templates-with-django)"),
and some other
[helpful posts](http://www.rossp.org/blog/2006/jul/11/sending-e-mails-templates/),
I decided to build something a bit useful for more thant just me.
The result is an
[email template solution for django](https://github.com/bradwhittington/django-templated-email).

It isn't perfect, but it scratches my itch. I wanted a simple way
to template and send emails, control html and plain text versions
easily, and, most importantly, have a growth strategy to move
beyond Django's templates. So...simple way to send transactional
mails (i.e. once off emails to users with custom information, as
opposed to mass mailing management), grow up to support different
development/deployment patterns. I was looking for the equivalent
of
[render\_to\_response](http://docs.djangoproject.com/en/dev/topics/http/shortcuts/#render-to-response)
for email. 

I have experience with
[Silverpop's transactional email](http://www.silverpop.com/)
component, and after some poking around I found the excellent
[PostageApp](http://postageapp.com/) for handling transactional
mail, and I have been in an environment where the design team
focuses on making emails work the best they can, so I recognise the
value in the pattern of working. Dealing with a templating language
is okay, but, email is a crazy beast, and if you can farm out the
nitty gritty of complex html email to a 3rd party provider, then it
is a winner (especially if it means not having to be IT support to
designers who are rocky in their grasp of version control systems).

So far I have built a simple back-end which uses Django's built in
send\_mail(), rendering the templates from a specific template
directory (namely templates/templated\_email). I have tested that
using a normal smtp server, and using
[django-ses](https://github.com/hmarr/django-ses) (the drop in
email backend to use Amazon SES). I then started working against
PostageApp, which required first that I build a
[python wrapper around PostageApp API](https://github.com/bradwhittington/python-postageapp)'s,
and then incorporated it as a
[backend for django-templated-email](https://github.com/bradwhittington/django-templated-email/blob/master/templated_email/backends/postageapp_backend.py).
I changed the back-end setting, and BAM I was sending using
PostageApp, with no need for any other code changes. This is great
as it means I can use different settings for testing and
production, with stub templates to handle rough testing and
development, without eating into more expensive emails in
production, allowing me to focus on developing in my IDE, without
having to break out into a different environment, just to cater for
emails. 

I have built a rough and untested back-end for rendering
using Django's template system, and sending with
[Mailchimp's STS](http://apidocs.mailchimp.com/sts/1.0/) backend
(which adds tracking on top of SES). But I don't know when I will
have the time to knuckle down and test it properly. 

I also saw a
pretty nifty (I guess you can call it that?) implementation of
[getting your plain-text, html, and email subject into a single file here](http://stackoverflow.com/questions/2051526/email-templating-in-django)
I don't love the specific hacking of the Django template rendering
stuff (because the API could plausibly change, and it breaks
template inheritance). I do love the single file management aspect,
and it solves the issue of email subject in a far more elegant way
than my project currently does. But it is what it is. It has
similarities with other solutions out there (like
[django-email-extras](http://code.google.com/p/django-email-extras/)),
and it doesn't (yet) support file attachments, or inline images
which are automatically attached from the filesystem, but it is
good enough for my purposes. I am going to be improving it in the
coming weeks, as I work on aspects which require features for my
work on
[Lessfuss](http://lessfuss.co.za "A personal assistant service for South Africa")
(more details on that,
[here](http://blog.lessfuss.co.za/tagged/getting_started)), for
e.g. sending file attachments.


