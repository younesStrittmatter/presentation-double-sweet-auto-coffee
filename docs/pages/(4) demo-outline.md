---
title: Demo - Outline
layout: default
permalink: '/demo-outline/'
heading: 'Demo - Outline'
prev_url: '/motivation/'
next_url: '/double-sweet-coffee/'
index: 4
---
<style>
.tools {
    position: absolute;
    bottom: 4vh;
    font-size: 20pt;
    color: #aaa;
}
</style>
<div id="left" class="content-column" style="overflow: hidden; position: relative">
</div>


<div id="right" class="content-column" >
<div class="clickable text small" id="paradigm">Paradigm - Coffee World</div>
<div class="clickable text small" id="condition">Experimental Design Space - Block Length</div>
<div class="clickable text small" id="runner">Run Experiment - LLM & Human Participants</div>
<div class="clickable text small" id="comparison">Model Comparison - Interpolation vs. Ego Model</div>
</div>

<script>

const clear = () => {
    const preview = document.getElementById('left');
    preview.innerHTML = '';
    let highestTimeoutId = setTimeout(() => {});
    for (let i = 0; i <= highestTimeoutId; i++) {
        clearTimeout(i);
    }

    let highestIntervalId = setInterval(() => {});
    for (let i = 0; i <= highestIntervalId; i++) {
        clearInterval(i);
    }
};

document.getElementById('paradigm').onclick = () => {
    clear();
    const preview = document.getElementById('left');
    preview.innerHTML = '<div class="tools">SweetBean</div>';
    const basePath = "{{ '/assets/images/' | relative_url }}";
    const contexts = [[0, 2, 4, 6, 8], [0, 3, 5, 7, 9], [1, 2, 5, 6, 9], [1, 3, 4, 7, 8]];
    let context_idx = 0;
    let src_idx = -1;
    
    
    window.setInterval(() => {
        const element = document.createElement('div');
        element.style.width = '8vw';
        element.style.height = '8vw';
        
        element.style.border = '1px solid #fff2';
        element.style.position = 'absolute';
        element.style.left = `101%`;
        element.style.top= `50%`;
        element.style.transform = `translateY(-50%)`;
        element.style.background = `#fff`;
        element.innerText = '?';
        element.style.fontSize = '4vw';
        element.style.fontWeight = 'bold';
        element.style.textAlign = 'center';
        element.style.lineHeight = '8vw';
        element.style.color = '#000';
        preview.appendChild(element);
        element.style.transition = 'left 6s linear';
        window.setTimeout(() => {
            src_idx += 1;
            if (src_idx > 4) {
                src_idx = 0;
                context_idx = Math.floor(Math.random()*4);
            }
            const imageIndex = contexts[context_idx][src_idx];
            const imageUrl = `${basePath}${imageIndex}.png`;
            let bg = '#fff';
            if (src_idx === 0) {
                bg = ['#f00', '#0f0', '#00f', '#ff0'][Math.floor(context_idx/2)];
            }
            element.style.background = `${bg} url('${imageUrl}')`;
            element.style.backgroundSize = 'cover';
            element.innerText = '';
            
            element.style.border = '3px solid #fff';
            window.setTimeout(() => {
                element.style.background = `${bg}4 url('${imageUrl}')`;
                element.style.border = '1px solid #fff5';
                element.style.backgroundSize = 'cover';
            }, 1200);
        }, 2000);
    
        window.setTimeout(() => {
            element.style.left = `-11vw`;
            window.setTimeout(() => {
                element.remove();
            }, 6100);
        }, 200);
    }, 1500);
};

document.getElementById('condition').onclick = () => {
    clear();
    const preview = document.getElementById('left');
    preview.innerHTML = '<div class="tools">SweetPea</div>';
    const element = document.createElement('div');
    element.style.width = '80%';
    element.style.height = '10%';
    element.style.background = 'linear-gradient(90deg, #f00 50%, #0f0 50%, #0f0 100%)';
    element.style.backgroundSize = '50% 100%'; 
    
    element.style.boxShadow = '0 0 10px #fff9';
     
    preview.appendChild(element);
    let expanding = true;
    let stripeWidth = 200; 

    const animateStripes = () => {
        if (expanding) {
            stripeWidth /= 2; 
            if (stripeWidth <= 4) {
                expanding = false; 
            }
        } else {
            stripeWidth *= 2; 
            if (stripeWidth >= 100) {
                expanding = true;
            }
        }
        element.style.backgroundSize = `${stripeWidth}% 100%`;
        window.setTimeout(animateStripes, 1500);
    };
    animateStripes();
};

document.getElementById('runner').onclick = () => {
    clear();
    const preview = document.getElementById('left');
    preview.innerHTML = '<div><h3>LLM</h3><div class="text small">SweetBean/AutoRA Experiments interface with LLMs as synthetic participants.</div></div>';
    preview.innerHTML += '<div><h3>Human Participants</h3><div class="text small">SweetBean/AutoRA Experiments also interface with prolific/firebase for running experiments on the web and recruiting participants.</div></div>';};

document.getElementById('comparison').onclick = () => {
    clear();
    const preview = document.getElementById('left');
    preview.innerHTML = '<div><h3>Interpolation</h3><div class="text small">AutoRA: "Interpolates" between conditions.</div></div>';
    preview.innerHTML += '<div><h3>Ego Model</h3><div class="text small">Psyneulink model - Episodic Generalization and Optimization.</div></div>';
};
</script>
