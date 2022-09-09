<template>
  <div class="stage" @click="pop"></div>
</template>

<script>
import * as PIXI from "pixi.js";
import anime from "animejs";

export default {
  name: "CanvasComponent",
  data() {
    return {
      imageList:[
      { name: 'test1', url: 'https://uploads.codesandbox.io/uploads/user/4bd37c15-6b32-410f-8bcf-8c41cf1c3a19/9Rrp-2.jpg', onComplete: function () {} },
      { name: 'test2', url: 'https://uploads.codesandbox.io/uploads/user/4bd37c15-6b32-410f-8bcf-8c41cf1c3a19/4k4V-1.jpg', onComplete: function () {} },
      ],
      loader: null,
      app: null,
      colors: ["75F4F4", "90E0F3", "B8B3E9", "D999B9"],
    };
  },
  mounted() {
    this.init();
  },
  methods: {
    init() {
      this.loader = new PIXI.Loader();
      this.loader.add(this.imageList).load(this.setUp);
      this.loader.onProgress.add((val) => {});
      this.loader.onError.add((val) => {
        console.log('error',val)
      });
    },
    setUp() {
      this.app = new PIXI.Application({
        transparent: true,
        antialias: true,
      });
      this.$el.appendChild(this.app.view);
      this.app.renderer.view.style.display = "block";
      this.app.renderer.autoResize = true;
      this.app.renderer.resize(window.innerWidth, window.innerHeight);

     

      let list = this.imageList.reduce((result,current,index)=>{
         let texture =
           PIXI.utils.TextureCache[
           current.name
          ];
          let rectangle = new PIXI.Rectangle(0,0,400,400);
          let textrue = new PIXI.Texture(texture,rectangle)
          result.push(textrue)
          return result
          
      },[])
      let animatedSprite = new PIXI.AnimatedSprite(list)
      animatedSprite.position.set(0,0);
      animatedSprite.animationSpeed = 0.1;
      animatedSprite.play()
      this.app.stage.addChild(animatedSprite)
      //  let sprite = new PIXI.Sprite.from(this.imageList[10]);
      // sprite.position.set(100, 100);
      // sprite.anchor.set(0.5);
      // console.log(this.loader.resources);
      
  
      // this.app.stage.addChild(sprite);
    },
    pop(event) {
      let mouse_x = event.x,
        mouse_y = event.y,
        particles = [],
        spread = 100;

      for (let i = 0; i < 50; i++) {
        let particle = new PIXI.Graphics();
        let rand = anime.random(1, this.colors.length);
        particle.beginFill("0x" + this.colors[rand - 1]);
        particle.drawCircle(0, 0, 4);
        particle.endFill();
        particle.x = mouse_x;
        particle.y = mouse_y;
        particles.push(particle);
        this.app.stage.addChild(particle);
      }

      anime({
        targets: particles,
        x() {
          return anime.random(mouse_x - spread, mouse_x + spread);
        },
        y() {
          return anime.random(mouse_y - spread, mouse_y + spread);
        },
        alpha: { delay: 300, duration: 500, value: 0 },
        easing: "easeOutQuint",
        complete: () => {
          for (let particle of particles) {
            this.app.stage.removeChild(particle);
          }
        },
      });
    },
  },
};
</script>
