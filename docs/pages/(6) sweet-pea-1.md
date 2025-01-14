---
title: SweetPea
layout: default
permalink: '/sweet-pea-1/'
heading: 'SweetPea'
prev_url: '/double-sweet-coffee/'
next_url: '/sweet-pea-2/'
index: 6
---
<style>
.exp {
    text-shadow: none;
}
</style>

<div id="left" class="content-column exp">
</div>
<div id="right" class="content-column">
<div>
<h3>Why is counterbalancing important?</h3>
<div class="text small">The congruency effect was found almost 100 years ago (Stroop, 1935). It has probably thousands of replications.</div>
</div>
<div>
<h3>BUT:</h3>
<div class="text small emph">Here, we disprove it with a simple experiment</div>
</div>
<div>
<div class="text small clickable" id="start stroop">Say the COLOR as fast as possible</div>
</div>


<script>
document.getElementById("start stroop").onclick = () => {
    var text = ["3", "2", "1", "START", "BLUE", "GREEN", "YELLOW", "GREEN", "PURPLE", "BROWN", "PINK", "BLUE", "ORANGE"];
    var color = ["white", "white", "white", "white", "red", "red", "red", "red", "red","red", "red", "blue", "red"];
    var index = 0;
    const exp = document.getElementById('left');
    const interval = () => {
            exp.innerHTML = text[index];
            exp.style.color = color[index];
            index++;
            if (index>=text.length) {index=4;}
            };
    window.setInterval(interval, 1500);
};
</script>

