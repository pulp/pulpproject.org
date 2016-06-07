---
id: 1115
title: Pulp in the New Year
date: 2015-01-02T18:21:58+00:00
author: Michael Hrivnak
layout: post
guid: http://www.pulpproject.org/?p=1115
permalink: /2015/01/02/pulp-in-the-new-year/
categories:
  - Development
---
2015 will bring some exciting under-the-hood changes to Pulp that I&#8217;d like to share with you.

# Django

Pulp has used [web.py](http://webpy.org/ "web.py") as its web framework for a long time, but unfortunately that project has gone dormant. Since Pulp only uses a small set of web.py&#8217;s features (essentially using it as a thin WSGI adapter), replacing it is a reasonably straight-forward task. This gave us an opportunity to re-evaluate what we want out of a web framework and consider several compelling options.

We ultimately chose [Django](https://djangoproject.com/ "Django"), a powerful and mature web framework that offers a lot of added value beyond just being a WSGI adapter. To start, I expect that Django will enable (and motivate) us to replace a lot of repeated boilerplate code in Pulp&#8217;s web handlers with reusable pieces. Longer-term, Pulp could take advantage of Django&#8217;s auth framework, notification facilities, form processing, and integration with a data-access layer (see below).

Work is already in-progress for converting from web.py to Django, and this will be part of a future release **after Pulp 2.6.x**.

# MongoEngine

Pulp&#8217;s data access layer is all-custom, using the pymongo library to talk directly to MongoDB. There is a lot of boilerplate code that gets duplicated from one collection to the next, with variations, and this has become a substantial headwind for development. That is all being replaced by use of [MongoEngine](http://mongoengine.org/ "MongoEngine"), a document-object mapper that feels very similar to Django&#8217;s built-in ORM. MongoEngine is mature, stable, and will give us one consistent and reliable way to access documents in our database. Defining each collection&#8217;s schema with MongoEngine will be especially valuable.

Work is in-progress to begin using MongoEngine. We will convert one collection at a time until the job is done, so the transition is likely to span several Pulp releases. The first baby steps will be included in Pulp 2.6.0, but substantial changes won&#8217;t be present until a future release **after Pulp 2.6.x**.

# Tastypie

Looking further into the future, once the conversions to Django and MongoEngine are complete, [Tastypie](http://tastypieapi.org/ "Tastypie") offers an enticing way to combine those two frameworks to auto-generate a REST API. Pulp&#8217;s current REST API involves a lot of custom code with custom documentation that is separate from the code. Using a framework like Tastypie to generate the REST API and associated documentation could save a lot of development time, improve API stability, improve guarantees about accuracy of documentation, and improve standards-compliance.

Pulp is not yet committed to using Tastypie, but we will consider it carefully as the conversions to Django and MongoEngine are finished. If we do move forward with Tastypie, it would likely involve enough changes to the API itself to warrant a major new release of Pulp.

# Summary

These three frameworks together open up great possibilities. They replace a lot of boilerplate code and let Pulp developers focus on core functionality. Django especially offers a lot of potential added value, and Tastypie is a great demonstration of what becomes possible when using advanced frameworks like Django and MongoEngine.

Stay tuned for more updates on these efforts, and let us know if you would like to contribute in any way. Testing, feedback, war stories, improving documentation, trying betas, and packaging are all helpful ways to contribute besides just writing code.