---
title: SweetPea
layout: default
permalink: '/sweet-pea-2/'
heading: 'SweetPea'
prev_url: '/sweet-pea-1/'
next_url: '/sweet-pea-3/'
index: 7
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
<h3>Is counterbalancing trivial?</h3>
<div class="text small">The task-switching effect is even older than the congruency effect (Jerslid, 1927) and is extremely robust.</div>
</div>
<div>
<h3>BUT:</h3>
<div class="text small emph">Here, we disprove it with a simple experiment even with counterbalanced switches and repetitions</div>
</div>
<div>
<div class="text small clickable" id="start stroop">Say the COLOR or WORD fast as possible</div>
</div>


<script>
document.getElementById("start stroop").onclick = () => {
    var stim = [
        ["3", "white"], 
        ["2", "white"],
        ["1", "white"],
        ["START", "white"],
        ["COLOR", "white"],["RED", "green"],
        ["WORD", "white"], ["GREEN", "blue"],
        ["WORD", "white"], ["BLUE", "green"],
        ["COLOR", "white"], ["PINK", "blue"],
        ["WORD", "white"], ["BLUE", "red"],
        ["COLOR", "white"], ["YELLOW", "blue"],
        ["WORD", "white"], ["BLUE", "green"],
        ["COLOR", "white"], ["RED", "blue"],
        ["COLOR", "white"], ["ORANGE", "yellow"]];
    var index = 0;
    const exp = document.getElementById('left');
    const interval = () => {
            exp.innerHTML = stim[index][0];
            exp.style.color = stim[index][1];
            index++;
            if (index>=stim.length) {index=4;}
            };
    window.setInterval(interval, 1500);
};
</script>

