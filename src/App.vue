<template>
  <div id="app">
    <img alt="Vue logo" src="./assets/logo.png">
    <HelloWorld msg="Welcome to Your Vue.js App"/>
   <clipper-basic
        class="my-clipper-upload"
        ref="clipper"
        :src="'http://gestudios.com/assets/img/carlos-guevara.jpg'"
        preview="my-preview"
        :rotate="rotation"
        shadow="rgba(0,0,0,0.6)"
        @load="imgLoad">

    </clipper-basic>
    <clipper-preview name="my-preview" class="my-clipper-upload" ref="preview">
        <div class="placeholder" slot="placeholder">preview area</div>
    </clipper-preview>
    <div class="form-row">
        <div class="col-md-6" >
            <clipper-range v-model="rotation"  style="max-width:300px" :min="0" :max="360"></clipper-range>
        </div>
        <div class="col-md-6">
            <div class="edit-image-tool">
                <div class="btn-group btn-upload-tickets-md" role="group">
                    <button class="btn btn-upload-tickets-md" @click.prevent="getResult" title="cropper.crop()"><i class="fa fa-check" aria-hidden="true"></i></button>
                    <button type="button" class="btn btn-upload-tickets-md" @click="cropper.clear()" title="cropper.clear()"><i class="fa fa-remove" aria-hidden="true"></i></button>
                </div>
            </div>
        </div>
    </div>
    <div class="modal-footer">
        <button type="button" class="btn btn-secondary" @click.prevent="closeCrop">Cerrar</button>
        <button type="button" class="btn btn-upload-tickets-bg" @click.prevent="saveImage">Guardar</button>
    </div>
    Probando github.dev
    <img src="http://gestudios.com/assets/img/carlos-guevara.jpg"  id="myimage">
  </div>
</template>

<script>
import HelloWorld from './components/HelloWorld.vue'

/*function convertImgToBase64(url, callback, outputFormat){

  var canvas = document.createElement('CANVAS'),
    ctx = canvas.getContext('2d'),
    img = new Image;
  img.crossOrigin = 'Anonymous';
  img.onload = function(){
    canvas.height = img.height;
    canvas.width = img.width;
    ctx.drawImage(img,0,0);
    var dataURL = canvas.toDataURL(outputFormat || 'image/png');
    callback.call(this, dataURL);
    canvas = null;
  };
  img.src = url;
}
*/

export default {
  name: 'App',
  data() {
      return {
          downloadedImg: null
      }
  },
  components: {
    HelloWorld
  },
  mounted(){
      let that = this;
      let imageURL = "https://cdn.glitch.com/4c9ebeb9-8b9a-4adc-ad0a-238d9ae00bb5%2Fmdn_logo-only_color.svg?1535749917189";

    this.downloadedImg = new Image;
    this.downloadedImg.crossOrigin = "Anonymous";
    this.downloadedImg.src = imageURL;
    this.downloadedImg.addEventListener("load", imageReceived, false);
    function imageReceived() {
        console.log("imagen cargada", that.downloadedImg)
        let canvas = document.createElement("canvas");
        let context = canvas.getContext("2d");

        canvas.width = that.downloadedImg.width;
        canvas.height = that.downloadedImg.height;

        context.drawImage(that.downloadedImg, 0, 0);

        try {
            console.log(canvas.toDataURL("image/jpg"));
            localStorage.setItem("saved-image-example", canvas.toDataURL("image/jpg"));
        }
        catch(err) {
            console.log("Error: " + err);
        }
        }
      /*
      var xhr = new XMLHttpRequest();
        xhr.open("GET", "http://gestudios.com/assets/img/carlos-guevara.jpg");
        xhr.responseType = "blob";
        xhr.onload = response;
        xhr.send();
        function response() {
            var urlCreator = window.URL || window.webkitURL;
            var imageUrl = urlCreator.createObjectURL(this.response);
            console.log(imageUrl)
            //document.querySelector("#image").src = imageUrl;
        }
        */
  },
  methods: {
        getResult() {
            this.showPreview = true
            const canvas = this.$refs.preview.clip();
            this.file = canvas.toBlob((blob) => {
                return new File([blob], "fileName.jpg", { type: "image/jpeg" })
            }, 'image/jpeg');
            /* let binStr = atob(canvas.toDataURL('image/jpg').split(',')[1])
            let arr = new Uint8Array(binStr.length)
            for (let i = 0; i < binStr.length; i++) {
            arr[i] = binStr.charCodeAt(i)
            }
            this.file = new File([arr], 'nombre', { type: 'image/jpg' }) */
            console.log(this.file)
        },
        saveImage() {
            fetch('http://gestudios.com/assets/img/carlos-guevara.jpg').then(function(response) {
            return response.blob();
            }).then(function(myBlob) {
            var objectURL = URL.createObjectURL(myBlob);
                console.log(objectURL);
            });
/*
             var xhr = new XMLHttpRequest();
            xhr.open("GET", "http://gestudios.com/assets/img/carlos-guevara.jpg");
            xhr.responseType = "blob";
            xhr.onload = response;
            xhr.send();
            function response() {
                var urlCreator = window.URL || window.webkitURL;
                var imageUrl = urlCreator.createObjectURL(this.response);
                console.log(imageUrl)
                //document.querySelector("#image").src = imageUrl;
            }
            */
           //const canvas = this.$refs.clipper.clip();
           // let resultURL = canvas.toDataURL("image/jpeg", 1);
           // console.log(resultURL)
        },
        imgLoad() {
            const imgRatio = this.$refs.clipper.imgRatio;
            if(imgRatio < 1) this.based =  this.maxHeight*imgRatio;
            else this.based = this.maxWidth;
        },
        send() {
            //Ocr.send(this.file, 'ticket', this.companyID)
            //this.$router.push('/mis-tickets');
        },
        closeCrop() {
            this.show = false
            this.$emit('closeCrop')
        }
    },
}
</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
</style>
