<template>
<div class="full-container">

<transition 
            appear
            type="animation"
            enter-active-class="animate__animated animate__fadeInDown animate__fast"
            leave-active-class="animate__animated animate__fadeOutUp animate__fast"
>
  <div v-if="titleChangeActive || summaryChangeActive" class="editing-notice">
    Düzenlemeniz bittiğinde enter'a basın veya herhangi bir yere tıklayın!
  </div>
</transition>

<transition 
            appear
            type="animation"
            enter-active-class="animate__animated animate__fadeInDown animate__fast"
            leave-active-class="animate__animated animate__fadeOutUp animate__fast"
>
  <div v-if="isCodeCopied" class="copied-notice">
    Kod kopyalandı, CTRL / CMD + V ile yapıştırın.
  </div>
</transition>


<div :class="{blur: codeGenerated}" class="page-container">



  <div class="review-container">
        <div class="review-header">
            <h4 class="review-header__title">Özetle...</h4>
            <button @click="codeGenerated = true" class="code-generator-button">Kodu Alayım</button>
        </div>
        <div class="review-body-left">

            <img :src="images[selectedImage].url" alt="" class="review-grade" @click="displayedRightComponent = 'grade-select'">
            
            
            <h5 id="review-summary" class="review-summary" 
            v-if="titleChangeActive == false"
            @dblclick="titleChangeActive = !titleChangeActive; summaryChangeActive = false"> {{reviewTitle}}</h5>
            
            <textarea id="review_summary_new_title"
            class="review_summary_new_title"
            v-focusSelect
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
            v-else
            v-focusSelect
            v-model="reviewSummaryText"
            v-on:blur="summaryChangeActive = false"
            @keydown.enter.exact.prevent
            @keydown.enter.exact="summaryChangeActive = false"

            ></textarea>
        </div>
        <div class="review-body-right">
        <h5 class="review-title__positive">Sevebilirsiniz...</h5>
        <ul
        id="review-text" class="review-text">
            <li v-for="(positive, index) in reviewPositives"
            @contextmenu.prevent="deleteListItem(reviewPositives, index)"
            id="review-text__item" class="review-text__item"> 
              {{ positive }} 
            </li>
        </ul>
        <hr class="review-body-right-hr">
        <h5 class="review-title__negative">Sevmeyebilirsiniz...</h5>
        <ul id="review-text" class="review-text">
             <li v-for="(negative, index) in reviewNegatives" 
             @contextmenu.prevent="deleteListItem(reviewNegatives, index)"
             id="review-text__item" class="review-text__item"> 
             {{ negative }}</li>
         </ul>
        </div>
        <div class="review-bottom-section">
            <div class="review-bottom">
                <a href="https://www.altevren.net/altevren-inceleme-sistemi" class="review-link">İnceleme sistemimiz hakkında daha fazlası için tıklayın!</a>
            </div>
        </div>
    </div>

    <component :is="displayedRightComponent"
               :images="images"
               @listsCleared="clearLists" 
               @gradeSelected="changeGrade"
               @positivesUpdated="updatePositives"
               @negativesUpdated="updateNegatives"
    > </component>


</div>
    <div class="code-container" v-if="codeGenerated">
      <div class="code-container__title"> Kodunuz Hazır! </div>
      <textarea id="finalCode" class=code-container__code readonly> {{unchangedCodePart1
        + images[selectedImage].url
        + unchangedCodePart2
        + reviewTitle
        + unchangedCodePart3
        + reviewSummaryText
        + unchangedCodePart4
        + positivesListHTML
        + unchangedCodePart5
        + negativesListHTML
        + unchangedCodeFinal}}
      </textarea>
      <div class="code-container__info">Oluşturduğunuz kodu buradan kopyalayıp, AltEvren admin panelinde "Özel HTML" olarak yapıştırabilirsiniz.</div>
      <div class="code-container__button_row">
        <button @click="copyFinalCode" class="code-container__button code-container__copy"> 📃 Kopyala</button>
        <button @click="codeGenerated=false" class="code-container__button code-container__close">Kapat</button>
      </div>
      
    </div>
</div>

</template>

<script>

import GradeSelect from "./components/GradeSelect.vue"
import QuickAddListElements from './components/QuickAddListElements.vue'

export default {
  name: 'App',
  data() {
    return {
      titleChangeActive: false,
      summaryChangeActive: false,
      isChoosingImage: false,

      selectedImage: 2,
      reviewTitle: "Başlık belirlemek için çift tıklayın!",
      reviewSummaryText: "Buraya başlığınızı da detaylandıran çok kısa, tercihen tek cümlelik bir yorum girin.",

      reviewPositives: ["Bu çizgi romanın nasıl güçlü yanları var?", "Eser kimlerin hoşuna gidebilir?", "Aklınıza gelen başka olumlu noktalar var mı?"],
      reviewNegatives: ["Bu çizgi roman kimlerin hoşuna gitmeyebilir?", "Bu çizgi romanın hoşuna gitmeyebileceği başka kişiler var mı?", "Aklınıza gelen başka bir olumsuz durum varsa buraya ekleyebilirsiniz."],


      images: [
                { alias: 'muhtesemAmaHerkeseGoreDegil',
                  title: "Muhteşem, ama herkese göre değil",
                  url: "https://www.altevren.net/wp-content/uploads/2018/02/7_muhtamaherkesdegil.png",
                  explanation: "Bu notu, size göre çok başarılı olan, ancak herkesin keyif almayacağı bir çizgi romanı tanıtmak için kullanabilirsiniz. Örneğin, çok derin, çok kompleks, ama okunması zor bir çizgi roman için bu not ideal seçim olabilir."
                  },

                { alias: 'herkesOkumali',
                  title: "Herkesin okuması gereken bir çizgi roman.",
                  url: "https://www.altevren.net/wp-content/uploads/2018/02/earth.png",
                  explanation: "Bu notu, gerçekten çizgi romanla ilgilenen (hatta ilgilenmeyen) herkesin okuması gereken, okuyanların %95'inin beğeneceği bir çizgi roman için kullanın. Daytripper gibi bir çizgi roman bunun iyi bir örneği olabilir. Unutmayın, bu notu seçtiğinizde oldukça güçlü bir iddiada bulunuyorsunuz, o yüzden bu notu dikkatli ve nadiren kullanın."
                  },

                { alias: 'harika',
                  title: "Harika",
                  url: "https://www.altevren.net/wp-content/uploads/2018/02/5_çogzel.png",
                  explanation: "Bu notu, gerçekten pek çok açıdan harika, olağanüstü olan, sizi çok etkileyen bir çizgi roman için kullanın. Bu notu özellikle tür sınırlamalarını aşan çizgi romanlar için kullanın, yani o türü sevmeyen kişilerin bile muhtemelen beğeneceği bir çizgi romanı bu not ile ifade edin. Watchmen, Walking Dead'in ilk cildi gibi çizgi romanlar bu nota ideal örnekler olabilir."
                },

                { alias: 'turunEn', 
                  title: "Türün En İyi Örneklerinden Biri",
                  url: "https://www.altevren.net/wp-content/uploads/2018/02/4_sübj3.png",
                  explanation: "Bu notu, konu aldığınız türün en iyi çizgi romanlarını tanıtmak için kullanın. Süper kahraman çizgi romanlarını seven insanların mutlaka seveceği bir süper kahraman çizgi romanı, bunun için ideal bir örnek olabilir. Eğer okuduğunuz çizgi romanı bu türü sevmeyenlerin bile beğeneceğini düşünüyorsanız, 'Harika' notunu da tercih edebilirsiniz."},

                { alias: 'tureUygun', 
                  title: "Türü Sevenler İçin",
                  url: "https://www.altevren.net/wp-content/uploads/2018/02/3_sübj2.png",
                  explanation: "Çok olağanüstü olmayan, 'harika, muhteşem' denemeyecek, ama bu türü ve bu tarzı sevenlerin keyif alacağı çizgi romanlar için bu ifadeyi kullanın."},
            
                { alias: 'sadeceTurunHayranlari', 
                  title: "Sadece Türün Hayranları İçin",
                  url: "https://www.altevren.net/wp-content/uploads/2018/02/2_sübj1.png", 
                  explanation: "Bunu pek çok açıdan sıradan olan, pek çok açıdan beğenilmeyecek, ancak o türün hayranlarına hitap edebilecek çizgi romanlar için kullanın. Marvel ve DC'nin sık sık yayımladığı event hikayeleri bunun için ideal bir örnek olabilir: Pek çok okur, hikaye zincirini ve eser içindeki onlarca karakteri tanımayacağı için bu eserden zevk almaz, ama yine de serileri düzenli takip eden uzun süreli okurlar bu eserleri keyifle okuyabilir.", },

                { alias: 'tuhaf', 
                  title: "Tuhaf",
                  url: "https://www.altevren.net/wp-content/uploads/2019/05/8_tuhaf.png", 
                  explanation: "Eğer bir çizgi roman okuduğunuzda, ilk verdiğiniz tepki 'Yahu ben ne okudum şimdi...' oluyorsa, diğer notlardan çok bu notu ön plana çıkartmayı düşünebilirsiniz. Kendi içinde çok mantıklı bulmadığınız deneysel, farklı, egzantirik çizgi romanlar, bu not için ideal olabilir."},
                
                { alias: 'vasat', 
                  title: "Vasat",
                  url: "https://www.altevren.net/wp-content/uploads/2018/02/1_vasat.png",
                  explanation: "Eğer bir çizgi romanın, teknik ve anlatı açısından gerçekten başarısız olduğunu düşünüyorsanız, bu notu kullanın. Ama bu notu bir 'son tercih' olarak görmeye çalışın; eğer çizgi roman gerçekten, objektif olarak kötü değilse mutlaka keyif alacak birileri çıkacaktır."},
                
                { alias: 'notYok', 
                  title: "Not Yok",
                  url: "https://www.altevren.net/wp-content/uploads/2018/02/0_notyok.png",
                  explanation: "Bu sistem üzerinde uğraşıyoruz, ama sonuçta her çizgi romanı illa buradaki sekiz maddeden biriyle tanımlayabileceğiz diye bir iddiamız yok. Eğer bir çizgi roman, buradaki not skalasına göre değerlendirilemiyorsa, veya hiçbirisi tam olarak içinize sinmiyorsa, bu notu kullanmaktan çekinmeyin."
                },

      ],


      displayedRightComponent: QuickAddListElements,

      codeGenerated: false,
      code: "This will be the code.",
      isCodeCopied: false,

      unchangedCodePart1: '<div class="review-container"><div class="review-header"><h4 class="review-header__title">Özetle...</h4></div><div class="review-body-left"><img src="', 
      unchangedCodePart2: '" class="review-grade"><h5 id="review-summary" class="review-summary">',
      unchangedCodePart3: '</h5><p id="review-summary-text" class="review-summary-text">',
      unchangedCodePart4: '</p></div><div class="review-body-right"><h5 class="review-title__positive">Sevebilirsiniz...</h5><ul id="review-text" class="review-text">',
      unchangedCodePart5: '</ul><hr class="review-body-right-hr"><h5 class="review-title__negative">Sevmeyebilirsiniz...</h5><ul id="review-text" class="review-text">',
      unchangedCodeFinal: '</ul></div><div class="review-bottom-section"><div class="review-bottom"><a href="https://www.altevren.net/altevren-inceleme-sistemi" class="review-link">İnceleme sistemimiz hakkında daha fazlası için tıklayın!</a></div></div></div>',
    
    }
  },

  methods: {
    changePositive() {
      console.log("ChangePositive ran.")
    },
    changeNegative() {
      console.log("ChangeNegative ran.")
    },

    updatePositives(positives) {
      this.reviewPositives = positives
    },

    changeGrade(selectedGrade) {
      this.selectedImage = selectedGrade
      this.displayedRightComponent = QuickAddListElements
    },

    updateNegatives(negatives) {
      this.reviewNegatives = negatives
    },

    changeListItem(list, index) {
      list.splice(index, 1, this.onPageEditedListItem)
    },

    deleteListItem(list, index) {
      list.splice(index, 1)
    },

    clearLists() {
      console.log("Clear order recieved from child.")
      this.reviewPositives = []
      this.reviewNegatives = []
    },

    copyFinalCode() {
      console.log("copyFinalCode ran.")
      this.isCodeCopied = true
      finalCode = document.getElementById("finalCode")
      finalCode.select()
      document.execCommand("copy");
      setTimeout(() => { 
        this.isCodeCopied = false
       }, 2000);
    }


  },

  computed: {
    positivesListHTML() {
      let completeString = ""
      let beginningTag = '<li id="review-text__item" class="review-text__item">'
      let endingTag = "</li>"

      this.reviewPositives.forEach(positive  => {
        let positiveHTML = beginningTag + positive + endingTag
        completeString = completeString + positiveHTML
      })

      return completeString
    },

    negativesListHTML() {
      let completeString = ""
      let beginningTag = '<li id="review-text__item" class="review-text__item">'
      let endingTag = "</li>"

      this.reviewNegatives.forEach(negative  => {
        let positiveHTML = beginningTag + negative + endingTag
        completeString = completeString + positiveHTML
      })

      return completeString
    },

    },


  components: {
    "grade-select": GradeSelect,
    "quick-add-list-elements": QuickAddListElements,
  },

  directives: {
    focusSelect: {
      inserted (el) {
        el.focus()
        el.select()
      }
    }
  }


}
</script>

<style>

* {
  box-sizing: border-box;
} 

h1, h2, h3, h4, h5, h6 {
  margin: 0;
}

body {
  overflow: hidden;
}



.page-container {
  display: grid;
  grid-template-columns: 60% 40%;
  position: absolute;
  }

.blur {
  filter: blur(.2rem); 
}

@media only screen and (max-width: 600px) {
  .page-container {
    display: flex;
    flex-direction: column;
    align-items: center;
  }
}

.editing-notice {
  position: absolute;
  z-index: 300;
  width: 100vw;
  height: 60px;
  display: flex;
  align-items: center;

  font-size: 1.2rem;
  font-family: -apple-system, BlinkMacSystemFont, 'Segoe UI', Roboto, Oxygen, Ubuntu, Cantarell, 'Open Sans', 'Helvetica Neue', sans-serif;
  padding: 20px;
  color: white;
  
  top: 0px;
  left: 0px;
  bottom: 20px;
  margin-bottom: 40px;
  margin-left: 0px;

  background-color: rgb(139, 165, 214);
  border-bottom: 1px solid rgb(112, 152, 226);
}

.copied-notice{

  position: absolute;
  z-index: 300;
  width: 100vw;
  height: 60px;
  display: flex;
  align-items: center;

  font-size: 1.2rem;
  font-family: "Open Sans", Arial, Helvetica, sans-serif;
  padding: 20px;
  color: white;
  
  top: 0px;
  left: 0px;
  bottom: 20px;
  margin-bottom: 40px;
  margin-left: 0px;

  background-color: rgb(87, 221, 132);
  border-bottom: 1px solid rgb(87, 221, 132);


}


.review-container{

  font-family: -apple-system, BlinkMacSystemFont, 'Segoe UI', Roboto, Oxygen, Ubuntu, Cantarell, 'Open Sans', 'Helvetica Neue', sans-serif;

    margin: 10px 20px;
    padding: 0;
    box-sizing: border-box;

    max-width: 730px;

    grid-column: 1 / 2;

    display: grid;
    grid-template-rows: 1fr auto 1fr;
    grid-template-columns: repeat(12, 1fr);

    border: 2px solid black;

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
    justify-content: space-between;
    max-height: 60px;
}

.code-generator-button {
  font-family: "Open Sans";
  color: white;
  margin-right: 10px;
  height: 60%;
  width: 10rem;
  background-color: white;
  color: black;
  font-size: 1rem; 
  letter-spacing: 0.1em;
  cursor: pointer;
  border: 1px solid white;
  border-radius: 3px;
  box-shadow: 0 1px 4px rgba(0, 0, 0, 0.3), 0 0 40px rgba(0, 0, 0, 0.1) inset;
}

.code-generator-button:active{
  transform: scale(.8);
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

.review-grade:hover {
  cursor: pointer;
  transform: scale(1.050);
  transition: scale .2s;
  filter: contrast(60%);
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
  cursor: pointer;
  user-select: none;
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

#grade-select{
  grid-column: 2/-1;
}

#quick-add-list-elements {
  grid-column: 2/-1;
}

.code-container{
  position: absolute;
  left: 30%;
  top: 5%;
  width: 600px;
  height: 600px;
  background-color: white;;
  z-index: 20;
  border: 1px solid rgb(199, 199, 199);
  border-radius: 10px;
  padding: 10px;

  display: flex;
  flex-direction: column;

}

.code-container__title {
  font-size: 1.4rem;
  font-family: "Open Sans", Arial, Helvetica, sans-serif;
  margin-bottom: 10px;

}

.code-container__code {
  width: 90%;
  height: 40%;
  border:1px solid rgb(216, 216, 216);
  border-radius: 5px;
  margin: 0 auto;
  resize: none;;
  overflow: scroll;
}

.code-container__info {

  width: 90%;
  margin-right: auto;
  margin-left: auto;
  margin-top: 20px;
  font-family: "Open Sans", Arial, Helvetica, sans-serif;
  font-size: .9rem;
  color: grey;
  margin-bottom: 20px;

}

.code-container__button_row {
  width: 90%;
  margin: 0px auto;
  display: flex;
  justify-content: space-between;
}

.code-container__button{
  color: white;
  font-size: 1rem;
  padding: 10px;
  letter-spacing: 0.1rem;
  border-radius: 2px;
  cursor: pointer;
}

.code-container__button:hover{
  box-shadow: 2px 2px 3px rgb(173, 173, 173);
}

.code-container__button:active {
  transform: scale(.9);
}

.code-container__close{
  background-color: rgb(85, 8, 8);
  color: white;
  width: 32%;
  border: 1px solid rgb(85, 8, 8);
  border-radius: 2px;
  align-self: flex-end;
}

.code-container__copy{
  background-color: rgb(26, 83, 8);
  color: white;
  border: 1px solid rgb(26, 83, 8);
  width: 65%;
  border-radius: 2px;
  align-self: flex-end;
}

</style>
