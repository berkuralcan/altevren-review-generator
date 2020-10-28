<template>
<div class="grade-selector-container">
    <h2 class="grade-selector__title"> Çizgi Romana Vermek İstediğiniz "Notu" Seçin! </h2>
    <div class="grade-images-container">
        <div v-for="(image, index) in images" 
        @mouseenter="displayExplanation(index)"
        @click="selectGrade(index)"
        class="grade-images">
            <img class="grade-image" :src="image.url" alt="grade-image">
        </div>
    </div>


    <div class="grade-image_details">
        <h3 class="grade-image-explanation_title">Not Açıklaması: {{hoveredGrade.title}}</h3>
        <div class="grade-image_expanation"> {{hoveredGrade.explanation}} </div>
    </div>

</div>
</template>

<script>
export default {
    name: "GradeSelect",
    data() {
        return {
            selectedGrade: "",
            hoveredGrade: "",
            hello: "Hello World"
        }
    },

    props: ["images",],

    methods: {
        selectGrade(index) {
            this.selectedGrade = this.images[index]
            this.$emit("gradeSelected", index)
        },

        displayExplanation(index) {
            console.log("Ran!")
            this.hoveredGrade = this.images[index]
        }
    }
    
}
</script>

<style scoped>

*{
    box-sizing: border-box;
    margin: 0;
    padding: 0;
}

.grade-selector-container {
    z-index:1;
    padding: 10px;
    border: 1px solid #eee;
    margin-top: 20px;
    margin-bottom: 20px;
}

.grade-selector__title {
    margin-bottom: 20px;
    font-family:  Helvetica, sans-serif;
    letter-spacing: 0.05;
}

.grade-images-container {
    display: grid;
    grid-template-columns: 1fr 1fr 1fr;
    grid-template-rows: 1fr 1fr 1fr;
    border-top: 1px solid gray;
    border-left: 1px solid gray;
}

.grade-images {
    display: flex;
    cursor: pointer;
    padding: 10px;
    justify-content: center;
    border-right: 1px solid gray;
    border-bottom: 1px solid gray;
}

.grade-images:hover{
    background-color: rgb(212, 212, 212);
}


.grade-image {
    max-width: 80px;
    max-height: 80px;
}

.grade-image_details{
    max-width: 500px;
}

.grade-image-explanation_title {
    margin-top: 20px;
    margin-bottom: 20px;
    font-size: 1.2rem;
    font-family: Arial, Helvetica, sans-serif;
}

.grade-image_expanation {
    font-family: "Open Sans", sans-serif;
    font-weight: 400;
    font-size: .93rem;
    line-height: 1.73rem;
    letter-spacing: 0.02px;
}

</style>