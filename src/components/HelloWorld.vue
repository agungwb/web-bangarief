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
          <div v-if="modal.form.review" class="review">
            <h1>{{modal.title_review}}</h1>
            <div class="story">
              <h3>{{modal.form.title}}</h3>
              <div class="author">
                {{modal.form.author}}
              </div>
              <p v-html="modal.form.story">
              </p>
            </div>
            <button class="btn" @click="submitStory">{{modal.form.submit}}</button>
          </div>
          <div v-else class="form">
            <div>
              <h1>{{modal.title}}</h1>
            </div>
            <div>
              <div class="label">{{modal.label.author}}</div><br />
              <input v-model="modal.form.author" v-bind:placeholder="modal.placeholder.author"> <br />
              <div class="label">{{modal.label.title}}</div><br />
              <input v-model="modal.form.title" v-bind:placeholder="modal.placeholder.title"> <br />
              <div class="label">{{modal.label.story}}</div><br />
              <textarea v-model="modal.form.story" v-bind:placeholder="modal.placeholder.story"></textarea> <br />
              <button class="btn" @click="reviewStory">{{modal.form.submit}}</button>
            </div>
          </div>
        </div>
        <!-- <div class="modal-confirm">

        </div> -->
        <!-- <div class="modal-failed">
          This is just test
        </div> -->
      </div>
    </transition>
    <h3 class="opening">{{ msg1 }}</h3>
    <h1 class="title">{{ msg2 }}</h1>
    <img v-bind:src="img1" class="main"/>
    <div class="preface">
      <div class="innalillahi">{{ msg3 }}</div>
      <p>{{ msg4 }}</p>
      <p>{{ msg5 }}</p>
      <p>{{ msg6 }}</p>
      <p>{{ msg7 }}</p>
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

    <div class="stories">
      <h2>{{story.total}} Stories</h2>
      <button class="btn" @click="showFormModal">{{ btn1 }}</button>
      <div v-for="story in story.stories" :key="story.idx" class="story">
        <h3>{{story.title}}</h3>
        <div class="date">{{story.created_on}}</div>
        <div class="author">
          {{story.author}}
        </div>
        <p v-html="story.story">
        </p>
      </div>
      <button v-if="story.total > story.stories.length" class="btn-more" @click="getMoreStories"><img v-bind:src="require('@/assets/icon/ic-gray-plus.svg')" /></button>
      <button class="btn" @click="showFormModal">{{ btn1 }}</button>
    </div>
  </div>
</template>

<script>
import { Swiper, SwiperSlide, directive } from 'vue-awesome-swiper'

// import style (<= Swiper 5.x)
import 'swiper/css/swiper.css'
import axios from 'axios'

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
          require('@/assets/slideshow/bangarief3.png'),
          require('@/assets/slideshow/bangarief4.png'),
          require('@/assets/slideshow/bangarief5.jpeg')
        ],
        position: 0,
        length: 5,
        direction: 'left',
        timer: '',
        timeout: ''
      },
      msg1: 'In Loving Memory of',
      msg2: 'Bang Arief Munandar',
      msg3: 'إِنَّا لِلَّٰهِ وَإِنَّا إِلَيْهِ رَاجِعُونَ',
      msg4: 'Wafatnya Bang Arief meninggalkan duka bagi kita semua.',
      msg5: 'Sosok Bang Arief sebagai suami, ayah, kakek, teman, dan guru akan kita rindukan.',
      msg6: 'Selama hidupnya, Bang Arief (Alm) pasti memberikan kesan yang mendalam ataupun pesan-pesan pelajaran kepada kita semua. Pesan kebaikan sebagai seorang suami, ayah, guru, teman, dan berbagai peran yang diemban.',
      msg7: 'Untuk mengenang pesan kebaikan beliau, kami mengundang bapak, ibu, murid, dan siapapun yang mengenal beliau untuk berbagi testimoni, cerita, dan pesan tentang Bang Arief (Alm).',
      btn1: 'Add Your Story',
      story: {
        stories: [],
        stories_short: [],
        total: 0,
        form: {
          flag: false
        },
        loading: false
      },
      modal: {
        title: 'Tell Your Story with Bang Arief',
        title_review: 'Review It First',
        label: {
          author: 'Name',
          title: 'Title',
          story: 'Story'
        },
        form: {
          author: '',
          title: '',
          story: '',
          submit: 'Submit',
          review: false
        },
        placeholder: {
          author: 'Put Your Name',
          title: 'Give It a Title',
          story: 'Tell Your Story ...'
        },
        loading: false
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
      this.modal.form.author = ''
      this.modal.form.title = ''
      this.modal.form.story = ''
      this.modal.form.review = false
    },
    preprocessStory () {
      let temp = this.modal.form.story.split('\n')
      let processedStory = ''
      for (let i = 0; i < temp.length; i++) {
        if (temp[i] !== '') {
          processedStory = processedStory + temp[i] + '<br /><br />'
        }
      }
      this.modal.form.story = processedStory
    },
    reviewStory () {
      this.preprocessStory()
      this.modal.form.review = true
    },
    submitStory () {
      this.modal.loading = true
      axios.post('http://localhost:8081/v1/stories', {
        author: this.modal.form.author,
        title: this.modal.form.title,
        story: this.modal.form.story,
        email: 'test@test.com'
      })
        .then(response => {
          alert('Submission berhasil.\n\nStory Anda akan dimoderasi terlebih dahulu. Mohon menunggu.')
          this.modal.loading = false
          this.hideFormModal()
        }).catch(error => {
          alert('Submission gagal.\n\nHarap periksa kembali semua isian')
          console.log(error)
          this.modal.form.review = false
        })
    },
    getMoreStories () {
      this.story.loading = true
      axios.get('http://localhost:8081/v1/stories', {
        params: {
          id: (this.story.stories[this.story.stories.length - 1].id)
        }
      })
        .then(response => {
          this.story.stories.push(...response.data.data.stories)
          this.story.total = response.data.data.total
        }).catch(error => {
          console.log(error)
        })
        .finally(() => {
          this.story.loading = false
        })
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
  },
  mounted () {
    this.story.loading = true
    axios.get('http://localhost:8081/v1/stories')
      .then(response => {
        this.story.stories.push(...response.data.data.stories)
        this.story.total = response.data.data.total
      }).catch(error => {
        console.log(error)
      })
      .finally(() => {
        this.story.loading = false
      })
  }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>

* {
  background-color:rgba(255, 255, 255, 0.5);
  font-family: "Lora-Regular";
  color: rgb(101, 101, 101);
}

p {
  font-size: 22px;
}

h1, h2, h3, h4, h5, h6{
  font-weight: normal;
}

h3.opening {
  font-family: "Roboto-Light";
  border-radius: 1rem;
  margin:auto;
  width: 14rem;
  padding: 0.5rem;
}

h1.title {
  font-family: "DancingScript-SemiBold";
  font-size: 52px;
  color: #d4af37;
  background: radial-gradient(ellipse farthest-corner at right bottom, #FEDB37 0%, #FDB931 8%, #9f7928 30%, #8A6E2F 40%, transparent 80%),
                radial-gradient(ellipse farthest-corner at left top, #5d4a1f 0%, #5d4a1f 8%, #D1B464 25%, #FFFFAC 62.5%, #FFFFFF 100%);
  -webkit-background-clip: text;
  -webkit-text-fill-color: transparent;
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

.preface {
  margin-left: 25%;
  margin-right: 25%;
}

.preface .innalillahi {
  margin-top: 3rem;
  margin-bottom: 2rem;
  font-family: "Lora-Regular";
  font-size: 21pt;
}

.preface p {
  font-family: "Lora-Regular";
  font-size: 15pt;
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
  transition: .5s;
}

.slideshow .slide-left-enter {
  transform: translate(-100%, 0);
}

.slideshow .slide-left-leave-to /* .fade-leave-active below version 2.1.8 */ {
  transform: translate(100%, 0);
}

.slideshow .slide-right-enter-active, .slideshow .slide-right-leave-active  {
  transition: .5s;
}

.slideshow .slide-right-enter {
  transform: translate(100%, 0);
}

.slideshow .slide-right-leave-to /* .fade-leave-active below version 2.1.8 */ {
  transform: translate(-100%, 0);
}

.stories {
  margin-top: 3rem;
  margin-left: 20%;
  margin-right: 20%;
  border-top: 1px solid rgb(216, 216, 216);
}

.stories h2 {
  margin-top: 3rem;
  margin-bottom: 2rem;
  font-size: 24pt;
  color: rgb(97, 97, 97);
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
  font-family: "Roboto-Light";
}

.stories .btn:hover{
  background-color: rgba(216, 216, 216, 0.5);
  cursor: pointer;
}

.stories .btn-more{
  display: block;
  margin: auto;
  margin-bottom: 2rem;
  padding-right: 1rem;
  padding-left: 1rem;
  width: 3rem;
  height: 3rem;
  border-radius: 50%;
  border: 2px solid rgb(216, 216, 216);
  color: red;
}

.stories .btn-more img {
  transform: scale(1.3);
  margin-top: 5px;
}

.stories .btn-more:hover{
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
  margin-top: .5rem;
  font-size: 16pt;
  font-weight: bold;
}

.stories .story .date{
  margin-top: -1rem;
  font-size: 10pt;
  font-family: "Roboto-light";

}

.stories .story p{
  font-size: 13pt;
}

.stories .story .author{
  margin-top: 1rem;
  font-size: 11pt;
  font-family: "Roboto-Bold";
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

.modal .modal-failed {
  z-index: 2;
  background-color: #ffffff;
  margin: 5% auto; /* 15% from the top and centered */
  padding: 3rem;
  width: 30rem; /* Could be more or less, depending on screen size */
  height: 20rem;
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
  width: 50%;
  text-align: left;
}

.modal .modal-content h1{
  font-family: "Roboto-Medium";
}

.modal .modal-content .form .label{
  margin-left: 1rem;
  margin-bottom: -1rem;
  background-color: transparent;
  font-family: "Roboto-Light";
  font-size: 11pt;
}

.modal .modal-content .form input{
  width: 20rem;
  height: 1rem;
  margin-bottom: 1rem;
  border: 2px solid rgb(216, 216, 216);
  border-radius: 1rem;
  padding: 1rem;
  font-size: 11pt;
}

.modal .modal-content .form input:focus{
  outline: none;
}

.modal .modal-content .form input:empty{
  color: black;
  /* font-size: 12pt; */
}

.modal .modal-content .form textarea{
  width: 33rem;
  height: 10rem;
  border: 2px solid rgb(216, 216, 216);
  border-radius: 1rem;
  padding: 1rem;
  font-size: 11pt;
  margin-bottom: 1rem;
}

.modal .modal-content .form textarea:focus{
  outline: none;
}

.modal .modal-content .form textarea:empty{
  color: black;
  /* font-size: 11pt; */
}

.modal .modal-content .btn{
  padding-right: 1rem;
  padding-left: 1rem;
  width: 10rem;
  height: 3rem;
  border-radius: 1rem;
  border: 2px solid rgb(216, 216, 216);
  font-size: 12pt;
  font-weight: normal;
  margin: 1;
  cursor: pointer;
  font-family: "Roboto-Regular"
}

.modal .modal-content .btn:hover{
  background-color: rgba(216, 216, 216, 0.5);
}

.modal .modal-content .review {
  height: 100%;
  width: 50%;
  text-align: left;
}

.modal .modal-content .review .story {
  height: 65%;
  width: 100%;
  overflow: scroll;
  padding: 1rem;
  margin-bottom: 1rem;
  border: 1px solid rgb(216, 216, 216);
  border-radius: 1rem;
}

.modal .modal-content .review .story h3{
  margin-top: .5rem;
  font-size: 16pt;
  font-weight: bold;
}

.modal .modal-content .review .story .date{
  margin-top: -1rem;
  font-size: 10pt;
  font-family: "Roboto-light";

}

.modal .modal-content .review .story p{
  font-size: 13pt;
}

.modal .modal-content .review .story .author{
  margin-top: 1rem;
  font-size: 11pt;
  font-family: "Roboto-Bold";
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

.fade-enter-active, .fade-leave-active{
  transition: opacity .5s;
}

.fade-leave-active {
  /* display: none; */
}

.fade-enter, .fade-leave-to /* .fade-leave-active below version 2.1.8 */ {
  opacity: 0;
}

</style>
