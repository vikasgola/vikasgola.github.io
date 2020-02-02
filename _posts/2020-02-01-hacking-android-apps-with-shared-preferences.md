---
title: Hacking Android Apps with Shared Preferences
tags: [Android, Cyber Security, Hacking]
style: 
color: 
description: In this post, I would show how you can edit/exploit shared preferences of android applications.
comments: true
---

<a class="text-center" href="https://feedburner.google.com/fb/a/mailverify?uri=VikasGola&amp;loc=en_US" onclick="window.open(this.href, 'subscribe',
    'left=20,top=20,width=500,height=500,toolbar=1,resizable=0'); return false;">Subscribe for New Posts</a>

Shared Preferences allow android developers to store data as key-value pair in android devices for any specific application which may be used later for multiple purposes. It is kind of local storage for android applications. It does not use any kind of encryption by itself to store this data and it is not safe to save important data there. However, some developers either don't know about it or thought that thing won't be attacked. This data is stored at location “/data/data/” in [XML](https://www.wikiwand.com/en/XML) files which can’t be accessed by normal android users. So, how can we access it and what’s so important there?

[Rooting](https://www.wikiwand.com/en/Rooting_(Android)) android device is similar to achieving super user access in android which opens a whole new world of android. With root user, you can tweak hardware settings, remove bloatware, fully control applications, install custom ROMs, install BusyBox (bundle of Unix utilities), and much more. Now, you probably have guessed now that we would be needed a rooted android device. I would not be discussing how to root android device and you can search on the web for how to do it.

Root android user can also read, write, and modify all files of “/” directory. I would be using ES File Explorer(you may use any other root file explorer) to view and edit files as most of the android device does not contain root file manager or explorer which can show these files.

1. Install the app you want to hack or explore and use it for a while.

2. Open path “/data/data/” in ES File Explorer where you would find folders with package name of your installed applications.
    {% include elements/figure.html image="https://vikasgola.github.io/assets/2020-02-01-hacking-android-with-shared-preferences-root.jpg" caption="Click 'Device' to open root directory " %}

    {% include elements/figure.html image="https://vikasgola.github.io/assets/2020-02-01-hacking-android-with-shared-preferences-data-data.jpg" caption="/data/data/ directory" %}

1. Open folder of any application that you want to explore and open “shared_prefs” folder(if it does not exist try to use that app a little more and it would be created eventually.) in it. Final path would be “/data/data/io.package.name/shared_prefs”.

    {% include elements/figure.html image="https://vikasgola.github.io/assets/2020-02-01-hacking-android-with-shared-preferences-shared-pref.jpg" caption="shared preference directory" %}

    This folder contains all the Shared Preferences data in XML files related to app whose folder it is. Every XML file contains number of key-value pairs. 
    These files contain all the hidden configuration, non-hidden configuration, cookies of app, and most of the things which an app needs to store to work properly. It may include boolean values for verification of the membership or for verification of accessibility of premium features. Some gaming apps might store details like how max have you scored or at which level you are.

    Here is the example of part of an XML file of WhatsApp:-
    ```xml
    <int name="document_limit_mb" value="100" />
    <int name="media_limit_mb" value="16" />
    <int name="status_video_max_duration" value="30" />
    <int name="image_quality" value="80" />
    ```

    It seems now we can send images without decreasing its quality and longer video status in WhatsApp by changing values of above mentioned keys.

2. Force stop the app from app info page whose shared preferences you're going to edit. Then, edit the value of any key in XML file using any text editor and save it.

3. Now, open the app and changes should have been reflected.

Note that this trick might not work on some key-value pair configuration as, they might be confirmed every time from the server and get sets to its correct value which it should have been.

*Warning:- I am not responsible for any damage to your phone or you, do it at your own risk. Also, rooting your phone may void your phone’s warranty.*


{% if page.comments %}
<div id="disqus_thread"></div>
<script>
var disqus_config = function () {
this.page.url = "{{ site.url }}"+"{{ page.url }}";  // Replace PAGE_URL with your page's canonical URL variable
this.page.identifier = "{{ page.id }}"; // Replace PAGE_IDENTIFIER with your page's unique identifier variable
};

(function() { // DON'T EDIT BELOW THIS LINE
var d = document, s = d.createElement('script');
s.src = 'https://vikasgola.disqus.com/embed.js';
s.setAttribute('data-timestamp', +new Date());
(d.head || d.body).appendChild(s);
})();
</script>
<noscript>Please enable JavaScript to view the <a href="https://disqus.com/?ref_noscript">comments powered by Disqus.</a></noscript>
{% endif %}