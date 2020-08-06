### takagotch
---

![takagotch](https://github.com/mjpitz/mjpitz/raw/main/pis.jpeg)
![Google Analytics](https://www.google-analytics.com/collect?v=1
&tid=UA-174694405-1
&cid=555
&t=pageview
&ec=repo
&ea=open
&dp=%2F&dt=%2F)


```
// Google analytics
// Event Tracking
https://www.google-analytics.com/collect?
v=1                  // Version.
&tid=UA-174694405-1  // Tracking ID / Property ID.
&cid=555             // Anonymous Client ID.
&t=pageview          // Pageview hit type.
&ec=repo             // Documet hostname.
&ea=open             // Page.
&dp=%2F              // Page.
&dt=%2F              // Title.


// Page Tracking
v=1             // Version.
&tid=UA-XXXX-Y  // Tracking ID / Property ID.
&cid=555        // Anonymous Client ID.
&t=pageview     // Pageview hit type.
&dh=mydemo.com  // Document hostname.
&dp=/home       // Page.
&dt=homepage    // Title.

// Event Tracking
v=1             // Version.
&tid=UA-XXXX-Y  // Tracking ID / Property ID.
&cid=555        // Anonymous Client ID.
&t=event        // Event hit type
&ec=video       // Event Category. Required.
&ea=play        // Event Action. Required.
&el=holiday     // Event label.
&ev=300         // Event value.
```

```html
<!-- Global site tag (gtag.js) - Google Analytics -->
<script async src="https://www.googletagmanager.com/gtag/js?id=UA-174694405-1"></script>
<script>
  window.dataLayer = window.dataLayer || [];
  function gtag(){dataLayer.push(arguments);}
  gtag('js', new Date());

  gtag('config', 'UA-174694405-1');
</script>

```
