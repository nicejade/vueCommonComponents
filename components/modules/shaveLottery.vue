<template>
    <div id="shave-lottery">
        <a class="fresh-btn" href='javascript:;' @click='onFreshBtnClick'>请刷新</a>
        <label>已刮开<span id="draw-percent">{{ drawPercent }}</span> 区域。</label>
        <div id="lottery-container"></div>
    </div>
</template>


<script type="text/javascript">
import {Lottery} from './../../plugin/lottery.js'

export default {
	data(){
        return {
            lottery: "default",
            drawPercent: '0%',
            succAreaPercent: 50,
            elementId: 'lottery-container',
            defaultImage: 'http://7xoosr.com1.z0.glb.clouddn.com/encourage.png',
        }
    },
    ready(){
        this.updateLottery()
    },
    props: ['paramsList'],
   	watch: {
        paramsList: function (newVal, oldVal) {
            this.updateLottery()
        }
    },
    methods: {
        getParamsList: function(){
            let params = {
                id: this.elementId,
                cover: this.paramsList.cover || '#CCC',
                coverType: this.paramsList.coverType || 'cover',
                width: this.paramsList.width || 300,
                height: this.paramsList.height || 100,
                lotteryContent: this.paramsList.lotteryContent || this.defaultImage,
                lotteryType: this.paramsList.lotteryType || 'image',
                paintSize: this.paramsList.paintSize || 30,
                callBackFunc: this.onDrawPercentCallback,
            }
            this.succAreaPercent = this.paramsList.succAreaPercent || 50;

            return params
        },

        updateLottery: function(){
            $('#lottery-container').empty()

            var params = this.getParamsList()
            this.lottery = new Lottery( params );
            this.lottery.init( this.paramsList.lotteryContent , this.paramsList.lotteryType );

            let lotteryWidth = this.paramsList.width + 'px'
            let lotteryHeight = this.paramsList.height + 'px'
            $('#lottery-container').css({'width': lotteryWidth, 'height': lotteryHeight })

            this.lottery.init( this.paramsList.lotteryContent , this.paramsList.lotteryType );
        },

        //------------------------------Default Function callBack-----------------------------
        onFreshBtnClick: function(){
            this.drawPercent = '0%';
            this.lottery.init( this.paramsList.lotteryContent , this.paramsList.lotteryType );
        },

        onDrawPercentCallback: function( drawPercent ){
            this.drawPercent = drawPercent + '%'
            if( drawPercent >= this.succAreaPercent  && null != this.paramsList.callBackFunc ){
                this.paramsList.callBackFunc("Okay，刮开的区域占比已超过设定: " + this.drawPercent )
            }
        }
    },
    events: {
    }
}
</script>


<style media="screen">
#shave-lottery{
    margin: auto;
}
.fresh-btn{
    padding: .2% 3%;
    background-color: #999;
    border-radius: 1em;
    border: 1px solid #000;
    font-size: 1em;
    color: #111;
    box-shadow: 0px 0px 5px #222;
}
#lottery-container {
    position:relative;
    width: 100%;
    height: 30%;
    margin: auto;
}
#draw-percent {
    color:#F60;
}
</style>
