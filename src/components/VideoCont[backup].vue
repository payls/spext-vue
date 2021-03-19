<template>
    <div class="spext-frame">
        <div class="spext-flex">
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
                <img :src="require('../assets/img/icons/play_icon.svg')">
            </div>
            <input type="text" name="text" v-model="message" placeholder="Enter Subtitle Text here">
            <div>
                <a class="spext-btn spext-addFrame-btn" v-on:click="isHidden = false" v-if="isHidden">Add a frame</a>
                <div class="spext-relative" v-if="!isHidden">
                    <div v-if="!addFrame">
                        <span class="spext-close-btn spext-flex-center" v-on:click="isHidden = true">x</span>
                        <label for="spext-upload" class="spext-btn spext-upload-btn">Upload Frame</label>
                        <input id="spext-upload" type="file" class="" @change="onFileChange">
                    </div>
                    <div>
                        <span class="spext-uploadedFileName">{{fileName}}</span>
                        <div v-if="addFrame" class="spext-btn spext-addFramebtn">
                           <div class="spext-center">
                                Length 10 secs
                                <div class="spext-addFrameSequence">Add</div>
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
            addFrame: false,
            fileName: null
        }),
        methods: {
            StartAnimation: function() {
                this.AnimateNow();
            },
    
            AnimateNow: function() {
                let animRef;
                // let imgNumber = 1;
                // let lastImgNumber = 11272;
                let canvas = this.$refs.canvas;
                let ctx = canvas.getContext('2d');
                const isPlaying = false;
        
                // let naming = 'frames/s_000#####.jpg'
                // let replacee = naming.match(/#/g).join('');
                // let suffix = replacee.replace(/#/g, '0');

                this.init = function() {
                    for (var i = 0; i < 11272; i++) {
                        let naming = 'frames/s_000#####.jpg'
                        let replacee = naming.match(/#/g).join('');
                        let suffix = replacee.replace(/#/g, '0');
                        let src = naming.replace(replacee, (suffix.substring(1) + i).slice(suffix.length * -1));

                        const img = new Image();
                        img.id = i+1;

                        img.src = src;
                    }
                }

                this.doAnim = function (time) {
                    isPlaying = true;
                    let delta = (time - this.curTime) / 1000;
                    cnt += (delta * this.fps);
                  // var curFrame = Math.max(Math.floor(this.cnt + this.start), 0);

                  // if (curFrame > this.end) {
                  //   this.cnt = 0;
                  //   curFrame = this.start;
                  //   if (!this.isLoop) {
                  //     cancelAnimationFrame(this.animRef);
                  //     if (typeof this.callback == 'function') this.callback();
                  //     return;
                  //   }
                  }
                
                // var ctx = canvas.getContext('2d');
                // var img = new Image;
                // img.onload = function(){
                //     ctx.clearRect( 0, 0, ctx.canvas.width, ctx.canvas.height );
                //     ctx.drawImage( img, 0, 0 );
                //     // ctx.font = "20px Calibri";
                //     // ctx.fillText(this.message, 10, 10);
                // };

                // var timer = setInterval( function(){
                //     if (imgNumber > lastImgNumber){
                //         clearInterval( timer );
                //     } else {
                //         var src = naming.replace(replacee, (suffix.substring(1) + imgNumber++).slice(suffix.length * -1));
                //         img.src = src
                //     }
                // }, 1000/10 ); //Draw at 10 frames per second

                // this.pauseAnim = function () {
                //   cancelAnimationFrame(this.animRef);
                //   this.isPlaying = false;
                // };

                this.init();
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
               var fileData =  event.target.files[0];
               this.fileName = fileData.name;
               this.addFrame = true
            }
        },
        mounted: function (){
            this.StartAnimation();
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