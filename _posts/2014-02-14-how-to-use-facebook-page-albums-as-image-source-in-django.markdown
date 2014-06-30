---
layout: post
title: "How To Use Facebook Page Albums As Image Source in Django"
date: 2014-02-14 20:39:36 +0530
comments: true
categories: 
---

Lot of companies use Facebook for marketing their products. Facebook is mainly built around images. In fact the whole ecosystem is fueled by images. In order to scale for the vast user base of Facebook, they have done a really good job. 

So what if you don't want to use your own storage for uploading images and want to use Facebook for Images? It would be so cool, right?
There are two benefits:

- You don't have to spend anytime setting up an app or service to deal with images. Believe me it is a real tough problem to solve given the size and count of images. You can't put a bar on either of these two.
- It gives your users a much user friendly way. They upload images just once on Facebook and it auto-magically appears on your website. Facebook almost never gets down so there is very good chance you will get a 99.9% uptime. 

So, In order to fix this problem last year I created a Ajax based plugins to fetch images from Facebook album. It asked user to enter album URL and name. But there was an issue with this approach. In order to get access to Facebook graph database it has to get access tokens and each access token only last two months. Plus, it collapsed when it faced IE, since there is unsolvable CORS issue with IE < 9 (our 50% client base for that project).


It was a tough problem to solve. But i finally found some good articles and projects on Internet. One such project was django-fbgallery. It used a really neat approach to get the Facebook images server side and renders it the page purely server side. It not only solved the IE issue, but also the quality and speed of image loading became instant.

But there was an issue with this app, it used URLS to render the albums and spoiled the pretty URLS that you would like for pages in Django. So I took the core part of the app ( the Facebook interaction part) and made a sweet CMS plugin out of it. 

Here is the code to make it all work:

```python Facebook.py 
from django.conf import settings
from django.core.cache import cache
import urllib2, urllib
import django.utils.simplejson as json
from django.template import defaultfilters


fql_url = 'https://api.Facebook.com/method/fql.query'
cache_expires = getattr(settings, 'CACHE_EXPIRES', 30)

def get_fql_result(fql):
    cachename = 'fbgallery_cache_' + defaultfilters.slugify(fql)
    data = None
    if cache_expires > 0:
        data = cache.get(cachename)
    if data == None: 
        options = {
            'query':fql,
            'format':'json',
        }
        f = urllib2.urlopen(urllib2.Request(fql_url, urllib.urlencode(options)))
        response = f.read()
        f.close()  
        data = json.loads(response)
        if cache_expires > 0:
            cache.set(cachename, data, cache_expires*60)
    return data

def display_album(album_id):
    """Display a Facebook album

    First check that the album id belongs to the page id specified
    """
    fb_id = settings.FB_PAGE_ID
    fql = "select aid, name from album where owner=%s and aid='%s'" % (fb_id, album_id)
    valid_album = get_fql_result(fql)
    if valid_album:
        fql = "select pid, src, src_small, src_big, caption from photo where aid = '%s'  order by created desc" % album_id
        album = get_fql_result(fql)
        #album_detail = [item for item in valid_album]       
        return album
```

I asked user to enter the album ID and name, then rendered the album using the plugin here:


```python cms_plugins.py 
from cms.plugin_base import CMSPluginBase
from cms.plugin_pool import plugin_pool
from django.utils.translation import ugettext_lazy as _
from Facebook import display_album

from models import FacebookGallery

class FacebookGalleryPlugin(CMSPluginBase):
    model = FacebookGallery
    name = _("Facebook Album Gallery")
    render_template = "cmsplugin_fbgallery/album.html"

    def render(self, context, instance, placeholder):
        album = display_album(instance.album_id)
        context.update({
          'object': instance,
          'album': album,
          })
        return context

plugin_pool.register_plugin(FacebookGalleryPlugin)
```

You could choose whatever template you want. It's entirely upto you.  

Here is the image of this plugin live in action: 

<a href="http://imgur.com/dmrxcXh"><img src="http://i.imgur.com/dmrxcXh.png" title="Hosted by imgur.com" /></a>

The plugin is open sourced here: [cmsplugin-fbgallery](https://github.com/changer/cmsplugin-fbgallery) . Feel free to contribute and ask questions.