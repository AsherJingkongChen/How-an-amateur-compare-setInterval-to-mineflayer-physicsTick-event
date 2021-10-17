# How-an-amateur-compare-setInterval-to-mineflayer-physicsTick-event
```javascript
try{
    const mineflayer = require('mineflayer'); //see mineflayer set-up in gitHub
    const mathjs = require('mathjs');
    const math = require('math');
    const mytps = require('mineflayer-tps')(mineflayer);
    const myplot = require('nodeplotlib');
    const bot = mineflayer.createBot(botconfig); //must be const
    bot.loadPlugin(mytps);
    try{
        var timelog = [], vartimelog = []; //vartimelog phys
        var k=1, starto;
        bot.on('physicsTick',()=>{
            if(k==1){starto=Date.now()}
            timelog.push(Date.now()-starto);
            if(k==20*1200){ //1200s
                for(i=1;i<k;i++){
                    vartimelog.push(timelog[i]-timelog[i-1]);
                }
                bot.emit('plot');
                console.log(`phys: ${math.sum(vartimelog)/vartimelog.length} ${mathjs.std(vartimelog)}`);
                k+=1;
            }
            k+=1;
        })
        var cc=1, aa, ss, whattps=[], intlog=[], varintlog=[]; //varintlog interval
        aa = setInterval(() => {
            if(cc==1){ss=Date.now()}
            intlog.push(Date.now()-ss);
            if(cc>=1){whattps.push(bot.getTps())}
            if(cc==20*1200){ //1200s
                for(j=1;j<cc;j++){
                    varintlog.push(intlog[j]-intlog[j-1]);
                }
                console.log(`tps: ${math.sum(whattps)/whattps.length}`);
                console.log(`intv: ${math.sum(varintlog)/varintlog.length} ${mathjs.std(varintlog)}`);
                bot.emit('ploti')
                clearInterval(aa);
            }
            cc+=1;
        }, 50);
        let range = n => Array.from(Array(n).keys()) //def
        var plotx = range(100+1).slice(1) //def
        var ploty = []
        var plotyi = []
        var plotyt = []
        function coco(cocoarr, cocopara){return cocoarr.filter(x => x==cocopara).length} //def
        bot.on('plot',()=>{
            for(p=1;p<=100;p++){
                ploty.push(coco(vartimelog, p));
            }
            plotdata = [{x:plotx,y:ploty,type:"line",name:"physicstick"}];
            myplot.plot(plotdata);
        })
        bot.on('ploti',()=>{
            for(q=1;q<=100;q++){
                plotyi.push(coco(varintlog, q));
            }
            for(q=1;q<=100;q++){
                plotyi.push(coco(whattps, q));
            }
            plotdatai = [{x:plotx,y:plotyi,type:"line",name:"setinterval"}];
            plotdatat = [{x:plotx,y:plotyt,type:"line",name:"tps"}];
            myplot.stack(plotdatai);
            myplot.stack(plotdatat); //tps + interval||physicstick
            myplot.plot();
        })
    }catch(a){console.log(a)}
}catch(b){console.log(b)}
//I don't know how to prettify and make the result more accurate
//Below is this rotten code's result
```
![無標題10_20211017213942](https://user-images.githubusercontent.com/37398747/137630929-8507e478-827f-4344-b4ec-dc19754db913.png)
