<template>
  <div id="app">
    <div class="modal" v-show="modalBlock.status">
      <div class="hi">
        <div class="msg">{{ modalBlock.message }}</div>
        <button v-on:click="start">New Game</button>
      </div>
    </div>
    <div
      class="flip-container"
      ontouchstart="this.classList.toggle('hover');"
      v-on:click="flip"
    >
      <div class="flipper" v-for="(url, index) in data" :key="index">
        <div class="back">
          <img :src="url" alt="image" />
        </div>
        <div class="front"></div>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  name: "app",
  data() {
    return {
      data: [],
      current: "",
      block: true,
      modalBlock: { status: true, message: "Good day!" },
      await: null,
      final: 0
    };
  },

  beforeMount() {
    this.getPhoto();
  },

  methods: {
    start() {
      this.modalBlock.message = "Loading... Wait please";
      this.await.then(res => {
        this.modalBlock.status = false;
        const flipperArr = document.querySelectorAll(".flipper");
        flipperArr.forEach(e => {
          e.classList.toggle("flip");
          setTimeout(() => {
            e.classList.toggle("flip");
            this.block = false;
          }, 3000);
        });
      });
    },
    flip(event) {
      const target = event.target;
      if (!target.classList.contains("front") || this.block) return;
      const parent = target.closest(".flipper");
      const image = parent.querySelector("img");
      parent.classList.toggle("flip");

      if (this.current.currentSrc === image.currentSrc) {
        this.final += 1;
        if (this.final === 14) {
          const otherFlipp = document.querySelectorAll('.flipper');
          otherFlipp.forEach((elem) => {
            if (!elem.classList.contains('.flip')) elem.classList.add('flip');
          })
          setTimeout(() => {
            this.getPhoto();
            this.modalBlock.status = true;
            this.modalBlock.message = 'Congratulation!!! Maybe another one? :)';
            const flipImage = document.querySelectorAll('.flip');
            flipImage.forEach((element) => element.classList.remove('flip'));

          }, 2000)
        }
        this.current = "";
      } else if (this.current === "") {
        this.current = image;
      } else {
        this.block = true;
        setTimeout(() => parent.classList.toggle("flip"), 1000);
        setTimeout(() => {
          this.current.closest(".flipper").classList.toggle("flip");
          this.current = "";
          this.block = false;
        }, 1000);
      }
    },
     getPhoto() {
      this.await = new Promise((resolve, reject) => {
        function randomInteger(min, max) {
          return Math.round(Math.random() * (max - min) + min);
        }

        const set = new Set();

        const reverted = async () => {
          // пихает уникальные значения в сет
          const url = `https://picsum.photos/id/${randomInteger(
            1,
            900
          )}/200/100`;

          const response = await fetch(url);
          if (response.ok) set.add(url);
          if (set.size !== 15) return reverted();
          resolve(set);
        };
        reverted();
      }).then(set => {
        const finalArr = [...set].concat([...set]);
        function shuffle(array) {
          for (let i = array.length - 1; i > 0; i--) {
            let j = Math.floor(Math.random() * (i + 1)); // случайный индекс от 0 до i
            [array[i], array[j]] = [array[j], array[i]];
          }
        }
        shuffle(finalArr);
        this.data = finalArr;
      });
    }
  },
  computed: {}
};
</script>

<style>
.flip-container {
  display: grid;
  grid-template-columns: 1fr 1fr 1fr 1fr 1fr 1fr;
  grid-gap: 1vw;
}
img {
  width: 100%;
  height: 100%;
  object-fit: cover;
}
.flip-container {
  perspective: 1000;
}
.flip {
  transform: rotateY(180deg);
}
.flipper {
  min-height: 100px;
  min-width: 80px;
  transition: 0.8s;
  transform-style: preserve-3d;
  position: relative;
}
.front,
.back {
  backface-visibility: hidden;
  position: absolute;
  top: 0;
  left: 0;
  right: 0;
  bottom: 0;
}
.front {
  background-color: brown;
  z-index: 2;
}
.back {
  background-color: chartreuse;
  transform: rotateY(180deg);
}
.modal {
  position: fixed;
  top: 0;
  bottom: 0;
  right: 0;
  left: 0;
  overflow: hidden;
  overflow-y: auto;
  -webkit-overflow-scrolling: touch;
  display: flex;
  flex-flow: column nowrap;
  justify-content: center;
  align-items: center;
  z-index: 99;
  padding: 30px 0;
  background-color: rgb(5, 5, 5, 0.7);
}
.hi {
  width: 160px;
  height: 100px;
  background-color: white;
  margin-bottom: 250px;
  display: flex;
  flex-wrap: wrap;
}
.msg {
  text-align: center;
  width: 100%;
  height: 2rem;
  margin-top: 10px;
}
.hi button {
  height: 2rem;
  margin-left: auto;
  margin-right: auto;
}
</style>
