<template>
  <div class="goodIdea">
    <div class="banner">
      <img class="basePicture" src="../assets/底圖.png" alt="goodidea">
      <div class="backgroundBlack"></div>
      <div class="backgroundBrown"></div>
      <img class="good shake-constant shake-slow" src="../assets/GOOD.png" alt="">
      <img class="ideas shake-constant shake-slow" src="../assets/IDEAS.png" alt="">
      <img class="doYouWantToKnow" src="../assets/想知道嗎.png" alt="">
      <img class="starLeft shake-constant shake-opacity" src="../assets/Starleft.png" height="56" width="58" alt="">
      <img class="starRight shake-constant shake-opacity" src="../assets/Starright.png" height="58" width="58" alt="">
    </div>
    <content class="row">
      <h2>每週分享會資訊</h2>
      <button type="button" class="navButton">
        <i class="fa fa-bars" aria-hidden="true"></i>
      </button>
      <ul class="navList">
        <li class="all" @click="showAll()" :class="{backgroundChange:allChange}">全部活動</li><!--
        --><li class="finished" @click="showFinished()" :class="{backgroundChange:finishedChange}">已結束活動</li><!--
        --><li class="unfinished" @click="showUnfinished()" :class="{backgroundChange:unfinishedChange}">未開始活動</li>
      </ul>
      <a href="https://devche.com/goodideabillboard/backstage">
        <button type="button" class="issue"  @mouseover="toggleShake()" @mouseleave="toggleShake()" :class="{shake:shaking}">發佈活動</button>
      </a>
      <div class="courseList">
        <div class="row">
          <div v-for="course in courses"  v-show="course.view"  class="course">
            <img :src="course.class_img" alt=""><!--
            --><div class="courseInformation">
              <h4>{{course.title}}</h4>
              <span class="courseStatus">{{course.course_status}}</span>
              <p class="speechDate">{{course.speech_date}}</p> 
              <div class="speaker">
                <img :src="course.speaker_img" alt="">
                <h5>{{course.speaker}}</h5>
              </div>
              <p>{{course.message}}</p>
              <a :href="course.link" v-if="course.showLink">
                <button type="button" class="handoutLink">內容連結</button>
              </a>
            </div>
          </div>
        </div>
      </div>
      <div class="moveToTop" @click="moveToTop()">
        <i class="fa fa-chevron-circle-up" aria-hidden="true"></i>
      </div>
    </content>
    <footer>
      <p>Copyright © 2017 <a href="http://goodideas-studio.com/">好想工作室</a><br>All Rights Reserved</p>
    </footer>
  </div>
</template>

<script>
export default {
  name: 'goodIdea',
  data () {
    return {
      courses: [],
      allChange: true,
      finishedChange: false,
      unfinishedChange: false,
      shaking: false
    }
  },
  created(){
    this.courseData()
  },
  methods:{
    courseData(){
      fetch('https://devche.com/api/speech/data')
        .then((res) => {res.json()
          .then((data) => {
          var courseList = data.result
          for (let i =0; i<courseList.length; i++){
            courseList[i].view = true

            var nowTime = new Date()
            var speechDate = courseList[i].speech_date
            var classDate = new Date(speechDate)
            if(classDate > nowTime){
              courseList[i].course_status = '即將開始'
            }else{
              courseList[i].course_status = '已經結束'
            }
            if(courseList[i].link === "null" || courseList[i].link === null){
              courseList[i].showLink = false
            }else{
              courseList[i].showLink = true
            }
          }

          courseList.sort(function (a, b) {
            if (a.speech_date > b.speech_date) {
              return -1
            }
            if (a.speech_date < b.speech_date) {
              return 1
            }
            return 0
          })
          this.courses = courseList
        })
      })
    },
    showFinished(){
      this.allChange = false
      this.finishedChange = true
      this.unfinishedChange = false

      var courseList = this.courses
      var nowTime = new Date()
      for (let i =0; i<courseList.length; i++) {
        var speechDate = courseList[i].speech_date
        var classDate = new Date(speechDate)
        if(classDate < nowTime){
          courseList[i].view=true;
        }else{
          courseList[i].view=false;
        }
      }

    },
    showUnfinished(){
      this.allChange = false
      this.finishedChange = false
      this.unfinishedChange = true

      var courseList = this.courses
      var nowTime = new Date()
      for (let i =0; i<courseList.length; i++) {
        var speechDate = courseList[i].speech_date
        var classDate = new Date(speechDate)
        if(classDate < nowTime){
          courseList[i].view=false;
        }else{
          courseList[i].view=true;
        }
      }
    },
    showAll(){
      this.allChange = true
      this.finishedChange = false
      this.unfinishedChange = false

      var courseList = this.courses
      for (let i =0; i<courseList.length; i++) {
            courseList[i].view=true;
      }
    },
    toggleShake(){
      if(this.shaking == false){
        this.shaking = true
      }else{
        this.shaking =false
      }
    },
    // moveToTop(){
    //   window.scrollTo(0, 0);
    // }
    moveToTop(){
      jQuery("html,body").animate({
          scrollTop:0
      },500)
    }
  }
}
$(document).ready(function(){
$(".navButton").click(function(){
    $(".navList").slideToggle("slow");
  });
});

$(window).scroll(function() {
    if ( $(this).scrollTop() > 300){
        $('.moveToTop').fadeIn("fast");
    } else {
        $('.moveToTop').stop().fadeOut("fast");
    }
});

</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped lang="sass">

*
  margin: 0
  padding: 0
.goodidea
  width: 100%
.banner
  position: relative
  height: 33.7vw
  margin-bottom: 5vw
  .basePicture
    width: 100%
    height: 100%
  .backgroundBlack
    position: absolute
    top: 0
    left: 27.5%
    background-color: rgba(74,74,74,0.5)
    width: 45%
    height: 33.7vw
    z-index: 0
  .backgroundBrown
    position: absolute
    top: 0
    left: 38.75%
    background-color: #8b572a
    width: 22.5%
    height: 33.7vw
    z-index: 1
  .good
    position: absolute
    top: 12%
    left: 38.75%
    width: 22.5%
    height: auto
    z-index: 2
  .ideas
    position: absolute
    top: 35%
    left: 37%
    width: 26%
    height: auto
    z-index: 3
  .doYouWantToKnow
    position: absolute
    top: 65%
    left: 35%
    width: 30%
    height: auto
    z-index: 4
  .starLeft
    position: absolute
    top: 25%
    left: 34%
    width: 3%
    height: auto
    z-index: 5
  .starRight
    position: absolute
    top: 5%
    left: 62%
    width: 3%
    height: auto
    z-index: 6

content
  width: 86%
  margin: auto
  position: relative
  h2
    margin: 0 1%
    color: #0f375b
  a
    @media(max-width: 768px)
      width: 100%
    .issue
      font-size: 24px
      width: 150px
      height: 40px
      background-color: #1e8ba6
      color: white
      position: absolute
      top: 0
      right: 0
      border-radius: 5px
      border: 0px
      cursor: pointer
      @media(max-width: 768px)
        width: 100%
        position: relative
        margin: 1vw 0
  .issue:active
    margin-top: 1px
  .navButton
    position: absolute
    right: 0
    width: 35px
    height: 35px
    border-radius: 20%
    background-color: white
    border: 0px
    padding-top: 3px
    @media(min-width: 769px)
      display: none
    i
      font-size: 25px
      color: #0f375b
  .navButton:active
    margin-top: 2px
  ul
    text-align: justify
    width: 100%
    margin: 15px 0
    background-color: white
    border: 1px #0f375b solid
    border-radius: 15px
    @media(max-width: 768px)
      display: none
    .backgroundChange
      background-color: #0f375b
      color: white
      font-weight: bold
    li
      width: 33.3333%
      padding: 5px
      display: inline-block
      text-align: center
      border: 1px #0f375b solid
      color: #0f375b
      font-size: 24px
      cursor: pointer
      @media(max-width: 768px)
        width: 100%
    li:hover
      font-weight: bold
    .all
      border-radius: 14px 0 0 14px
      @media(max-width: 768px)
        border-radius: 14px 14px 0 0
    .unfinished
      border-radius: 0 14px 14px 0
      @media(max-width: 768px)
        border-radius: 0 0 14px 14px
    li:hover
      cursor: point
  .courseList
    width: 100%
    .course
      width: 100%
      margin: 15px 0
      text-align: left
      min-height: 20vw
      img, .courseInformation
        display: inline-block
        vertical-align: top
      img
        width: 22%
        height: auto
        margin-right: 2%
      .courseInformation
        position: relative
        margin-left: auto
        padding: 5px 5px 40px 5px
        width: 76%
        text-align: left
        background-color: rgba(30,139,166,0.1)
        min-height: 19vw
        @media(max-width: 768px)
          padding-bottom: 40px
        .courseStatus
          font-size: 20px
          color: #1e8ba6
          font-weight: bold
          position: absolute
          right: 1vw
          top: 0.5vw
          @media(max-width: 768px)
            top: 80%
            left: 1vw
        h4
          margin: 1vw 0 0 1vw
          color: #7a062e
        .speaker
          img,h5
            display: inline-block
          img
            width: 50px
            height: auto
            border-radius: 50%
            margin: 0.5vw 1vw 0 1vw
          h5
            color: #0f375b
            line-height: 5vw
        p
          text-align: justify
          margin: 0.5vw 1vw
          font-size: 18px
          height: 60px
          overflow: auto
          white-space: pre-wrap
        .speechDate
          display: inline
          color: #1e8ba6
          font-size: 18px
        .handoutLink
          background-color: #0f375b
          color: white
          width: 120px
          height: 35px
          font-size: 18px
          border: 0px
          border-radius: 5px
          position: absolute
          right: 1vw
          bottom: 1vw
          cursor: pointer
  .moveToTop
    position: fixed
    right: 2%
    bottom: 10%
    font-size: 50px
    color: rgba(30,139,166,0.56)
    cursor: pointer
    animation-name: moveToTop
    animation-duration: 2000ms
    animation-timing-function: ease-in-out
    animation-iteration-count: infinite
    @keyframes moveToTop
      20%
        transform: translateY(-5px)
      40%
        transform: translateY(5px)
      60%
        transform: translateY(0)
      80%
        transform: translateY(0)
      0%, 100%
        transform: translateY(0)
    @media(max-width: 768px)
      right: 1%

footer
  background-color: rgba(30,139,166,0.56)
  margin-top: 2vw
  color: white
  height: 80px
  padding: 20px
  p
    font-size: 14px
    a
      color: white
      font-weight: bold
</style>