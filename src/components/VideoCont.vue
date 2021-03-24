<template>
    <div class="spext-frame">
        <div class="spext-flex spex-sampleVideo-wrp">
            <div id="spext-canvas-ratio">
                <div class="spext-hdg">Sample Video</div>
                <!-- <img :src="require('../assets/img/p_1.png')"> -->
                <div class="spect-canvas-outerWrp">
                    <div class="spect-canvas-innerWrp spext-relative">
                        <canvas id="canvas" ref="canvas" width="480" height="360"></canvas>
                        <div class="spext-Frametext">{{ message || 'Text entered in the box below appears as subtitle here' }}</div>
                    </div>
                </div>
            </div>
            <div class="spext-frame-settings">
                <div class="spext-hdg">Size Option</div>
                <ul>
                    <li @click="toggleBodyClass('addClass', '')">
                        <img :src="require('../assets/img/icons/original_icon.svg')"> 
                        <div>Original</div>
                    </li>
                    <li @click="toggleBodyClass('addClass', 'spext-lands')">
                        <img :src="require('../assets/img/icons/landscape_icon.svg')"> 
                        <div>
                            Landscape 
                            <div class="spext-specs">16:9</div>
                        </div>
                    </li>
                    <li @click="toggleBodyClass('addClass', 'spext-portrait')">
                        <img :src="require('../assets/img/icons/portrait_icon.svg')"> 
                        <div>
                            Portrait 
                            <div class="spext-specs">9:16</div>
                        </div>
                    </li>
                    <li @click="toggleBodyClass('addClass', 'spext-square')">
                        <img :src="require('../assets/img/icons/square_icon.svg')"> 
                        <div>
                            Square 
                            <div class="spext-specs">1:1</div>
                        </div>
                    </li>
                </ul>
            </div>
        </div>
        <div class="spext-flex spext-frame-ctrls">
            <div class="spext-play-btn">
                <img :src="require('../assets/img/icons/play_icon.svg')" v-if="!pauseFrame" v-on:click="StartAnimation(); pauseFrame = true">
                <img :src="require('../assets/img/icons/pause_icon.svg')" v-if="pauseFrame" v-on:click="pauseFrame = false">
            </div>
            <input type="text" name="text" v-model="message" placeholder="Enter Subtitle Text here">
            <div>
                <a class="spext-btn spext-addFrame-btn" v-on:click="isHidden = false; AnimateNow(addFrameBtn());" v-if="isHidden">Add a frame</a>
                <div class="spext-relative" v-if="!isHidden">
                    <div v-if="!addFrame">
                        <span class="spext-close-btn spext-flex-center" v-on:click="isHidden = true">x</span>
                        <label for="spext-upload" class="spext-btn spext-upload-btn">Upload Frame</label>
                        <input id="spext-upload" type="file" accept="image/*" class="" @change="onFileChange">
                    </div>
                    <div>
                        <span class="spext-uploadedFileName">{{fileName}}</span>
                        <div v-if="addFrame" class="spext-btn spext-addFramebtn">
                           <div class="spext-center">
                                Length {{ frameTime }} secs
                                <div class="spext-addFrameSequence" v-on:click="AnimateNow(addFrameToCanvas())">Add</div>
                            </div>
                        </div>
                    </div>
                </div>
            </div>
        </div>
    </div>
</template>
<script>
    export default {
        name: 'VideoCont',
        // props: {
        //   msg: String
        // }
        data: () => ({
            message: '',
            isHidden: true,
            pauseFrame: false,
            addFrame: false,
            fileName: null,
            frameTime: null,
            isPlaying: false,
            currFrame: 0,
            uploadedFrame: '',
            frameArr: []
        }),
        methods: {
            StartAnimation: function() {
                this.isPlaying = true;
                this.AnimateNow();
            },

            AnimateNow: function() {
                let canvas = this.$refs.canvas;
                let ctx = canvas.getContext('2d');
                var img = new Image;

                let secs = 0;

                img.onload = function(){
                    ctx.clearRect( 0, 0, ctx.canvas.width, ctx.canvas.height );
                    ctx.drawImage( img, 0, 0 );
                    // ctx.font = "20px Calibri";
                    // ctx.fillText(this.message, 10, 10);
                };

                const req = require.context('../assets/frames/', true, /\.(png|jpe?g|svg)$/);
                var arr = req.keys();
                var index = 0;

                this.frameArr = arr;

                // console.log(this.frameArr);

                if(this.isPlaying) {
                    var timer = setInterval(function(){
                        secs++
                        if(index == arr.length){
                            clearInterval(timer);
                        } else {
                            // let src = 'frames/' + arr[index++].split('/')[1];
                            let src = req(arr[index++]);
                            img.src = src
                         }
                    }, 1000/10);
                }                    

                // console.log(this.isPlaying);

                // Pause Canvas Animation
                this.PauseAnimaton = function () {
                  clearInterval(timer);
                  this.isPlaying = false;
                  this.currFrame = index;
                  // console.log(secs);
                };

                this.addFrameBtn = function() {
                    clearInterval(timer);
                    this.isPlaying = false;
                    this.currFrame = index;
                    this.frameTime = Math.round(secs/10);
                }

                this.addFrameToCanvas = function() {
                    let reader = new FileReader();
                    reader.readAsDataURL(this.uploadedFrame);
                    reader.onload = (event) => {
                        this.uploadedFrame = event.target.result;
                        this.frameArr.splice(this.currFrame, 0, this.uploadedFrame);
                        console.log(this.frameArr);
                    }
                }
            },

            // Canvas Aspect ratio
            toggleBodyClass(addRemoveClass, className) {
                const el = document.getElementById('spext-canvas-ratio');
                const elClass = el.classList
                elClass.remove('spext-lands'); elClass.remove('spext-portrait'); elClass.remove('spext-square');
        
                if (addRemoveClass === 'addClass' && !el.classList.contains(className)) {
                    el.classList.add(className);
                } else {
                    el.classList.remove(className);
                }
            },

            // Input file name function
            onFileChange(event){
               this.uploadedFrame =  event.target.files[0];
               this.fileName = this.uploadedFrame.name;
               this.addFrame = true;
            }
        },
        mounted: function (){
            // this.StartAnimation();
        }
    }
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
    ul {
        list-style-type: none;
        padding: 0;
    }
</style>