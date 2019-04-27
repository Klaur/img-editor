<template>
  <div class="hello">
    <p>canvas</p>
    <canvas id="main" width="500" height="300"></canvas>
    <button @click="exportImg">导出{{this.ratito}}</button>
    <img :src="src" alt="">
  </div>
</template>

<script>
  import {fabric} from 'fabric';
  export default {
    name: 'HelloWorld',
    data() {
      return {
        msg: 'Welcome to Your Vue.js App',
        canvas: null,
        src:'',
        ratito:''
      }
    },
    mounted() {
      this.init()
    },
    methods: {
      init() {
       let image = require('./demo2.jpg')
       let canvas= this.canvas = new fabric.Canvas('main')
        fabric.Image.fromURL(image, (img) => {
          if(img.width>canvas.width&&img.height>canvas.height){

          }else{
            img.set({
              // 通过scale来设置图片大小，这里设置和画布一样大
              scaleX: img.width,
              scaleY: img.height,
            });
            canvas.setWidth(img.width)
            canvas.setHeight(img.height)
          }
          if(img.width/canvas.width>=img.height/canvas.height){
            img.set({
              scaleX:canvas.width/img.width,
              scaleY:canvas.width/img.width,
            });
            this.ratito=canvas.width/img.width
            canvas.setHeight(img.height*(canvas.width/img.width))
          }else{
            img.set({
              scaleX:canvas.height/img.height,
              scaleY:canvas.height/img.height,
            });
            this.ratito=canvas.height/img.height
            canvas.setWidth(img.width*(canvas.width/img.width))
          }

          // img.set({
          //   // 通过scale来设置图片大小，这里设置和画布一样大
          //   scaleX: canvas.width / img.width,
          //   scaleY: canvas.height / img.height,
          // });
          // 设置背景
          canvas.setBackgroundImage(img, canvas.renderAll.bind(canvas));
          const textbox = new fabric.Textbox('这是一段文字', {
            left: 50,
            top: 50,
            width: 150,
            fontSize: 20, // 字体大小
            fontWeight: 800, // 字体粗细
            // fill: 'red', // 字体颜色
            // fontStyle: 'italic',  // 斜体
            // fontFamily: 'Delicious', // 设置字体
            // stroke: 'green', // 描边颜色
            // strokeWidth: 3, // 描边宽度
            hasControls: false,
            borderColor: 'orange',
            editingBorderColor: 'blue' // 点击文字进入编辑状态时的边框颜色
          });
// 添加文字后，如下图
          canvas.add(textbox);
          canvas.renderAll();
        });

      },
      exportImg(){
       this.src= this.canvas.toDataURL({
         multiplier:1/this.ratito,
         left:0,
         top:0,
         width:100,
         height:100
            // width:this.canvas.width/this.ratito,
            // height:this.canvas.height/this.ratito
          })
      },
  }
  }
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style>
  .canvas-container {
    margin: 0 auto;
  }

  canvas {
    border: 1px dashed #bbb;
    margin: 0 auto;
  }
</style>
