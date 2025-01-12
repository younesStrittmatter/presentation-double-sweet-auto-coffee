---
title: Research Loop
layout: default
permalink: '/loop/'
heading: 'AutoRA: Research <span id="loop">"Loop"</span>'
prev_url: '/motivation/'
next_url: '/loop/'
index: 3
---

<style>
.agent {
position: absolute;
top: 50%;
left: 50%;
background: red;
}
.box {
padding: 1vh 3vw;
border-radius: 5px;
background: #222;
border: 2px solid #222;
color: #fff6;
text-shadow: 0 0 1px #fff2;
box-shadow: 0 0 30px 10px #000;
}
.activated {
color: #fff;
}
.box:hover {
color: #fff;
text-shadow: 0 0 1px #fff2;
cursor: pointer;
}
#theorist {
transform: translate(calc(-17.3vh - 50%), calc(-10vh - 50%));
}
#experimentalist {
transform: translate(calc(17.3vh - 50%), calc(-10vh - 50%));
}
#runner {
transform: translate(-50%, calc(20vh - 50%));
}


#circle {
overflow: visible;
position: relative;
    width: 40vh;
    height: 40vh;
    border-radius: 50%;
    border: 1px solid #fff2;
    }
#data {
padding: 2px 5px;
background: #111;
border-radius: 3px;
border: 1px solid #222;
color: #fff1;
text-shadow: 0 0 1px #fff2;
}
#left {
display: flex;
flex-direction: column;
}
</style>
<div id="left" class="content-column">
</div>
<div id="right" class="content-column">
<div id="circle">
<div id="data" class="agent text extra-small"></div>
<div id="theorist" class="agent box text small">Theorist</div>
<div id=experimentalist class="agent box text small">Experimentalist</div>
<div id=runner class="agent text box small">Runner</div>

</div>
</div>

<script>
const setLeft = (text) => {
    let input = '';
    let output = '';
    let example = '';
    if (text === 'Theorist') {
        input = '<h3>Input</h3><div class="text small">Experimental Data</div>';
        output = '<h3>Output</h3><div class="text small">(Fitted) Model</div>';
        example = '<h3>Example</h3><div class="text small">Linear Regression<br>Neural Network</div>';
    } else if (text === 'Experimentalist') {
        input = '<h3>Input</h3><div class="text small">Model + Experimental Data</div>';
        output = '<h3>Output</h3><div class="text small">Experiment (Condition)</div>';
        example = '<h3>Example</h3><div class="text small">Uncertainty Sampling<br>Random Sampling</div>';
    } else {
        input = '<h3>Input</h3><div class="text small">Experiment</div>';
        output = '<h3>Output</h3><div class="text small">Experimental Data</div>';
        example = '<h3>Example</h3><div class="text small">Web-Based Experiment<br>Simulations</div>';
    }
    document.getElementById('left').innerHTML = `<div>${input}</div><div>${output}</div><div>${example}</div>`;
};


document.getElementById('theorist').onclick = (() => {
    document.getElementById('theorist').classList.add('activated');
    if (document.getElementById('experimentalist').classList.contains('activated')) {
        document.getElementById('experimentalist').classList.remove('activated');
    }
    if (document.getElementById('runner').classList.contains('activated')) {
        document.getElementById('runner').classList.remove('activated');
    }
    setLeft('Theorist');
});
document.getElementById('experimentalist').onclick = (() => {
    document.getElementById('experimentalist').classList.add('activated');
    
    if (document.getElementById('theorist').classList.contains('activated')) {
        document.getElementById('theorist').classList.remove('activated');
    }
    if (document.getElementById('runner').classList.contains('activated')) {
        document.getElementById('runner').classList.remove('activated');
    }
    setLeft('Experimentalist');
});
document.getElementById('runner').onclick = (() => {
    document.getElementById('runner').classList.add('activated');
    if (document.getElementById('theorist').classList.contains('activated')) {
        document.getElementById('theorist').classList.remove('activated');
    }
    if (document.getElementById('experimentalist').classList.contains('activated')) {
        document.getElementById('experimentalist').classList.remove('activated');
    }
    setLeft('Runner');
});

document.getElementById('loop').onclick = () => {
    document.getElementById('loop').innerText = 'State';
    const content = document.getElementsByClassName('content-container')[0];
    content.style.position = 'relative';
    content.innerHTML = '';
    
    const createWeb = (x, y) => {
        let startX = x;
        let startY = y;
        let currentX = x;
        let currentY = y;
        let list = [[x, y]];
        let index = 0;  
        let closed = false;
        while (!closed) {
            let nextX = currentX +  (30) * (.5-(Math.random() < .5) ) ;
            let nextY = currentY + (40) * (.5-(Math.random() < .5));
            while (nextX < 20 || nextX > 80 || nextY < 20 || nextY > 80) {
                nextX = currentX +  (30) * (.5-(Math.random() < .5));
                nextY = currentY + (40) * (.5-(Math.random() < .5));
            }
            if (Math.random() < .1) {
                const _lst = createWeb(currentX, currentY);
                list = list.concat(_lst);
            } else if (Math.random() < .1 && index > 3 || index > 6) {
                nextX = startX;
                nextY = startY;
                closed = true;
            } 
            index += 1;
            const line = createLine(currentX, currentY, nextX, nextY, content);
            content.appendChild(line);
            currentX = nextX;
            currentY = nextY;
            list.push([currentX, currentY]);
        }
        return list;
    };
    let startX = Math.random() * 10 + 45;
    let startY = Math.random() * 10 + 45;
    const lst = createWeb(startX, startY);
      

    const movingObject = document.createElement('div');
    movingObject.style.position = 'absolute';
    movingObject.innerHTML = 'State';
    movingObject.style.padding = '1px 3px';
    movingObject.style.background = '#111';
    movingObject.style.borderRadius = '3px';
    movingObject.style.border = '1px solid #222';
    movingObject.style.color = '#fff1';
    movingObject.style.textShadow = '0 0 1px #fff2';
    movingObject.style.fontSize = '12pt';
    
    content.appendChild(movingObject);
    lst.forEach(([x, y]) => {
        const point = document.createElement('div');
        point.style.position = 'absolute';
        point.style.padding = '.25vh 0.75vw';
        point.style.borderRadius = '5px';
        point.style.background = '#222';
        point.style.border = '2px solid #222';
        point.style.left = `${(x / 100) * content.offsetWidth}px`;
        point.style.top = `${(y / 100) * content.offsetHeight}px`;
        point.style.transform = 'translate(-50%, -50%)';
        point.innerHTML = 'AutoRA Agent';
        point.style.color = '#fff6';
        point.style.textShadow = '0 0 1px #fff2';
        point.style.boxShadow = '0 0 5px 2px #000';
        point.style.fontSize = '16pt';
        content.appendChild(point);
    });  
    let currentIndex = 0;
    const moveToNextPoint = () => {
    if (currentIndex >= lst.length - 1) {
      currentIndex = 0;
      setTimeout(moveToNextPoint, 10); 
      return;
    }
    const [x1, y1] = lst[currentIndex];
    const [x2, y2] = lst[currentIndex + 1];

    const x1Px = (x1 / 100) * content.offsetWidth;
    const y1Px = (y1 / 100) * content.offsetHeight;
    const x2Px = (x2 / 100) * content.offsetWidth;
    const y2Px = (y2 / 100) * content.offsetHeight;

    const distance = Math.sqrt((x2Px - x1Px) ** 2 + (y2Px - y1Px) ** 2);
    const duration = distance * 7;

    movingObject.style.transition = `transform ${duration}ms linear`;
    movingObject.style.transform = `translate(calc(${x2Px}px - 50%), calc(${y2Px}px - 50%))`;

    
    currentIndex++;
    setTimeout(moveToNextPoint, duration);
  };

  
  const [X, Y] = lst[0];
  movingObject.style.transform = `translate(calc(${(X / 100) * content.offsetWidth}px - 50%), calc(${(Y / 100) * content.offsetHeight}px - 50%))`;

  
  setTimeout(moveToNextPoint, 100);
};
function createLine(x1, y1, x2, y2, container) {
  const containerWidth = container.offsetWidth;
  const containerHeight = container.offsetHeight;

  const x1Px = (x1 / 100) * containerWidth;
  const y1Px = (y1 / 100) * containerHeight;
  const x2Px = (x2 / 100) * containerWidth;
  const y2Px = (y2 / 100) * containerHeight;

  const length = Math.sqrt((x2Px - x1Px) ** 2 + (y2Px - y1Px) ** 2);
  const angle = Math.atan2(y2Px - y1Px, x2Px - x1Px) * (180 / Math.PI);
  const line = document.createElement('div');
  line.style.position = 'absolute';
  line.style.width = `${length}px`; 
  line.style.height = '1px';
  line.style.backgroundColor = '#222';
  line.style.left = `${x1Px}px`; 
  line.style.top = `${y1Px}px`; 
  line.style.transform = `rotate(${angle}deg)`;
  line.style.transformOrigin = '0 0';
    return line
}

const dataElement = document.getElementById('data');
const start = performance.now();
const animateData = () => {
    const t = (performance.now() - start) / 2000;
    const x = 20 * Math.cos(t);
    const y = 20 * Math.sin(t);
    

    dataElement.style.transform = `translate(calc(${x}vh - 50%), calc(${y}vh - 50%))`;
    const angle = t%(2*Math.PI);
    if (angle < Math.PI/2 || angle > 270) {
        dataElement.innerHTML = 'Experiment';
    } else if (angle > Math.PI/2 && angle < 42*Math.PI/36) {
        dataElement.innerHTML = 'Experimental Data';
    } else if (angle > 42*Math.PI/36 && angle < 66*Math.PI/36) {
        dataElement.innerHTML = 'Model';
    } else {
        dataElement.innerHTML = 'Experiment';
    }
    

    
   

requestAnimationFrame(animateData);
};
requestAnimationFrame(animateData);
</script>