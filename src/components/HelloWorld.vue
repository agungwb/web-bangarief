<template>
  <div class="container">
    <transition name="fade">
      <div v-if="story.form.flag" id="myModal" class="modal">
        <!-- Modal content -->
        <div class="modal-content">
        <button class="close-btn" @click="hideFormModal">
          <img class="arrow-prev" v-bind:src="require('@/assets/icon/ic-gray-cross.svg')"/>
        </button>
        <div class="img" v-bind:style="'background-image: url('+require('@/assets/bangarief2.jpeg')+');'">
        </div>
        <!-- <span class="img">
            <img v-bind:src="require('@/assets/bangarief2.jpeg')" />
        </span> -->
        <!-- <img class="img" v-bind:src="require('@/assets/bangarief2.jpeg')" width="400px"/> -->
        <div class="form">
          <div>
            <h1>{{modal.title}}</h1>
          </div>
          <div>
            <div class="label">{{modal.label.author}}</div><br />
            <input v-model="message" v-bind:placeholder="modal.form.author"> <br />
            <div class="label">{{modal.label.title}}</div><br />
            <input v-model="message" v-bind:placeholder="modal.form.title"> <br />
            <div class="label">{{modal.label.story}}</div><br />
            <textarea v-model = "textmessage" v-bind:placeholder="modal.form.story"></textarea> <br />
            <button class="btn">{{modal.form.submit}}</button>
          </div>
        </div>
        </div>
      </div>
    </transition>
    <h3>{{ msg1 }}</h3>
    <h1 class="title">{{ msg2 }}</h1>
    <img v-bind:src="img1" class="main"/>
    <div class="block">
      <p>{{ msg3 }}</p>
      <p>{{ msg4 }}</p>
      <p>{{ msg5 }}</p>
      <p>{{ msg6 }}</p>
    </div>
    <div class="slideshow">
      <div class="prev"
        v-on:mouseover="addOverlayPrevSlideShow"
        v-on:mouseleave="removeOverlayPrevSlideShow"
        v-on:click="prevSlideShowImageHandler" >
        <transition v-bind:name="'slide-'+slideshow.direction">
          <div class="img" v-bind:key="currentSlideShowPos" v-bind:style="'background-image: url('+slideshow.img[prevSlideShowPos]+');'">
            <transition name="fade">
              <div v-bind:class="{overlay:slideshow.overlay.prev}" :key="slideshow.overlay.prev"></div>
            </transition>
          </div>
        </transition>
        <div class="arrow-container">
          <img class="arrow-prev" v-bind:src="require('@/assets/icon/ic-black-arrow.svg')"/>
        </div>
      </div>
      <div class="current">
        <transition v-bind:name="'slide-'+slideshow.direction">
          <div class="img" v-bind:key="currentSlideShowPos" v-bind:style="'background-image: url('+slideshow.img[currentSlideShowPos]+');'"></div>
        </transition>
      </div>
      <div class="next"
        v-on:mouseover="addOverlayNextSlideShow"
        v-on:mouseleave="removeOverlayNextSlideShow"
        v-on:click="nextSlideShowImageHandler" >
        <transition v-bind:name="'slide-'+slideshow.direction">
          <div class="img" v-bind:key="currentSlideShowPos" v-bind:style="'background-image: url('+slideshow.img[nextSlideShowPos]+');'">
            <transition name="fade">
              <div v-bind:class="{overlay:slideshow.overlay.next}" :key="slideshow.overlay.next"></div>
            </transition>
          </div>
        </transition>
        <div class="arrow-container">
          <img class="arrow-next" v-bind:src="require('@/assets/icon/ic-black-arrow.svg')"/>
        </div>
      </div>
    </div>

    <!-- <div>
      <swiper class="swiper" :options="swiperOption">
        <swiper-slide><img v-bind:src="slideshow.img[0]" width="200px"/></swiper-slide>
        <swiper-slide><img v-bind:src="slideshow.img[1]" width="200px"/></swiper-slide>
        <swiper-slide><img v-bind:src="slideshow.img[2]" width="200px"/></swiper-slide>
        <swiper-slide><img v-bind:src="slideshow.img[0]" width="200px"/></swiper-slide>
        <swiper-slide><img v-bind:src="slideshow.img[1]" width="200px"/></swiper-slide>
        <swiper-slide><img v-bind:src="slideshow.img[2]" width="200px"/></swiper-slide>
        <swiper-slide>Slide 7</swiper-slide>
        <swiper-slide>Slide 8</swiper-slide>
        <swiper-slide>Slide 9</swiper-slide>
        <swiper-slide>Slide 10</swiper-slide>
        <div class="swiper-pagination" slot="pagination"></div>
      </swiper>
    </div> -->

    <div class="stories">
      <h2>23 Stories</h2>
      <button class="btn" @click="showFormModal">{{ btn1 }}</button>
      <div v-for="story in story.stories" :key="story.idx" class="story">
        <h3>{{story.title}}</h3>
        <p>
          {{story.story}}
        </p>
        <div class="author">
          {{story.author}}
        </div>
      </div>
      <button class="btn" @click="showFormModal">{{ btn1 }}</button>
    </div>
  </div>
</template>

<script>
import { Swiper, SwiperSlide, directive } from 'vue-awesome-swiper'

// import style (<= Swiper 5.x)
import 'swiper/css/swiper.css'
export default {
  name: 'HelloWorld',
  components: {
    Swiper,
    SwiperSlide
  },
  directives: {
    swiper: directive
  },
  data () {
    return {
      swiperOption: {
        slidesPerView: 'auto',
        centeredSlides: true,
        spaceBetween: 30,
        pagination: {
          el: '.swiper-pagination',
          clickable: true
        }
      },
      img1: require('@/assets/bangarief.jpeg'),
      slideshow: {
        overlay: {
          prev: false,
          next: false
        },
        img: [require('@/assets/slideshow/bangarief1.jpeg'),
          require('@/assets/slideshow/bangarief2.jpeg'),
          require('@/assets/slideshow/bangarief3.jpeg')
        ],
        position: 0,
        length: 3,
        direction: 'left',
        timer: '',
        timeout: ''
      },
      msg1: 'In Loving Memory of',
      msg2: 'Bang Arief Munandar',
      msg3: 'إِنَّا لِلَّٰهِ وَإِنَّا إِلَيْهِ رَاجِعُونَ',
      msg4: 'Wafatnya Bang Arief meninggalkan duka bagi kita semua. Sosok Bang Arief sebagai suami, ayah, kakek, teman, dan guru akan kita rindukan.',
      msg5: 'Selama hidupnya, Bang Arief (alm) pasti memberikan kesan yang mendalam ataupun pesan-pesan pelajaran kepada kita semua. Pesan kebaikan sebagai seorang suami, ayah, guru, teman, dan berbagai peran yang diemban.',
      msg6: 'Untuk mengenang pesan kebaikan beliau, kami mengundang bapak, ibu, murid, dan siapapun yang mengenal beliau untuk berbagi testimoni, cerita, dan pesan tentang Bang Arief (Alm).',
      btn1: 'Add Your Story',
      story: {
        stories: [
          {
            title: 'A Man',
            story: 'Tahun 2008, belasan tahun lalu. Tahun itu bukan tahun yang cukup menyenangkan buat saya. Hidup terasa gini-gini aja, bahkan seperti-nya saya mengalami quarter life crisis yang lebih cepat dibandingkan kawan sebaya. Hingga akhirnya, Muhammad Alkautsar memperkenalkan saya dengan sosok yang satu ini.',
            author: 'Akew',
            date: '01/02/03'
          },
          {
            title: 'A Man',
            story: 'Tahun 2008, belasan tahun lalu. Tahun itu bukan tahun yang cukup menyenangkan buat saya. Hidup terasa gini-gini aja, bahkan seperti-nya saya mengalami quarter life crisis yang lebih cepat dibandingkan kawan sebaya. Hingga akhirnya, Muhammad Alkautsar memperkenalkan saya dengan sosok yang satu ini.',
            author: 'Akew',
            date: '01/02/03'
          },
          {
            title: 'A Man',
            story: 'Tahun 2008, belasan tahun lalu. Tahun itu bukan tahun yang cukup menyenangkan buat saya. Hidup terasa gini-gini aja, bahkan seperti-nya saya mengalami quarter life crisis yang lebih cepat dibandingkan kawan sebaya. Hingga akhirnya, Muhammad Alkautsar memperkenalkan saya dengan sosok yang satu ini.',
            author: 'Akew',
            date: '01/02/03'
          }
        ],
        form: {
          flag: false
        }
      },
      modal: {
        title: 'Tell Your Story with Bang Arief',
        label: {
          author: 'Name',
          title: 'Title',
          story: 'Story'
        },
        form: {
          author: 'Put Your Name',
          title: 'Give It a Title',
          story: 'Tell Your Story ...',
          submit: 'Submit'
        }
      }
    }
  },
  computed: {
    currentSlideShowPos () {
      return this.slideshow.position
    },
    prevSlideShowPos () {
      return (this.slideshow.position - 1) >= 0 ? (this.slideshow.position - 1) : (this.slideshow.length - 1)
    },
    nextSlideShowPos () {
      return (this.slideshow.position + 1) < this.slideshow.length ? (this.slideshow.position + 1) : 0
    }
  },
  methods: {
    addOverlayPrevSlideShow () {
      this.slideshow.overlay.prev = true
      // some code to filter users
    },
    removeOverlayPrevSlideShow () {
      this.slideshow.overlay.prev = false
      // some code to filter users
    },
    addOverlayNextSlideShow () {
      this.slideshow.overlay.next = true
      // some code to filter users
    },
    removeOverlayNextSlideShow () {
      this.slideshow.overlay.next = false
      // some code to filter users
    },
    prevSlideShowImageHandler () {
      clearTimeout(this.slideshow.timeout)
      clearInterval(this.slideshow.timer)
      this.slideshow.timeout = setTimeout(() => {
        this.setTimer()
      }, 5000)
      this.slideshow.direction = 'left'
      this.prevSlideShowImage()
    },
    prevSlideShowImage () {
      this.slideshow.position = (this.slideshow.position - 1) >= 0 ? (this.slideshow.position - 1) : (this.slideshow.length - 1)
    },
    nextSlideShowImageHandler () {
      clearTimeout(this.slideshow.timeout)
      clearInterval(this.slideshow.timer)
      this.slideshow.timeout = setTimeout(() => {
        this.setTimer()
      }, 3000)
      this.slideshow.direction = 'right'
      this.nextSlideShowImage()
    },
    nextSlideShowImage () {
      this.slideshow.position = (this.slideshow.position + 1) < this.slideshow.length ? (this.slideshow.position + 1) : 0
    },
    showFormModal () {
      this.story.form.flag = true
    },
    hideFormModal () {
      this.story.form.flag = false
    },
    setTimer () {
      this.slideshow.timer = setInterval(() => {
        if (this.slideshow.direction === 'right') {
          this.nextSlideShowImage()
        } else {
          this.prevSlideShowImage()
        }
      }, 5000)
    }
  },
  created () {
    this.setTimer()
  }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>

* {
  background-color:rgba(255, 255, 255, 0.5);
  font-family: "Alegreya Sans", ui-sans-serif;
  color: rgb(101, 101, 101);
}

p {
  font-size: 22px;
}

h1, h2, h3, h4, h5, h6{
  font-weight: normal;
}

h1.title {
  font-size: 40px;
  color: rgb(55, 55, 55)
}

img.main {
  border-radius: 50%;
  height: 20rem;
}

.container {
  margin-top: -2rem;
  margin-left: 5%;
  margin-right: 5%;
  padding-top: 2rem;
  padding-bottom: 2rem;
  background-color:rgb(255, 255, 255);
  border-radius: 1rem;
  border: solid 2px rgb(232, 232, 232);
}

.block {
  margin-left: 25%;
  margin-right: 25%;
}

.slideshow{
  display: flex;
  flex-direction: row;
  justify-content: center;
  align-items: center;
  margin-top: 5rem;
  border: solid 2px rgb(232, 232, 232);
  border-radius: 1rem;
  margin-right: 5%;
  margin-left: 5%;
  min-width: 70rem;
  padding: 1rem;
}

/* .slideshow .prev {
  position: relative;
  overflow: hidden;
}

.slideshow .prev .img {
  border-radius: 3pt;
  height: 30rem;
  width: 12rem;
  background-size: cover;
  background-position: right;
} */

.slideshow .next, .slideshow .prev {
  height: 30rem;
  width: 12rem;
  position: relative;
  overflow: hidden;
}

.slideshow .next .img, .slideshow .prev .img {
  position: absolute;
  border-radius: 3pt;
  height: 30rem;
  width: 40rem;
  background-size: cover;
}

.slideshow .prev .img {
  top: 0;
  right:0;
  background-position: center;
}

.slideshow .next .img {
  top: 0;
  left: 0;
  background-position: center;
}

.slideshow .current {
  margin-left: 1rem;
  margin-right: 1rem;
  height: 30rem;
  width: 40rem;
  position: relative;
  overflow: hidden;
}

.slideshow .current .img {
  position: absolute;
  top: 0;
  left: 0;
  bottom: 0;
  right:0;
  border-radius: 3pt;
  height: 100%;
  width: 100%;
  background-size: cover;
  background-position: center;
}

.slideshow .prev, .slideshow .current, .slideshow .next {
  cursor: pointer;
}

.slideshow .img .overlay {
  width: 100%;
  height: 100%;
  background-color:rgba(0, 0, 0, 0.7);
  border-radius: 3pt;
}

.slideshow .prev .arrow-container, .slideshow .next .arrow-container {
  position: absolute;
  height: 3rem;
  width: 3rem;
  margin-top: 13rem;
  margin-left: 5rem;
  border-radius: 50%;
}

.slideshow .prev .arrow-container .arrow-prev {
  margin-top: 33%;
  background-color: transparent;
  transform: rotate(180deg) scale(2);
}

.slideshow .next .arrow-container .arrow-next {
  margin-top: 33%;
  background-color: transparent;
  transform: scale(2)
}

.slideshow .slide-left-enter-active, .slideshow .slide-left-leave-active  {
  transition: 2s;
}

.slideshow .slide-left-enter {
  transform: translate(-100%, 0);
}

.slideshow .slide-left-leave-to /* .fade-leave-active below version 2.1.8 */ {
  transform: translate(100%, 0);
}

.slideshow .slide-right-enter-active, .slideshow .slide-right-leave-active  {
  transition: 2s;
}

.slideshow .slide-right-enter {
  transform: translate(100%, 0);
}

.slideshow .slide-right-leave-to /* .fade-leave-active below version 2.1.8 */ {
  transform: translate(-100%, 0);
}

.stories {
  margin-top: 6rem;
  margin-left: 20%;
  margin-right: 20%;
  border-top: 1px solid rgb(216, 216, 216);
}

.stories h2 {
  margin-top: 5rem;
  margin-bottom: 2rem;
  font-size: 24pt;
  color: rgb(55, 55, 55);
  font-weight: normal;
  font-style: italic;
}

.stories .btn{
  padding-right: 1rem;
  padding-left: 1rem;
  width: 10rem;
  height: 3rem;
  border-radius: 2rem;
  border: 2px solid rgb(216, 216, 216);
  font-size: 12pt;
  font-weight: normal;
}

.stories .btn:hover{
  background-color: rgba(216, 216, 216, 0.5);
  cursor: pointer;
}

.stories .story{
  border: 1px solid rgb(216, 216, 216);
  border-radius: 1rem;
  font-size: 18pt;
  padding-top: 1rem;
  padding-bottom: 1rem;
  padding-right: 2rem;
  padding-left: 2rem;
  margin-top: 2rem;
  margin-bottom: 2rem;
  text-align: left;
}

.stories .story h3{
  font-size: 18pt;
}

.stories .story p{
  font-size: 14pt;
}

.stories .story .author{
  font-size: 13pt;
  font-weight: bold;
}

.modal {
  display: block; /* Hidden by default */
  position: fixed; /* Stay in place */
  z-index: 1; /* Sit on top */
  left: 0;
  top: 0;
  width: 100%; /* Full width */
  height: 100%; /* Full height */
  overflow: auto; /* Enable scroll if needed */
  background-color: rgba(0,0,0,0.8); /* Black w/ opascity */
  animation:fadein 2s;
}

/* Modal Content/Box */
.modal .modal-content {
  display: flex;
  flex-direction: row;
  justify-content: center;
  align-items: center;
  position:relative;
  background-color: #ffffff;
  margin: 5% auto; /* 15% from the top and centered */
  padding: 3rem;
  width: 80%; /* Could be more or less, depending on screen size */
  height: 70%;
  border-radius: 1rem;
  border: solid 2px rgb(232, 232, 232);
}

.modal .modal-content .img{
  /* display: inline-block; */
  /* flex-basis: 30%;
  min-width: 30%; */
  height: 30rem;
  width: 22rem;
  margin-right: 5rem;
  background-size: cover;
  background-position: center;
  border-radius: 1rem;
}

.modal .modal-content .form{
  text-align: left;
}

.modal .modal-content .form .label{
  margin-left: 1rem;
  margin-bottom: -1rem;
  background-color: transparent;
}

.modal .modal-content .form input{
  width: 20rem;
  height: 1rem;
  margin-bottom: 1rem;
  border: 2px solid rgb(216, 216, 216);
  border-radius: 1rem;
  padding: 1rem;
  font-size: 11pt;
  font-family: Arial, Helvetica, sans-serif;
}

.modal .modal-content .form input:focus{
  outline: none;
}

.modal .modal-content .form input:empty{
  color: black;
}

.modal .modal-content .form textarea{
  width: 33rem;
  height: 10rem;
  border: 2px solid rgb(216, 216, 216);
  border-radius: 1rem;
  padding: 1rem;
  font-size: 11pt;
  margin-bottom: 1rem;
  font-family: Arial, Helvetica, sans-serif;
}

.modal .modal-content .form textarea:focus{
  outline: none;
  color: black;
}

.modal .modal-content .form textarea:empty{
  color: black;
}

.modal .modal-content .form .btn{
  padding-right: 1rem;
  padding-left: 1rem;
  width: 10rem;
  height: 3rem;
  border-radius: 1rem;
  border: 2px solid rgb(216, 216, 216);
  font-size: 12pt;
  font-weight: normal;
  margin: 1;
  background-color: rgb(216, 216, 216);
  cursor: pointer;
}

.modal .modal-content .form .btn:hover{
  background-color:aquamarine;
}

/* .modal .modal-content .img img{
  height: 80%;
} */

.modal .modal-content .close-btn {
  position: absolute;
  top: -1rem;
  right: -1rem;
  border-radius: 50%;
  height: 2rem;
  width: 2rem;
  margin: auto;
  background-color:rgb(216, 216, 216);
  color: white;
  cursor: pointer;
  border: 2px solid rgb(216, 216, 216);
}

.modal .modal-content .close-btn img {
  background-color: rgb(216, 216, 216);
  color: black;
  transform: scale(1.4);
  margin-top: 4px;
}

.fade-enter-active, .fade-leave-active {
  transition: opacity .5s;
}
.fade-enter, .fade-leave-to /* .fade-leave-active below version 2.1.8 */ {
  opacity: 0;
}

</style>
