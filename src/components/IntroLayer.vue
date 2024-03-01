<script>

export default {
  name: "IntroLayer",

  methods: {
    open() {
      const top = document.querySelector('.top')
      const bottom = document.querySelector('.bottom')
      const center = document.querySelector('.center .button')
      const introLayer = document.querySelector('.intro-layer')
            
      top.style.top = '-150px';
      bottom.style.bottom = '-150px';
      
      center.style.backgroundColor = '#ffcc01';
      center.style.scale = '1.2';

      introLayer.style.opacity = '0';
      
      setTimeout(() => {
        introLayer.style.display = 'none';
      }, 3000);
    }  
  },
};

</script>


<template>

  <div class="intro-layer">
    <div class="pokeball">
      <div class="top">
        <img class="title" src="../assets/logo-pokemon.png" alt="logo-pokemon">
      </div>
      <div class="bottom">
        <div class="center">
          <div class="button" @click="open"></div>
        </div>
      </div>
    </div>
  </div>

</template>


<style lang="scss" scoped>
.intro-layer {
  overflow: hidden;
  position: absolute;
  z-index: 100;
  top: 0;
  left: 0;
  bottom: 0;
  right: 0;
  background-color: transparent;
  display: flex;
  justify-content: center;
  align-items: center;
  flex-direction: column;
  background-color: #fff;
  transition: 3s ease-in;

  .pokeball {
    position: relative;
    width: 400px;
    height: 400px;

    .top,
    .bottom {
      position: absolute;
      left: 0;
      height: 210px;
      width: 400px;
      border: 20px solid #333;
    }
    
    .top {
      z-index: 101;
      top: 0;
      border-top-left-radius: 210px;
      border-top-right-radius: 210px;
      background-color: #e0021f; 
      transition: top 3s ease-in-out;

      .title {
        height: 200px;
        position: absolute;
        z-index: 104;
        top: -70px;
        left: 50%;
        transform: translateX(-50%);
      }
    }
    
    .bottom {
      z-index: 102;
      bottom: 0;
      border-bottom-left-radius: 210px;
      border-bottom-right-radius: 210px;    
      background-color: #fff; 
      transition: bottom 3s ease-in-out;
      .center {
        height: 130px;
        width: 130px;
        position: absolute;
        z-index: 103;
        top: 0;
        left: 50%;
        transform: translate(-50%, -50%);
        border-radius: 50%;
        border: 20px solid #333;
        background-color: white;
        display: flex;
        justify-content: center;
        align-items: center;
  
        .button {
          height: 50px;
          width: 50px;
          border-radius: 50%;
          background-color: #333;
          transition: 0.3s;
  
          &:hover {
            background-color: #ffcc01;
            scale: 1.2;
            cursor: pointer;
          }
        }
  
      }
    }
  }
}

</style>