<template>
<div>
  <div class="review-container">
        <div class="review-header">
            <h4 class="review-header__title">Özetle...</h4>
        </div>
        <div class="review-body-left">

            <img :src="images[selectedImage]" alt="" class="review-grade">
            
            
            <h5 id="review-summary" class="review-summary" 
            v-if="titleChangeActive == false"
            @dblclick="titleChangeActive = !titleChangeActive; summaryChangeActive = false"> {{reviewTitle}}</h5>
            
            <textarea id="review_summary_new_title"
            class="review_summary_new_title"
            autofocus
            v-else
            v-model="reviewTitle"
            v-on:blur="titleChangeActive = false"
            @keydown.enter.exact.prevent
            @keydown.enter.exact="titleChangeActive = false"
            type="text"></textarea>


            <p
            @dblclick="summaryChangeActive = !summaryChangeActive; titleChangeActive = false "   
            v-if="!summaryChangeActive" id="review-summary-text" class="review-summary-text">
               {{reviewSummaryText}}
            </p>

            <textarea id="review_summary_new_text"
            class="review_summary_new_text"
            autofocus
            v-else
            v-model="reviewSummaryText"
            @keydown.enter.exact.prevent
            @keydown.enter.exact="summaryChangeActive = false"

            ></textarea>


            <p>{{titleChangeActive}}</p>
            <p>{{reviewTitle}}</p>
            <p>{{reviewSummaryText}}</p>






        </div>
        <div class="review-body-right">
        <h5 class="review-title__positive">Sevebilirsiniz...</h5>
        <ul id="review-text" class="review-text">
            <li id="review-text__item" class="review-text__item">Bu çizgi romanı okumaktan kim keyif alabilir?</li>
            <li id="review-text__item" class="review-text__item">Bu çizgi romanı okumaktan başka kim keyif alabilir?</li>
            <li id="review-text__item" class="review-text__item">Kimlere tavsiye ediyoruz?</li>
        </ul>
        <hr class="review-body-right-hr">
        <h5 class="review-title__negative">Sevmeyebilirsiniz...</h5>
        <ul id="review-text" class="review-text">
             <li id="review-text__item" class="review-text__item">Watchmen'in öncesinin, sonrasının, devamının vs. yazılmasından bir rahatsızlık duymuyorsanız</li>
            <li id="review-text__item" class="review-text__item">Watchmen'in öncesinin, sonrasının, devamının vs. yazılmasından bir rahatsızlık duymuyorsanız</li>
            <li id="review-text__item" class="review-text__item">Watchmen'in öncesinin, sonrasının, devamının vs. yazılmasından bir rahatsızlık duymuyorsanız</li>
         </ul>
        </div>
        <div class="review-bottom-section">
            <div class="review-bottom">
                <a href="https://www.altevren.net/altevren-inceleme-sistemi" class="review-link">İnceleme sistemimiz hakkında daha fazlası için tıklayın!</a>
            </div>
        </div>
    </div>
</div></template>

<script>
export default {
  name: 'App',
  data() {
    return {
      titleChangeActive: false,
      summaryChangeActive: false,
      isChoosingImage: false,

      selectedImage: "harika",
      reviewTitle: "Başlık belirlemek için çift tıklayın!",
      reviewSummaryText: "Buraya başlığınızı da detaylandıran çok kısa, tercihen tek cümlelik bir yorum girin.",

      images: {
                muhtesemAmaHerkeseGoreDegil: "https://www.altevren.net/wp-content/uploads/2018/02/7_muhtamaherkesdegil.png",
                herkesOkumali: "https://www.altevren.net/wp-content/uploads/2018/02/earth.png",
                harika: "https://www.altevren.net/wp-content/uploads/2018/02/5_çogzel.png",
                turunEn: "https://www.altevren.net/wp-content/uploads/2018/02/4_sübj3.png",
                tureUygun: "https://www.altevren.net/wp-content/uploads/2018/02/3_sübj2.png",
                sadeceTurunHayranlari: "https://www.altevren.net/wp-content/uploads/2018/02/2_sübj1.png", 
                tuhaf: "https://www.altevren.net/wp-content/uploads/2019/05/8_tuhaf.png",
                vasat: "https://www.altevren.net/wp-content/uploads/2018/02/1_vasat.png",
                notYok: "https://www.altevren.net/wp-content/uploads/2018/02/0_notyok.png",

      },
    }
  },

  methods: {

  },



}
</script>

<style>

* {
  box-sizing: border-box;
} 

h1, h2, h3, h4, h5, h6 {
  margin: 0;
}


.review-container{

  font-family: -apple-system, BlinkMacSystemFont, 'Segoe UI', Roboto, Oxygen, Ubuntu, Cantarell, 'Open Sans', 'Helvetica Neue', sans-serif;

    margin: 0 auto;
    padding: 0;
    box-sizing: border-box;

    max-width: 730px;

    display: grid;
    grid-template-rows: 1fr auto 1fr;
    grid-template-columns: repeat(12, 1fr);

    border: 2px solid black;

    height: auto;
}

@media only screen and (max-width: 500px) {
	.review-container {
		display: flex;
		flex-direction: column;
		align-items: center;
	}
}

.review-header{
    grid-row: 1 / 1;
    background-color: black;
    grid-column: 1 / -1;

	  display: flex;
		align-items: center;
    max-height: 60px;
}


@media only screen and (max-width: 500px) {
	.review-header {
		width: 100%;
		height: 70px;
	}
}

.review-header__title {
    color: white !important;
    padding-left: 20px !important;
		margin-bottom: 0 !important;
    font-size: 20px !important;
}

.review-body-left {
    display: flex;
    flex-direction: column;
    justify-content: flex-start;
    padding-left: 20px;
    grid-column: 1 / span 5;
}

.review-grade {
    max-width: 128px;
    margin: 0 auto 20px auto;
}

@media only screen and (max-width: 500px) {
	.review-grade {
		margin: 1em auto 20px auto !important;
	}
}

.review-summary {
    font-size: 21px !important;
    font-weight: bold !important;
    text-align: center;
		margin-bottom: 20px;
    margin-top: 0;
    cursor: pointer;
}

.review-summary:hover {
  color: rgb(90, 90, 90);
}

.review-summary-text{
    font-size: 17px !important;
	  font-style: italic !important;
    text-align: center;
    cursor: pointer;
}

.review-summary-text:hover {
  color: gray
}

@media only screen and (max-width: 500px) {
	.review-summary-text {
		margin-right: 1em !important;
	}
}

.review-body-right {

    display: flex;
    flex-direction: column;
    grid-column: 6 / -1;
    padding-right: 20px;
    padding-left: 20px;
}

.review-body-right-hr {

    width: 90%;
    border: 0.3px solid #DEDEDE;
    padding: 0;
    margin-left: 0;
}

.review-title__positive {
    font-size: 18px !important;
    margin-bottom: 10px !important;
    color: #339966 !important;
}

.review-title__negative {
    font-size: 18px !important;
    margin-bottom: 10px !important;
    color: #FF0000 !important;
}


.review-text {
    padding: 0 !important;
    list-style: none !important;
}

.review-text > li {
	list-style: none !important;
}

.review-text__item {
		font-size: 18px !important;
	  line-height: 30px !important;
    margin-bottom: 28px !important;
		color: black !important;
}

.review-text__item:last-child {
	margin-bottom: 0 !important;
}
.review-bottom::before {
    content: "";
    border-top: .3rem solid black;
    width: 90%;
    margin: 0 1rem;
    transform: translateY(-1rem);
}

.review-bottom-section {
    display: flex;
    align-items: center;
    grid-row: 3 / 4;
    grid-column: 1 / -1;
    padding: 20px;
}

@media only screen and (max-width: 500px) {
	.review-bottom-section {
		display: block;
		text-wrap: wrap;
	}
}

.review-bottom::before {
    content: "";
    display: inline-block;
    border-top: .3px solid rgb(185, 185, 185);
    margin: 0 auto;
    width: 690px;
}

@media only screen and (max-width: 500px) {
	.review-bottom::before {
		display: none;
	}
}

.review_summary_new_title {

  border: 2px solid rgb(17, 17, 187);
  border-radius: 5px;;
  outline: none;
  resize: none;
  

  font-family: -apple-system, BlinkMacSystemFont, 'Segoe UI', Roboto, Oxygen, Ubuntu, Cantarell, 'Open Sans', 'Helvetica Neue', sans-serif;

  font-size: 21px !important;
  font-weight: bold;
  text-align: center;
	margin-bottom: 10px;
  cursor: text;
  height: 8 0px;
  line-break: auto;
  padding: 10px;

  
  color: rgb(39, 39, 39)
  }

  .review_summary_new_text {

    color: rgb(39, 39, 39);
    height: 100px;
    cursor: text;
    text-align: center;
    outline: none;
    border: .75px solid rgb(17, 17, 187);
    font-family: -apple-system, BlinkMacSystemFont, 'Segoe UI', Roboto, Oxygen, Ubuntu, Cantarell, 'Open Sans', 'Helvetica Neue', sans-serif;
    font-size: 16px;
    resize: none;
    padding: 3px;
    border-radius: 5px;
    width: auto;


  }

</style>
