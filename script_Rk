
    let deformBtn, resetBtn ;

    let grains = [], nuclei = [];

    const MAX_CANV_SIZE = 600; 
    const MIN_GRAIN_AREA = 60;

    let initialDeform = 0.2;
    let additionalDeformation = 4.5;           

    class Grains {
        constructor() {
            this.x = random(width);
            this.y = random(height);
            this.initialSize = random(40, 60);
            this.angle = radians(random(-5, 5));
            this.color = color(random(32, 100), random(87, 190), random(129, 192));
            this.alpha = 255;
        }

        display(deform) {
            push();
            translate(this.x, this.y); 
            rotate(this.angle); 
            fill(red(this.color), green(this.color), blue(this.color), this.alpha);
            noStroke();
            ellipse(0, 0, this.initialSize / deform, this.initialSize * deform);
            pop(); 
        }

        getBorderPoints(deform) {
            let angle = random(TWO_PI);
            let w = this.initialSize / deform;
            let h = this.initialSize * deform;
            return {
                x: this.x + cos(angle) * w / 2,
                y: this.y + sin(angle) * h / 2
            };
        }
    }

    class Nucleus {
        constructor(x, y) {
            this.x = x;
            this.y = y;
            this.radius = 2;  
            this.maxSize = random(20, 40) 
            this.grothRate = 0.5;
            this.color = color(random(229, 255), random(152, 205), random(155, 178));
        }

        grow() {
            if (this.radius < this.maxSize) {
                this.radius += this.grothRate;  
            } 
        }

        show() {
            fill(this.color);
            noStroke();
            ellipse(this.x, this.y, this.radius * 2);
        }               
    }           

    function setup() {

        createCanvas(Math.min(windowWidth, MAX_CANV_SIZE), Math.min(windowHeight, MAX_CANV_SIZE))
            .style('border', '5px solid lightblue')
            .style('border-radius', '6px');

        createControls();      

        const numGrains = Math.floor((width * height) / MIN_GRAIN_AREA);
        
            for(let i = 0; i < numGrains; i++) {
                grains.push(new Grains());
            }

    }

    function createControls() {
        let controls = createDiv();
        controls.style('position', 'absolute');
        controls.style('top', '8px');
        controls.style('left', '8px');
        controls.style('background', '#EEEEEE');
        controls.style('padding', '5px');
        controls.style('border-radius', '5px');
        controls.style('display', 'flex');

        deformBtn = createButton('Усилить деформацию');
        deformBtn.parent(controls);
        deformBtn.style('font-family', 'Georgia, serif');
        deformBtn.style('background', '#D4BEE4');
        deformBtn.style('border', '1px solid #9B7EBD');
        deformBtn.style('border-radius', '3px');
        deformBtn.style('color', '#3B1E54');
        deformBtn.style('cursor', 'pointer');

        deformBtn.mousePressed(() => {
            if (additionalDeformation > 1) {
                additionalDeformation -= 0.5;
            }
            
            if (initialDeform * additionalDeformation <= 0.2) {
                createNuclei();
            }
        });    
        
        resetBtn = createButton('Перезапустить');
        resetBtn.parent(controls);
        resetBtn.style('font-family', 'Georgia, serif');
        resetBtn.style('background', '#D4BEE4');
        resetBtn.style('border', '1px solid #9B7EBD');
        resetBtn.style('border-radius', '3px');
        resetBtn.style('color', '#3B1E54');
        resetBtn.style('cursor', 'pointer');
        resetBtn.hide();

        resetBtn.mousePressed( () => {resetSim()});
    }

    
    function createNuclei() {
        nuclei = [];
        let totalDeform = getTotalDeform();
        for(let grain of grains) {
            if (random() < 0.2) {
                let pos = grain.getBorderPoints(totalDeform);
                nuclei.push(new Nucleus(pos.x, pos.y));
            }
        }
    }

    function draw() {            
        let totalDeform = getTotalDeform();

        for (let grain of grains) { 
            grain.display(totalDeform); 
            if (nuclei.length) {
                grain.alpha -= 10;
            }
        }            

        if (nuclei.length) {
            for (let nuc of nuclei) {
                nuc.grow();
                nuc.show();
            } 
        } 

        if (nuclei.length && nuclei.every(n => n.radius >= n.maxSize)) {
            resetBtn.show();
        }                       
    }               

    function windowResized() {
        resizeCanvas(Math.min(windowWidth, MAX_CANV_SIZE), Math.min(windowHeight, MAX_CANV_SIZE));
    
        grains.length = 0;
        const numGrains = Math.floor((width * height) / MIN_GRAIN_AREA);            
        for(let i = 0; i < numGrains; i++) {
            grains.push(new Grains());
        }                       
        
        let totalDeform = getTotalDeform();

        if (totalDeform >= 1) {
            nuclei.length = 0;                
            for(let grain of grains) {
                if (random() < 0.2) {
                    let pos = grain.getBorderPoints(totalDeform);
                    nuclei.push(new Nucleus(pos.x, pos.y));
                }
            }
        }
        
    }

    function getTotalDeform() {
        return initialDeform * additionalDeformation;
    }

    function resetSim() {
        nuclei = [];
        grains = [];

        const numGrains = Math.floor((width * height) / MIN_GRAIN_AREA);
        for (let i = 0; i < numGrains; i++) {
            grains.push(new Grains());
        }

        additionalDeformation = 4.5;
        resetBtn.hide();
    }


