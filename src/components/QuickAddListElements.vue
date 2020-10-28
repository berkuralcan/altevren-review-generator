<template>
    <div class="list-elements-container">
        <div class="list-elements-container__header">
            <h3 class="list-elements-container__title"> Hızlı Ekleme</h3>
            <button class="add-button" @click="clearLists">Listeleri Temizle</button>
        </div>
        
        <div class="list-elements-container__info">
            Bu inceleme için pozitif ve negatif noktaları aşağıdaki bölümden hızlı bir şekilde ekleyebilirsiniz.
            <br><br>
            Maddeleri noktalı virgülle ayırarak, her bir alana en fazla beş madde yazacak şekilde ekleyin! Beşten fazla madde yazarsanız, ilk maddeden sonrası otomatik olarak silinir. 
            <br><br>
            Eklemek için enter'a basabilirsiniz. Listeyi sıfırlamadan eklediğiniz bir spesifik bir maddeyi çıkartmak için kutu üzerinde listenin üzerine gelip sağ tıklayabilirsiniz.
        </div>
        <div class="list-elements-container__heading">Sevebilirsiniz</div>
        <textarea 
        @keydown.enter.exact.prevent
        @keydown.enter.exact="addPositives"
        rows="5" class="list-elements-container__input_field input-field-positive" v-model="positivesRawText" name="positivesRawText"></textarea>
        <button class="add-button" @click="addPositives">İncelemeye Ekle</button>
        <br><br>
        <div class="list-elements-container__heading">Sevmeyebilirsiniz</div>
        <textarea 
        @keydown.enter.exact.prevent
        @keydown.enter.exact="addNegatives"
        rows="5" class="list-elements-container__input_field input-field-negative" v-model="negativesRawText" name="negativesRawText"></textarea>
        <button class="add-button" @click="addNegatives">İncelemeye Ekle</button>

    </div>
</template>

<script>
export default {
    name: "QuickAddListElements",
    data() {
        return {
            positives: [],
            negatives: [],

            positivesRawText: "",
            negativesRawText: "",
        }
    },

    props: ["reviewPositives", "reviewNegatives"],

    methods: {
        addPositives() {
            let positivesRawList = this.positivesRawText.split(";")

            // Using an arrow function to access "this.positives"

            if(this.positives.length < 5) {
                positivesRawList.forEach( positive => {
                    positive = positive.trim()
                    if(positive != "" && this.positives.length < 5) {
                        this.positives.push(positive.charAt(0).toUpperCase() + positive.slice(1))
                    }
                })

            this.$emit("positivesUpdated", this.positives)
            this.positivesRawText = ""
            } else {
                alert("Alt kısmı çok uzun tutmamak adına, listelere beşden fazla madde eklemeyelim diyorum.")
            }


        },

        addNegatives(){
            let negativesRawList = this.negativesRawText.split(";")

            if(this.negatives.length < 5) {
                negativesRawList.forEach( negative => {
                    negative = negative.trim()
                    if(negative != "" && this.negatives.length < 5) {
                        this.negatives.push(negative.charAt(0).toUpperCase() + negative.slice(1))
                    }
                })
            this.$emit("negativesUpdated", this.negatives)
            this.negativesRawText = ""
            } else {
                alert("Alt kısmı çok uzun tutmamak adına, listelere beşden fazla madde eklemeyelim diyorum.")
            }
        },

        clearLists () {
            console.log("list clear ran.")
            this.$emit('listsCleared')
            this.positives = []
            this.negatives = []


        },
    }
    
}
</script>

<style scoped>

hr{
    width: 70%;
    margin: 10px auto;
}

.list-elements-container {
    margin-top: 20px;
    margin-bottom: 20px;
    font-family: "Open Sans",Helvetica, sans-serif;
}

.list-elements-container__header{
    display: flex;
    width: 90%;
    justify-content: space-between;
    margin-bottom: 20px;
    align-items: center;
}

.list-elements-container__info {
    width: 90%;
    font-size: .80rem;
    margin-bottom: 10px;
    line-height: 1.2rem;

}

.list-elements-container__title {
    font-size: 2rem;
}

.list-elements-container__heading {

    font-size: 1.4rem;
    font-weight: 600;
    margin-bottom: 10px;

}

.list-elements-container__input_field {
    width: 90%;
    font-size: 1rem;
    resize: none;
    margin-bottom: 10px;
    padding: 10px;
   

}

.input-field-positive:focus {
    border: 1px solid transparent;
    outline: 1px solid rgb(49, 201, 49);
}


.input-field-negative:focus {
    border: 1 px solid transparent;
    outline: 1px solid rgb(209, 57, 11);
}

.add-button {
    padding: 12px 18px;
    text-decoration: none;
    border: 2px solid #2c3135;
    border-radius: 3px;
    letter-spacing: 0.1em;
    color: black;
    overflow: hidden;
    background-color: white;
    font-size: .9rem;
    cursor: pointer;
}

.add-button:hover{
    background-color: #2c3135;
    color: white;
    transition: all .7s;
}

.add-button:active {
    background-color: #2c3135;
    color: white;
    transform: scale(0.9);
    transition: all .2s;
    
}


.list-elements-textarea{

}

</style>