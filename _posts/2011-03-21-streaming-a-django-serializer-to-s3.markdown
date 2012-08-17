---
author: bradwhittington
date: '2011-03-21 11:52:23'
layout: post
slug: streaming-a-django-serializer-to-s3
status: publish
title: '"Streaming" a django serializer to amazon S3 using boto'
wordpress_id: '11'
tags: [amazon, backup, boto, Django, Gist, Python, s3, serialization]
---

I have been trying out [ep.io](http://ep.io "ep.io") with a friend
for some new projects, and he needed a way to store backups of his
DB somewhere. After hours of scratching around google and
[Django](http://djangoproject.com "Django"), I was no closer to a
nice solution for storing backups directly to S3 (without using
local storage). You can use Django's serialization framework to
[write directly to a file-like object](http://docs.djangoproject.com/en/dev/topics/serialization/#serializing-data),
and you can use the "set\_contents\_from\_file" in boto's S3 bucket
class to read a file like object and stream it to S3. I dug around
a little bit more (because my wheels were spinning contemplating
writing threads/processes to connect the producer and consumer),
and discovered
[multipart uploads to S3 in boto](http://www.elastician.com/2010/12/s3-multipart-upload-in-boto.html).
I wrapped this in a file-like, buffered writer class which means I
can instantiate an object, and hand it to the django serializer
directly, and have my writes streamed directly to S3 (via an
intermediate buffer). It is liberal on the exception handling, and
could probably be improved monumentally, and is probably an
inappropriate use of the underlying S3/boto API. You can get a copy
of the class from github / gist:
[here](https://gist.github.com/872948) (Doesn't seem like I can
embed a gist directly to a wordpress.com hosted blog) Example usage
is:
    from my_app.models import MyModel
    from django.core import serializers
    fd = MultiPartUploader('my_bucket','my_models.xml')
    serializers.get_serializer('xml')().serialize(MyModel.objects.all(), stream=fd)
    fd.close()

You have to close the file handle after using it, otherwise the
file will not be 'closed' on S3 and hang around as an in-progress
multipart file upload.


