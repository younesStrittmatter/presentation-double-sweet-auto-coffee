---
title: SweetBean
layout: default
permalink: '/sweet-bean-1/'
heading: 'SweetBean'
prev_url: '/sweet-pea-3/'
next_url: '/sweet-bean-2/'
index: 9
---


<img src="{{ '/assets/images/sb-js.png' | relative_url }}" class="visualisation" style="width: 100vw" id="sb">

<script>
document.getElementById('sb').onclick = () => {
    const image = document.getElementById('sb');
    image.src = "{{ '/assets/images/sb-py.png' | relative_url }}";
    image.style.width = '30vw';
    image.style.height = '50vh';    
    image.style.margin = '4vh 38vw';
    
}
</script>

