---
title: Double Sweet Coffee
layout: default
permalink: '/double-sweet-coffee/'
heading: ''
prev_url: '/demo-outline/'
next_url: '/loop/'
index: 5
---
<style>
* {
    margin: 0;
    padding: 0;
}

#full-sequence {
    position: absolute;
    top: 5vh;
    left: 0;
    border-top: white 2px solid;
    border-bottom: white 2px solid;
    width: 100%;
    height: 20vh;
    overflow: hidden;
}

#full-sequence * {
    text-shadow: none;
}

#stimulus-loop {
    opacity: 0;
    position: fixed;
    top: 61.5vh;
    left: 50vw;
    width: 50vw;
    height: 30vh;
    border: 1pt solid #fff3;
    border-radius: 50%;
    transform: translate(-50%, -50%);
    transition: opacity 2s;
    text-align: center;
    line-height: 30vh;
    font-size: 14pt;
    color: yellow;
    overflow: visible;
    
}

#trial-sequence {
    opacity: 0;
    position: absolute;
    top: 36vh;
    transform: translateY(-50%);
    transition: opacity 2s;
    width: 100%;
    line-height: 0;
    border-bottom: 1pt solid #fff3;
    overflow: visible;
}

.empty {
    box-sizing: border-box;
    display: inline-block;
    position: absolute;
    width: 20vh;
    line-height: 2vh;
    
    text-align: center;
    overflow: hidden;
    
    font-size: 10pt;
}

#sweetPea {
    opacity: 0;
    position: fixed;
    transition: opacity 2s;
    top: 36vh;
    transform: translateY(-50%);
    background: black;
    box-shadow: 0 0 2vh 1vh black;
    font-size: 14pt;
    color: yellow;
}

.trial {
    box-sizing: border-box;
    position: absolute;
    display: inline-flex;
    transition: linear left 400ms, linear top 400ms;
    width: 80vh;
    height: 20vh;
}

.trial_feature {
    box-sizing: border-box;
    position: absolute;
    display: inline-flex;
    transition: linear left 400ms, linear top 400ms;
    width: 80vh;
    height: 20vh;
}

.trial:hover {
    background: #fff2;
}

.event {
    box-sizing: border-box;
    display: inline-block;
    position: absolute;
    top: 2vh;
    width: 20vh;
    height: 16vh;
    line-height: 16vh;
    text-align: center;
    border: 1pt #fff5 solid;
    overflow: hidden;
}

.fixation {
    font-size: 4vh;
}

.feedback {
    font-size: 2vh;
}

.dot {
    position: absolute;
    width: 2px;
    height: 2px;
    background-color: white;
    border-radius: 50%;
}

.rdp {
    position: absolute; 
}
.rdp_img {
    position: absolute;
    width: 20vh;
    height: 16vh;
    background-size: cover;
    
}

.rdp_l {
    position: absolute;
    width: 8vh;
    height: 8vh;
    line-height: 8vh;
    overflow: hidden;
    border: #fff8 solid 1px;
    top: 4vh;
    font-size: 12pt;
}
</style>


<div id="full-sequence"></div>
<div id="trial-sequence"></div>
<div id="stimulus-loop">sweetBean</div>
<div id="sweetPea">sweetPea</div>

<script>
let trials = [];
let trial_features = [];

const TRIAL_WIDTH = 20 * 4;
const SPEED_LIN = TRIAL_WIDTH / 40;
const SPEED_CYCLE = -Math.PI / 20;
let START = 0;

const H = document.documentElement.clientHeight;
const W = document.documentElement.clientWidth;
const R = W / H;
START = 44 * R;

const trial_list = [
    {'state': 0, 'distractor': null},
];

window.onresize = () => {
    location.reload()
};


const full_sequence = document.getElementById('full-sequence');
const trial_sequence = document.getElementById('trial-sequence');
const stimulus_loop = document.getElementById('stimulus-loop');

let is_full_active = true;



const update = () => {
    
    
    if (is_full_active) {
        update_full();
        update_cycle();
        update_trial_sequence();
    }
};

full_sequence.onclick = () => {
    stimulus_loop.style.opacity = '100%';
    trial_sequence.style.opacity = '100%';
    document.getElementById('sweetPea').style.opacity = '100%';
    
    full_sequence.style.opacity = '37%';
};

function ankle_to_xy(a) {
    return [Math.cos(a), Math.sin(a)]
}

let fixation_loop = fixation(stimulus_loop);
fixation_loop.style.transform = 'translate(-50%, -50%) scale(.75)';
fixation_loop.style.transition = 'linear left 200ms, linear top 200ms';
let fixation_ankle = 1.75 * Math.PI;

let soa_loop = soa(stimulus_loop);
soa_loop.style.transform = 'translate(-50%, -50%) scale(.75)';
soa_loop.style.transition = 'linear left 200ms, linear top 200ms';

let rdp_loop = rdp_l(stimulus_loop);
rdp_loop.style.transform = 'translate(-50%, -50%) scale(.75)';
rdp_loop.style.transition = 'linear left 200ms, linear top 200ms';

let feedback_loop = feedback_l(stimulus_loop);
feedback_loop.style.transform = 'translate(-50%, -50%) scale(.75)';
feedback_loop.style.transition = 'linear left 200ms, linear top 200ms';


function update_cycle() {
    fixation_ankle += SPEED_CYCLE;
    if (fixation_ankle > Math.PI * 2) {
        fixation_ankle -= Math.PI * 2
    }
    if (fixation_ankle < 0) {
        fixation_ankle += Math.PI * 2
    }
    let pos_f = ankle_to_xy(fixation_ankle);
    let x_f = pos_f[0];
    let y_f = pos_f[1];
    let pos_soa = ankle_to_xy(fixation_ankle + Math.PI / 2);
    let x_s = pos_soa[0];
    let y_s = pos_soa[1];
    let pos_rdp = ankle_to_xy(fixation_ankle + Math.PI);
    let x_rdp = pos_rdp[0];
    let y_rdp = pos_rdp[1];
    let pos_fe = ankle_to_xy(fixation_ankle + 3 * Math.PI / 2);
    let x_fe = pos_fe[0];
    let y_fe = pos_fe[1];
    fixation_loop.style.left = `${25 + 25 * x_f}vw`;
    fixation_loop.style.top = `${15 + 15 * y_f}vh`;
    soa_loop.style.left = `${25 + 25 * x_s}vw`;
    soa_loop.style.top = `${15 + 15 * y_s}vh`;
    rdp_loop.style.left = `${25 + 25 * x_rdp}vw`;
    rdp_loop.style.top = `${15 + 15 * y_rdp}vh`;
    feedback_loop.style.left = `${25 + 25 * x_fe}vw`;
    feedback_loop.style.top = `${15 + 15 * y_fe}vh`;
}


const update_full = () => {
    trials.forEach((trial, i) => {
        trial[1] -= SPEED_LIN;
        trial[0].style.left = `${trial[1]}vh`;
        const wi = document.documentElement.clientWidth;
        const hi = document.documentElement.clientHeight;

        const id = trial[0].id;
        const t_ = document.getElementById(id);
        const children = t_.children;
        for (const child of t_.children) {
            const bound = child.getBoundingClientRect();
            const x = bound['x'] + bound['width'] / 2;
            if ((x < wi / 2 + .1 * hi) && (x > wi / 2 - .1 * hi)) {
                child.style.background = '#222';
                
                if (child.classList.contains('fixation')) {
                    fixation_loop.style.background = '#222'
                } else {
                    fixation_loop.style.background = '#000'
                }
                if (child.classList.contains('soa')) {
                    soa_loop.style.background = '#222'
                } else {
                    soa_loop.style.background = '#000'
                }
                if (child.classList.contains('rdp')) {
                    child.style.background = '#fff'
                } else {
                    rdp_loop.style.background = '#000'
                }
                if (child.classList.contains('feedback')) {
                    feedback_loop.style.background = '#222'
                } else {
                    feedback_loop.style.background = '#000'
                }
            } else {
                if (child.classList.contains('rdp')) {
                    child.style.background = '#999'
                } else {
                    child.style.background = '#000'
                }
            }
        }
    });
    trials.forEach((trial, i) => {
        if (trial[1] < -TRIAL_WIDTH - 10) {
            if (trial[0] && trial[0].parentNode) {
                trial[0].parentNode.removeChild(trial[0]);
            }
            trials.splice(i, 1);
        }
    });
    if (trials.length < 8) {
        append_trial(trials)
    }
};

const update_trial_sequence = () => {
    trial_features.forEach((trial, i) => {
        trial[1] -= SPEED_LIN;
        trial[0].style.left = `${trial[1]}vh`
    });
    trial_features.forEach((trial, i) => {
        if (trial[1] < -TRIAL_WIDTH - 10) {
            if (trial[0] && trial[0].parentNode) {
                trial[0].parentNode.removeChild(trial[0]);
            }
            trial_features.splice(i, 1);
        }
    });
    if (trial_features.length < 8) {
        append_feature(trial_features)
    }
};
window.setInterval(update, 200);
while (trials.length < 8) {
    append_trial(trials)
}
while (trial_features.length < 8) {
    append_feature(trial_features)
}

update();


function fixation(parent) {
    const div = document.createElement('div');
    div.innerText = '+';
    div.classList.add('event');
    div.classList.add('fixation');
    div.style.left = 0;
    parent.appendChild(div);
    return div
}

function soa(parent) {
    const div = document.createElement('div');
    div.classList.add('event');
    div.classList.add('soa');
    div.style.left = '20vh';
    parent.appendChild(div);

    return div
}

function rdp(parent, state, distractor) {
    const div_outer = document.createElement('div');
    
    div_outer.classList.add('event');
    div_outer.classList.add('rdp');
    div_outer.style.background = 'white';
    div_outer.style.left = '40vh';
    const basePath = "{{ '/assets/images/' | relative_url }}";
    
    if (distractor === null) {
    const img = document.createElement('div');
    img.classList.add('rdp_img');
    let imageUrl = `${basePath}${state}.png`;
    img.style.background = `url('${imageUrl}')`;
    img.style.backgroundSize = '6vh 6vh';
    img.style.width = '20vh';
    img.style.height = '16vh';
    img.style.backgroundRepeat = 'no-repeat';
    img.style.backgroundPosition = '7vh 5vh';
    div_outer.appendChild(img);
} else {
    const img = document.createElement('div');
    img.classList.add('rdp_img');
    let imageUrl_2 = `${basePath}${state}.png`;
    img.style.background = `url('${imageUrl_2}')`;
    img.style.backgroundSize = '5vh 5vh';
    img.style.width = '20vh';
    img.style.height = '16vh';
    img.style.backgroundRepeat = 'no-repeat';
    img.style.backgroundPosition = '2vh 5.5vh';
    div_outer.appendChild(img);
    const dist = document.createElement('div');
    
    dist.classList.add('rdp_img');
    const imageUrl_d = `${basePath}${distractor}.png`;
    dist.style.background = `url('${imageUrl_d}')`;
    dist.style.backgroundSize = '5vh 5vh';
    dist.style.width = '20vh';
    dist.style.height = '16vh';
    dist.style.backgroundRepeat = 'no-repeat';
    dist.style.backgroundPosition = '13vh 5.5vh';
    div_outer.appendChild(dist);
}
    parent.append(div_outer);
    return div_outer
}

function rdp_l(parent) {
    const div_outer = document.createElement('div');
    
    div_outer.classList.add('event');
    div_outer.style.left = '40vh';
    const div_l = document.createElement('div');
    div_l.classList.add('rdp_l');
    div_l.style.left = '.5vh';
    div_l.innerText = '<State>';
    
    div_outer.appendChild(div_l);
    const div_r = document.createElement('div');
    div_r.classList.add('rdp_l');
    div_r.style.right = '.5vh';
    div_r.innerText = '<query>';
    div_outer.appendChild(div_r);
    parent.appendChild(div_outer);
    return div_outer
}



function feedback(parent, is_query) {
    const div = document.createElement('div');
    div.classList.add('event');
    div.classList.add('feedback');
    div.style.left = '60vh';
    const is_correct = Math.random() < .75;
    if (is_query) {
        
    
    div.innerText = is_correct ? 'CORRECT' : 'FALSE';
    div.style.color = is_correct ? 'green' : 'red';
}
    parent.append(div);
    return div
}

function feedback_l(parent) {
    const div = document.createElement('div');
    div.classList.add('event');
    div.classList.add('feedback');
    div.style.left = '60vh';
    div.innerText = '<feedback>';
    parent.append(div);
    return div;
}

function empty(parent, pos) {
    const div = document.createElement('div');
    div.classList.add('empty');
    div.style.left = `{pos}vh`;
    parent.append(div);
    return div;
}

function rdk_feature(parent, state, is_query) {
    const div = document.createElement('div');
    div.classList.add('empty');
    div.style.left = `40vh`;
    div.innerText = `{state:${state}, is_query:${is_query}}`;
    parent.append(div);
    return div
}

function trial(parent, left) {
    const container = document.createElement('div');
    container.classList.add('trial');
    const f = fixation(container);
    const s = soa(container);
    let is_query = Math.random() < .3;
    let state = Math.floor(Math.random() * 10);
    let dist = null;
    if (is_query) {
        dist = Math.floor(Math.random() * 10);
        while (dist === state) {
            dist = Math.floor(Math.random() * 10);
        }
    }
    const r = rdp(container, state, dist);
    const fe = feedback(container, is_query);
    container.style.left = `${left}vh`;
    parent.appendChild(container);
    container.id = Math.random().toString();
    return [container, left];
}

function trial_feature(parent, left) {
    const container = document.createElement('div');
    container.classList.add('trial_feature');
    const f = empty(container, 0);
    const s = empty(container, 20);
    let is_query = Math.random() < .3;
    let state = Math.floor(Math.random() * 10);
    let dist = null;
    if (is_query) {
        dist = Math.floor(Math.random() * 10);
        while (dist === state) {
            dist = Math.floor(Math.random() * 10);
        }
    }
    const r = rdk_feature(container, state, is_query);
    const fe = empty(container, 60);
    container.style.left = `${left}vh`;
    parent.appendChild(container);
    container.id = Math.random().toString();
    return [container, left]
}

function append_trial(lst) {
    if (lst.length <= 0) {
        lst.push(trial(full_sequence, START))
    } else {
        const last_trial = lst[lst.length - 1];
        const last_left = last_trial[1];
        lst.push(trial(full_sequence, TRIAL_WIDTH + last_left))
    }
}

function append_feature(lst) {
    if (lst.length <= 0) {
        lst.push(trial_feature(trial_sequence, START))
    } else {
        const last_trial = lst[lst.length - 1];
        const last_left = last_trial[1];
        lst.push(trial_feature(trial_sequence, TRIAL_WIDTH + last_left))
    }
}


function createDots(container, numberOfDots) {
    for (let i = 0; i < numberOfDots; i++) {
        const dot = document.createElement('div');
        dot.className = 'dot';
        dot.style.left = `${Math.random() * 8}vh`;
        dot.style.top = `${Math.random() * 8}vh`;
        container.appendChild(dot);
    }
}

</script>