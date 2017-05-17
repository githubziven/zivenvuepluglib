<template>
  <div id="transform" style="perspective: 900px">
    <div class="box-container" :style="isActive?containerTransform:''" ref="i">
      <span class="box-container__text" :style="isActive?textTransform:''">{{text}}</span>
    </div>
  </div>
</template>
<script>
export default {
  name:'transform',
  
  data() {
    return {
      text: "3d-transform效果小demo",
      isActive:false,
      containerTransform:
        {transform:'rotate3d(0,0,0,5deg)'}
    ,
      outline: '1px solid transparent',//消除火狐3d效果产生的锯齿
      textTransform: {
        transform: 'transloateX(0px) translateY(0px)'
      }

    }
  },
  mounted: function(){
     document.getElementById("transform").addEventListener("mousemove",this.onTrans3D,false)
     document.getElementById("transform").addEventListener("mouseout",this.onReset,false)
  },
  methods: {
    onTrans3D() {
      console.log(111)
      let rect = this.$refs.i.getBoundingClientRect() //获取card位置属性
      var xl = this.$refs.i.clientWidth / 2 //获取card宽度
      var yl = this.$refs.i.clientHeight / 2 //获取card高度
      var dtop = rect.top //获取card位置的高度
      var dleft = rect.left //获取card位置的宽度
      var mtop = event.clientY //获取鼠标点的位置
      var mleft = event.clientX
      var xm = mleft - dleft //与card之间的相对位置
      var ym = mtop - dtop;
      //通过相对位置算出旋转方向，与旋转角度
      var px = 0,
        py = 0
      px = (yl - ym) / yl
      py = (xm - xl) / xl
      var pz = 0
      let rotate = "rotate3d(" + px + "," + py + "," + pz + ",10deg)"
      var translate = 'translateX(' + py*10+ 'px) translateY(' + (-px*10) + 'px)'
      this.containerTransform.transform=rotate
      this.textTransform.transform=translate
      this.isActive=true
  
    },
    onReset() {
      this.isActive=false
    }
  }
}
</script>
<style lang="scss" scoped>
.box-container {
  position: relative;
  border: 0;
  width: 480px;
  height: 270px;
  transition: all 0.2s;
  background: url('./images/pic03.png');
  transform-origin: 50% 50%;
  &__text {
    position: absolute;
    color: #fff;
    font-size: xx-large;
    font-weight: bold;
    text-shadow: 2px 2px 2px #000;
    bottom: 40px;
    left: 20px;
    transition: all 0.2s;
    z-index: 100;
  }
}
</style>