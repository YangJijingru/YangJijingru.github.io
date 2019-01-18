---
layout: page
title: "Search"
header-img: "img/player.jpg"
---

<h3>Search</h3>
<hr>
{% include search-lunr.html %}

<script>
  (function() {
    var cx = '013752017740833085071:4zpbdlgpefk';
    var gcse = document.createElement('script');
    gcse.type = 'text/javascript';
    gcse.async = true;
    gcse.src = 'https://cse.google.com/cse.js?cx=' + cx;
    var s = document.getElementsByTagName('script')[0];
    s.parentNode.insertBefore(gcse, s);
  })();
</script>
<gcse:search></gcse:search>