<template>
    <div class="edit-container">
        <div class="edit-header">
            <h2 class="imb-font-semi-bold">게시물 수정</h2>
        </div>
        <div class="edit-wrap">
            <form class="edit-box" enctype="multipart/form-data">
                <div class="edit-img">
                    <Swiper class="edit-swiper" :options="swiperOption" v-if="previewImgUrls">
                        <SwiperSlide class="edit-swiper-slide" v-for="(data, idx) in previewImgUrls" v-bind:key="idx">
                            <img :src="data" class="edit-swiper-img"/>
                        </SwiperSlide>
                        <div class="swiper-pagination" slot="pagination"></div>
                        <div class="swiper-button-prev" slot="button-prev"></div>
                        <div class="swiper-button-next" slot="button-next"></div>
                    </Swiper>
                    <input id="rg-img-selctor" type="file" ref="selectFile" multiple style="display:none;" @change="previewFile" accept="image/*" required class="btn btn-outline-secondary imb-font-semi-bold" />
                </div>
                <div class="edit-user">
                    <img :src="userProfile.profileUrl"/>
                    <div class="imb-font-semi-bold">{{userProfile.nickName}}</div>
                </div>
                <div class="imb-font-regular">
                    <select class="category-select-edit p-2" v-model="menu">
                        <option selected hidden value="0">어떤 종류의 글인가요?</option>
                        <option :value="ca.code" v-for="ca in category" :key="ca.code">{{ca.name}}</option>
                    </select>
                    <input class="edit-text edit-title"  placeholder="이 글의 제목은 무엇인가요?" v-model="boardInfo.title" required/>
                    <textarea class="edit-text edit-content"  placeholder="이 글의 내용은 무엇인가요?" v-model="boardInfo.content" required/>
                </div>
            </form>
            <div v-if="menu == 0 || menu == 'D01'" class="write-btn">
            <label className="input-file-button" for="rg-img-selctor" class="btn btn-outline-secondary imb-font-semi-bold">사진 선택</label>&nbsp;&nbsp;
            <button type="button" class="btn btn-outline-secondary imb-font-semi-bold" @click="register" :disabled="simpleDisabled">자유글 등록</button>
        </div>
        <div v-else-if="menu == 'D02'" class="write-btn">
            <label className="input-file-button" for="rg-img-selctor" class="btn btn-outline-secondary imb-font-semi-bold">사진 선택</label>&nbsp;&nbsp;
            <button type="button" class="btn btn-outline-secondary imb-font-semi-bold" @click="zebraPredict" :disabled="imageEmptyCheck">얼룩말 검사</button>&nbsp;&nbsp;
            <button type="button" class="btn btn-outline-secondary imb-font-semi-bold" @click="register" :disabled="zebraDisabled">게시물 등록</button>
            <div id="zebra-edit-loding-div" class="check-loading-div">
                <img class="loading-image-icon" src="@/assets/icon/image.png" alt="">
                <div class="wavy imb-font-semi-bold">
                    <span style="--i:1;">얼</span>
                    <span style="--i:2;">룩</span>
                    <span style="--i:3;">말</span>
                    <span style="--i:4;">&nbsp;</span>
                    <span style="--i:5;">찾</span>
                    <span style="--i:8;">는</span>
                    <span style="--i:9;">&nbsp;</span>
                    <span style="--i:10;">중</span>
                    <span style="--i:11;">.</span>
                    <span style="--i:12;">.</span>
                </div>
            </div>
        </div>
        <div v-else-if="menu == 'D03'" class="write-btn">
            <label className="input-file-button" for="rg-img-selctor" class="btn btn-outline-secondary imb-font-semi-bold">사진 선택</label>&nbsp;&nbsp;
            <button type="button" class="btn btn-outline-secondary imb-font-semi-bold" @click="bottlePredict" :disabled="imageEmptyCheck">텀블러/컵 검사</button>&nbsp;&nbsp;
            <button type="button" class="btn btn-outline-secondary imb-font-semi-bold" @click="register" :disabled="bottleDisabled">게시물 등록</button>
            <div id="bottle-edit-loding-div" class="check-loading-div">
                <img class="loading-image-icon" src="@/assets/icon/image.png" alt="">
                <div class="wavy imb-font-semi-bold">
                    <span style="--i:1;">텀</span>
                    <span style="--i:2;">블</span>
                    <span style="--i:3;">러</span>
                    <span style="--i:4;">&nbsp;</span>
                    <span style="--i:5;">찾</span>
                    <span style="--i:8;">는</span>
                    <span style="--i:9;">&nbsp;</span>
                    <span style="--i:10;">중</span>
                    <span style="--i:11;">.</span>
                    <span style="--i:12;">.</span>
                </div>
            </div>
        </div>
        <div v-else-if="menu == 'D04'" class="write-btn">
            <label className="input-file-button" for="rg-img-selctor" class="btn btn-outline-secondary imb-font-semi-bold">사진 선택</label>&nbsp;&nbsp;
            <button type="button" class="btn btn-outline-secondary imb-font-semi-bold" @click="illustratedPredict" :disabled="imageEmptyCheck">동물도감 검사</button>&nbsp;&nbsp;
            <button type="button" class="btn btn-outline-secondary imb-font-semi-bold" @click="register" :disabled="illustratedDisabled">게시물 등록</button>
            <div id="illustrated-edit-loding-div" class="check-loading-div">
                <img class="loading-image-icon" src="@/assets/icon/image.png" alt="">
                <div class="wavy imb-font-semi-bold">
                    <span style="--i:1;">미</span>
                    <span style="--i:2;">션</span>
                    <span style="--i:3;">&nbsp;</span>
                    <span style="--i:4;">사</span>
                    <span style="--i:5;">진</span>
                    <span style="--i:8;">&nbsp;</span>
                    <span style="--i:9;">확</span>
                    <span style="--i:10;">인</span>
                    <span style="--i:11;">&nbsp;</span>
                    <span style="--i:12;">중</span>
                </div>
            </div>
        </div>
        <div v-else-if="menu == 'D05'" class="write-btn">
            <label className="input-file-button" for="rg-img-selctor" class="btn btn-outline-secondary imb-font-semi-bold">사진 선택</label>&nbsp;&nbsp;
            <button type="button" class="btn btn-outline-secondary imb-font-semi-bold" @click="register" :disabled="simpleDisabled">환경일기 등록</button>
        </div>
        <div v-else-if="menu == 'D06'" class="write-btn">
            <label className="input-file-button" for="rg-img-selctor" class="btn btn-outline-secondary imb-font-semi-bold">사진 선택</label>&nbsp;&nbsp;
            <button type="button" class="btn btn-outline-secondary imb-font-semi-bold" @click="interestPredict" :disabled="imageEmptyCheck">환경관심도 검사</button>&nbsp;&nbsp;
            <button type="button" class="btn btn-outline-secondary imb-font-semi-bold" @click="register" :disabled="interestDisabled">게시물 등록</button>
            <div id="interest-edit-loding-div" class="check-loading-div">
                <img class="loading-image-icon" src="@/assets/icon/image.png" alt="">
                <div class="wavy imb-font-semi-bold">
                    <span style="--i:1;">미</span>
                    <span style="--i:2;">션</span>
                    <span style="--i:3;">&nbsp;</span>
                    <span style="--i:4;">사</span>
                    <span style="--i:5;">진</span>
                    <span style="--i:8;">&nbsp;</span>
                    <span style="--i:9;">확</span>
                    <span style="--i:10;">인</span>
                    <span style="--i:11;">&nbsp;</span>
                    <span style="--i:12;">중</span>
                </div>
            </div>
        </div>
        </div>
    </div>
</template>

<script>
    import * as mobilenet from '@tensorflow-models/mobilenet';
    import * as tmImage from '@teachablemachine/image';
    import axios from "axios";
    import { mapGetters } from "vuex";
    import { Swiper, SwiperSlide } from 'vue-awesome-swiper'
    import 'swiper/css/swiper.css'
    export default {
        components: {
            Swiper,
            SwiperSlide
        },

        data() {
            return{
                userInfo: {},
                boardInfo:{
                    title: "",
                    content: ""
                },
                selectFiles:[],
                previewImgUrls:[],
                swiperOption: {
                    slidesPerView: 1,
                    spaceBetween: 30,
                    loop: false,
                    pagination: {
                        el: '.swiper-pagination',
                        clickable: true
                    },
                    navigation: {
                        nextEl: '.swiper-button-next',
                        prevEl: '.swiper-button-prev'
                    }
                },
                category:{},
                menu: 0,
                isZebra: false,
                isBottle: false,
                isIllustrated: false,
                isInterest: false,
            }
        },

        props: {
            info: []
        },

        methods: {
            previewFile(e) {
                this.isZebra = false;
                this.isBottle = false;
                this.isInterest = false;
                this.isIllustrated = false;
                this.selectFiles=[];
                this.previewImgUrls=[];
                // 선택된 파일이 있는가?
                if (e.target.files.length > 0) {
                    for(var i=0; i<e.target.files.length; i++){
                        // i 번째 파일을 가져 온다.
                        var file = e.target.files[i];
                        // 확장자 명 가져오기
                        var fileExt = file.name.substring(file.name.lastIndexOf(".") + 1); 
                        fileExt = fileExt.toLowerCase()
                        // 이미지 확장자 체크, 3메가 바이트 이하 인지 체크 
                        if(["jpeg", "png", "gif", "bmp", "jpg", "jfif"].includes(fileExt)
                                && file.size <= 3145728){
                            this.selectFiles.push(file);
                            this.previewImgUrls.push(URL.createObjectURL(file));
                        }
                        else {
                            this.$fire({title: "3MB 이하의 이미지 파일만 가능합니다.", type: "error", timer: 1000, showConfirmButton: false})
                        }
                    }
                }
            },
            async dataURLToBlob(url){
                const response = await fetch(url);
                const data = await response.blob();
                const ext = url.split(".").pop(); // url 구조에 맞게 수정할 것
                const filename = url.split("/").pop(); // url 구조에 맞게 수정할 것
                const metadata = { type: `image/${ext}` };
                return new File([data], filename, metadata);
            },
            register() {
                if (this.boardInfo.title == "" || this.selectFiles.length == 0 || this.boardInfo.content == ""){
                    this.$fire({title: "사진이나 내용을 넣어주세요", text: "사진이나 내용 없음", type: "error", timer: 1000, showConfirmButton: false})
                }
                else{
                    const uploadBoard = new FormData();
                    uploadBoard.append("title", this.boardInfo.title);
                    uploadBoard.append("userId", this.userInfo.id);
                    for (var i = 0; i < this.selectFiles.length; i++){
                        uploadBoard.append("fileList", this.selectFiles[i]);
                    }
                    uploadBoard.append("content", this.boardInfo.content);
                    uploadBoard.append("code", this.menu);
                    // for (var pair of uploadBoard.entries()) { console.log(pair[0]+ ' : ' + pair[1]); }
                    axios.patch('/api/board/' + this.info.id, uploadBoard,  { headers: { 'Content-Type': 'multipart/form-data' } })
                    .then(res => {  // eslint-disable-line no-unused-vars
                        this.$fire({title: "수정 되었습니다!", text: "완료", type: "success", timer: 1000, showConfirmButton: false})
                        this.boardInfo.title="";
                        this.boardInfo.content="";
                        this.selectFiles=[];
                        this.previewImgUrls=[];
                        this.$emit("list");
                    })
                    .catch(err => {
                        console.log(err);
                    });
                }
            },
            async zebraPredict(){
                document.getElementById("zebra-loding-div").style.display = "block";
                const URI = "https://teachablemachine.withgoogle.com/models/"
                const URL = URI + "nokvKV3oP/";
                const modelURL = URL + "model.json";
                const metadataURL = URL + "metadata.json";

                const model = await tmImage.load(modelURL, metadataURL);
                const maxPredictions = model.getTotalClasses();

                let length = document.getElementsByClassName('edit-swiper-img').length;
                for (let i = 0; i < length; i++) {
                    const prediction = await model.predict(document.getElementsByClassName("edit-swiper-img").item(i));

                    const classPrediction = {};
                    let maxLabel = null;
                    let maxValue = 0.00;

                    for (let p = 0; p < maxPredictions; p++) {
                        classPrediction[prediction[p].className] = prediction[p].probability.toFixed(2);
                        if(maxValue < prediction[p].probability.toFixed(2)){
                            maxLabel = prediction[p].className;
                            maxValue = prediction[p].probability.toFixed(2);
                        }
                    }
                    // console.log(i+'번째 분류 = ', classPrediction);

                    if(maxLabel == 'safers-zebra')
                        this.isZebra = true;
                    
                    if (i == length - 1){
                        document.getElementById("zebra-loding-div").style.display = "none";
                        if (this.isZebra){
                            console.log("safers의 얼룩말이 포함되어 있습니다!")
                            this.$fire({title: "찾았어요!", text:"Safers의 얼룩말이 최소 한장이 포함되있습니다!", type: "success", timer: 1500, showConfirmButton: false})
                        } else{
                            console.log("아무 관련 없는 사진인걸?")
                            this.$fire({title: "못찾았어요..", text:"얼룩말의 미션을 들어주세요.", type: "warning", timer: 1500, showConfirmButton: false});
                        }
                    }
                }
            },
            async bottlePredict(){
                const findObject = [];
                const correct = ["bottle", "mug", "cup"];
                let length = document.getElementsByClassName('edit-swiper-img').length;                
                document.getElementById("bottle-edit-loding-div").style.display = "block";

                mobilenet.load().then(model => {
                        
                    for (let i = 0; i < length; i++) {
                        model.classify(document.getElementsByClassName("edit-swiper-img").item(i)).then(predictions => {
                            predictions.forEach(e=>{
                                const temp = e.className.replace(',', '');
                                const tempResult = temp.split(' ');
                                tempResult.forEach(result => {
                                    if(correct.includes(result)){
                                        console.log(result + " 있따!");
                                        this.isBottle = true;
                                    }
                                    if(!findObject.includes(result))                                        
                                        findObject.push(result);
                                })
                            })
                            if (i == length - 1){
                                document.getElementById("bottle-edit-loding-div").style.display = "none";
                                console.log('끝!');
                                console.log(findObject)
                                if(this.isBottle)
                                    this.$fire({title: "찾았어요!", text:"컵이나 텀블러가 최소 한장이 포함되있습니다!", type: "success", timer: 1500, showConfirmButton: false});
                                else 
                                    this.$fire({title: "못찾았어요..", text:"얼룩말의 미션을 들어주세요.", type: "warning", timer: 1500, showConfirmButton: false});
                            }
                        });
                    }
                });
            },
            async illustratedPredict(){
                const URI = "https://teachablemachine.withgoogle.com/models/"
                const URL = URI + "0TuN2n1J6/";
                const modelURL = URL + "model.json";
                const metadataURL = URL + "metadata.json";
                document.getElementById("illustrated-edit-loding-div").style.display = "block";

                const model = await tmImage.load(modelURL, metadataURL);
                const maxPredictions = model.getTotalClasses();

                let length = document.getElementsByClassName('edit-swiper-img').length;
                for (let i = 0; i < length; i++) {
                    const prediction = await model.predict(document.getElementsByClassName("edit-swiper-img").item(i));

                    const classPrediction = {};
                    let maxLabel = null;
                    let maxValue = 0.00;

                    for (let p = 0; p < maxPredictions; p++) {
                        classPrediction[prediction[p].className] = prediction[p].probability.toFixed(2);
                        if(maxValue < prediction[p].probability.toFixed(2)){
                            maxLabel = prediction[p].className;
                            maxValue = prediction[p].probability.toFixed(2);
                        }
                    }
                    // console.log(classPrediction);

                    if(maxLabel == 'illustracted')
                        this.isIllustrated = true;
                    
                    if (i == length - 1){
                        document.getElementById("illustrated-edit-loding-div").style.display = "none";
                        if (this.isIllustrated) 
                            this.$fire({title: "찾았어요!", text:"도감 사진이 최소 한장이 포함되있습니다!", type: "success", timer: 1500, showConfirmButton: false});
                        else
                            this.$fire({title: "못찾았어요..", text:"도감 사진이 최소 한장이 포함되있습니다!", type: "warning", timer: 1500, showConfirmButton: false});
                    }
                }
            },
            async interestPredict(){
                const URI = "https://teachablemachine.withgoogle.com/models/"
                const URL = URI + "MwA8PPduz/";
                const modelURL = URL + "model.json";
                const metadataURL = URL + "metadata.json";
                document.getElementById("interest-edit-loding-div").style.display = "block";

                const model = await tmImage.load(modelURL, metadataURL);
                const maxPredictions = model.getTotalClasses();

                let length = document.getElementsByClassName('edit-swiper-img').length;
                for (let i = 0; i < length; i++) {
                    const prediction = await model.predict(document.getElementsByClassName("edit-swiper-img").item(i));

                    const classPrediction = {};
                    for (let p = 0; p < maxPredictions; p++) {
                        classPrediction[prediction[p].className] = prediction[p].probability.toFixed(2);
                    }
                    // console.log(i+'번째 분류 = ', classPrediction);

                    if (classPrediction["true"] > classPrediction["false"])
                        this.isInterest = true;
                    
                    if (i == length - 1){
                        document.getElementById("interest-edit-loding-div").style.display = "none";
                        if (this.isInterest) 
                            this.$fire({title: "찾았어요!", text:"미션 사진이 최소 한장이 포함되있습니다!", type: "success", timer: 1500, showConfirmButton: false});
                        else
                            this.$fire({title: "못찾았어요..", text:"미션 사진이 최소 한장이 포함되있습니다!", type: "warning", timer: 1500, showConfirmButton: false});
                    }
                }
            }
        },

        computed: {
            ...mapGetters({
                userProfile: "user/userProfile",
            }),
            imageEmptyCheck(){
              if (this.selectFiles.length > 0)
                return false;
              return true;
            },
            simpleDisabled(){
              // simple data check function - menu selected, title write, content write, select image file minimum
              if(this.selectFiles.length > 0 && this.menu != "0" && this.boardInfo.title.length > 0 && this.boardInfo.content.length > 0)
                  return false;
              return true;
            },
            zebraDisabled(){
              // zebra image check function - menu selected, title write, content write, select image file minimum
              if(this.isZebra && this.selectFiles.length > 0 && this.menu != "0" && this.boardInfo.title.length > 0 && this.boardInfo.content.length > 0)
                  return false;
              return true;
            },
            bottleDisabled(){
              // bottle image check function - menu selected, title write, content write, select image file minimum
              if(this.isBottle && this.selectFiles.length > 0 && this.menu != "0" && this.boardInfo.title.length > 0 && this.boardInfo.content.length > 0)
                  return false;
              return true;
            },
            illustratedDisabled(){
              // bottle image check function - menu selected, title write, content write, select image file minimum
              if(this.isIllustrated && this.selectFiles.length > 0 && this.menu != "0" && this.boardInfo.title.length > 0 && this.boardInfo.content.length > 0)
                  return false;
              return true;
            },
            interestDisabled(){
              // bottle image check function - menu selected, title write, content write, select image file minimum
              if(this.isInterest && this.selectFiles.length > 0 && this.menu != "0" && this.boardInfo.title.length > 0 && this.boardInfo.content.length > 0)
                  return false;
              return true;
            },
        },

        async mounted() {
            this.userInfo = { ...this.userProfile };
            for(var i=0; i<this.info.fileList.length; i++){
                this.previewImgUrls.push(this.info.fileList[i].filePath);
                this.selectFiles.push(await this.dataURLToBlob(this.info.fileList[i].filePath)
                .then(function(result){
                    return result;
                }))
            }
            this.boardInfo.title = this.info.title;
            this.boardInfo.content = this.info.content;
            this.menu = this.info.code.code;
        },

        created(){
            axios.get('/api/commoncode/category')
            .then(res => {
                this.category = res.data[0].code;
            })
            .catch(err => {
                console.log(err);
            })
        }
    }
</script>
<style>
    .edit-container {
        height: 94%;
    }
    .edit-header {
        display: grid;
        grid-template-columns: 1fr 1fr 1fr;
        align-items: center;
        margin-bottom: 10px;
        margin-top: -10px;
    }
    .edit-header > h2 {
        grid-column: 2/3;
        justify-self: center;
        margin-bottom: 0 !important;
    }
    .category-select-edit{
        width:100%;
        border: none;
        border-bottom: 1px rgb(220, 220, 220) solid;
        color: rgb(94, 94, 94);
    }
    .category-select-edit:focus{
        outline: none;
    }
    .edit-wrap{
        height: 100%;
    }
    .edit-box{
        display: grid;  
        grid-template-columns: 2fr 1fr;
        grid-template-rows: 1fr 10fr;
        border: 1px rgb(220, 220, 220) solid;
        height: 100%;
    }
    .edit-img{
        border-right: 1px rgb(230, 230, 230) solid;
        grid-column: 1 / 2;
        grid-row: 1 / 3;
    }
    .edit-swiper {
        width: 100%;
        height: 100%;
        text-align: center;
        vertical-align: middle;
        border-right: 1px rgb(230, 230, 230) solid;
        background-color: rgb(250, 250, 250);
    }
    .edit-swiper-slide {
        width: 100% !important;
    }
    .edit-swiper-img {
        height:100%; 
        width:100%; 
        object-fit: contain;
    }
    .edit-btn{
        display: flex;
        align-items: center;
        justify-content: center;
        height: 10%;
        margin-top: 0.5vw;
    }
    .edit-user {
        display: flex;
        align-items: center;
        padding: 2px;
        border-bottom: 1px rgb(230, 230, 230) solid;
    }
    .edit-user > img{
        height: 3.5vh; 
        width: 3.5vh; 
        border-radius: 100px;
        margin-left: 10px;
        margin-right: 10px;
    }
    .edit-user > div{
        font-size: 1.5vh;
    }
    .edit-text {
        border: none;
        width: 100%;
        font-size: 1.5vh;
    }
    .edit-text:focus {
        outline: none;
    }
    .edit-title {
        height: 10%;
        padding: 10px;
    }
    .edit-content {
        resize: none;
        overflow: auto;
        height: 80%;
        border-top: 1px rgb(230, 230, 230) solid;
        padding: 10px;
    }
    .edit-content::-webkit-scrollbar {
        display: none;
    }
</style>
