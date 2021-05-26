<template>
    <div :style="rulerStyle" id="ruler" class="rulerBlock">
        <div id="scaleBlock" :style="scaleBlockStyle"></div>
        <div id="numBlock" :style="numBlockStyle"></div>
        <div :style="unitStyle">cm</div>
    </div>
</template>


<script>
    let vm;
    export default {
        name: "ruler",
        data() {
            return {
                scaleColor:'#333',
                rulerStyle:{
                  position:'absolute',
                  top:'200px',
                  left:'20px',
                  zIndex:99999,
                  width:'200px',
                  height:'56px',
                  backgroundColor:'#fff',
                },
                numBlockStyle:{
                    display: 'flex',
                     justifyContent: 'space-between',
                     paddingLeft:'2px',
                     width:'180px;'
                },
                scaleBlockStyle:{
                    display: 'flex',
                    //justifyContent: 'space-between',  //刻度绘制方法2
                    paddingLeft:'10px',
                    width:'180px;'
                },
                unitStyle:{
                    fontSize:'12px',
                    color:'#333',
                    paddingLeft:'8px'
                }
            }
        },
        methods: {
            setNormalRuler(params){
               if(params.scaleBaseWidth&&params.scaleNum){
                   vm.rulerStyle.width=(params.scaleBaseWidth*params.scaleNum+20)+'px'
               }
               if(params.scaleColor){
                   vm.scaleColor=params.scaleColor;
                   vm.unitStyle.color=vm.scaleColor
               }
               if(params.rulerBgColor){
                   vm.rulerStyle.backgroundColor=params.rulerBgColor
               }


               vm.unitStyle.paddingLeft='8px';

               let marginRight=(params.scaleBaseWidth*params.scaleNum-(params.scaleNum*10+1))/(params.scaleNum*10);
               // vm.scaleBlockStyle.width=(params.scaleBaseWidth*params.scaleNum+10)+'px';  //刻度绘制方法2

               let html='';
               for(let i=0;i<params.scaleNum*10+1;i++){
                   if(i%10==0){//整厘米时
                       if(i==params.scaleNum*10){ //最后一个刻度不需要设置marginright
                           html+="<div style='margin-right:0;background-color:"+vm.scaleColor+";width:1px;height: 20px;'></div>"
                       }else{
                           html+="<div  style='margin-right: "+marginRight+"px;background-color:"+vm.scaleColor+";width:1px;height: 20px;'></div>"
                       }

                      // html+="<div style='margin-right:0;background-color:"+vm.scaleColor+";width:1px;height: 20px;'></div>" //刻度绘制方法2
                   }else if(i%10==5){//半厘米时
                       html+="<div  style='margin-right: "+marginRight+"px;background-color: "+vm.scaleColor+";width:1px;height: 15px;'></div>"
                      // html+="<div  style='background-color: "+vm.scaleColor+";width:1px;height: 15px;'></div>"  //刻度绘制方法2
                   }else{
                       html+="<div  style='margin-right:"+marginRight+"px;background-color: "+vm.scaleColor+";width:1px;height: 10px;'></div>"
                       //html+="<div  style='background-color: "+vm.scaleColor+";width:1px;height: 10px;'></div>"  //刻度绘制方法2
                   }
               }
               document.getElementById('scaleBlock').innerHTML=html;

                vm.numBlockStyle.width=(params.scaleBaseWidth*params.scaleNum+16)+'px';
               let html_1='';
                for(let i=0;i<params.scaleNum+1;i++){
                    html_1+="<div  style='color: "+vm.scaleColor+";font-size: 12px;text-align: center;width: 16px'>"+i+"</div>"
                }
                document.getElementById('numBlock').innerHTML=html_1;

            },
            setVerticalRuler(params){
                vm.rulerStyle.width='56px';
                if(params.scaleBaseWidth&&params.scaleNum){
                    vm.rulerStyle.height=(params.scaleBaseWidth*params.scaleNum+20)+'px'
                }
                if(params.scaleColor){
                    vm.scaleColor=params.scaleColor
                    vm.unitStyle.color=vm.scaleColor
                }
                if(params.rulerBgColor){
                    vm.rulerStyle.backgroundColor=params.rulerBgColor
                }

                vm.rulerStyle.display='flex';
                vm.scaleBlockStyle.paddingLeft='0px';
                vm.scaleBlockStyle.paddingTop='10px';
                vm.scaleBlockStyle.flexDirection='column';
                vm.numBlockStyle.flexDirection='column';
                vm.numBlockStyle.paddingTop='2px';
                vm.unitStyle.paddingLeft='1px';

                let marginBottom=(params.scaleBaseWidth*params.scaleNum-(params.scaleNum*10+1))/(params.scaleNum*10);

                let html='';
                for(let i=0;i<params.scaleNum*10+1;i++){
                    if(i%10==0){//整厘米时
                        if(i==params.scaleNum*10){ //最后一个刻度不需要设置marginright
                            html+="<div style='margin-bottom:0;background-color:"+vm.scaleColor+";width:20px;height: 1px;'></div>"
                        }else{
                            html+="<div  style='margin-bottom: "+marginBottom+"px;background-color:"+vm.scaleColor+";width:20px;height: 1px;'></div>"
                        }
                    }else if(i%10==5){//半厘米时
                        html+="<div  style='margin-bottom: "+marginBottom+"px;background-color: "+vm.scaleColor+";width:15px;height: 1px;'></div>"
                    }else{
                        html+="<div  style='margin-bottom:"+marginBottom+"px;background-color: "+vm.scaleColor+";width:10px;height: 1px;'></div>"
                    }
                }
                document.getElementById('scaleBlock').innerHTML=html;

                vm.numBlockStyle.height=(params.scaleBaseWidth*params.scaleNum+16)+'px';
                let html_1='';
                for(let i=0;i<params.scaleNum+1;i++){
                    html_1+="<div  style='color: "+vm.scaleColor+";font-size: 12px;text-align: center;height: 16px;transform: rotate(-90deg);vertical-align: center'>"+(params.scaleNum-i)+"</div>"
                }
                document.getElementById('numBlock').innerHTML=html_1;


            }
        },
        components: {},
        mounted() {
            vm=this;
            //设置拖拽
            let drag = document.getElementById('ruler');
            drag.onmousedown = function(event) {
                var event = event || window.event;  //兼容IE浏览器
                //    鼠标点击物体那一刻相对于物体左侧边框的距离=点击时的位置相对于浏览器最左边的距离-物体左边框相对于浏览器最左边的距离
                var diffX = event.clientX - drag.offsetLeft;
                var diffY = event.clientY - drag.offsetTop;
                if (typeof drag.setCapture !== 'undefined') {
                    drag.setCapture();
                }

                document.onmousemove = function(event){
                    var event = event || window.event;
                    var moveX = event.clientX - diffX;
                    var moveY = event.clientY - diffY;
                    if(moveX < 0){
                        moveX = 0
                    }else if(moveX > window.innerWidth - drag.offsetWidth){
                        moveX = window.innerWidth - drag.offsetWidth
                    }
                    if(moveY < 0){
                        moveY = 0
                    }else if(moveY > window.innerHeight - drag.offsetHeight){
                        moveY =  window.innerHeight - drag.offsetHeight
                    }
                    drag.style.left = moveX + 'px';
                    drag.style.top = moveY + 'px'
                }

                document.onmouseup = function(event){
                    this.onmousemove = null;
                    this.onmouseup = null;
                    //修复低版本ie bug
                    if(typeof drag.releaseCapture!='undefined'){
                        drag.releaseCapture();
                    }
                }
            }

        },
        destroyed() {
        }
    }
</script>

<style scoped>
</style>
