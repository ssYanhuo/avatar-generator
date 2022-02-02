<template>
  <v-container>
    <v-row class="text-center" justify="center">
      <v-col center cols="10">
        <v-stepper v-model="stepNow" class="mt-8" vertical>
          <v-stepper-step :complete="stepNow > 1" step="1">
            上传原始头像
          </v-stepper-step>

          <v-stepper-content step="1">
            <v-card class="mb-4" color="grey lighten-2" height="200px" outlined @click="uploadFile">
              <input id="uploadBox" accept="image/*" style="display: none" type="file" @change="setImagePath">
              <div v-if="imagePath === ''" style="transform: translateY(50%)">
                <v-icon size="96">mdi-file-upload</v-icon>
              </div>
              <div v-else style="transform: translateY(35%)">
                <v-row class="text-center" justify="center">
                  <div>
                    <v-img :src="imagePath" height="128" width="128"/>
                  </div>
                </v-row>

              </div>
            </v-card>
            <div class="mb-4">
              <v-btn :disabled="imagePath === ''" color="second" @click="stepNow = 2">
                下一步
              </v-btn>
            </div>
          </v-stepper-content>

          <v-stepper-step :complete="stepNow > 2" step="2">
            选择一个头像框
          </v-stepper-step>

          <v-stepper-content step="2">
            <v-card class="mb-4" color="grey lighten-3" outlined>
              <v-radio-group v-model="selected">
                <v-row class="mt-2" justify="center" no-gutters>

                  <v-col class="text-center" cols="12" md="6">
                    <div style="display: inline-block; max-width: 128px;">
                      <v-img height="128" src="../assets/img1.png" width="128"/>
                      <v-radio style="display: inline-block; margin: 0" class="ma-2" value="1"/>
                    </div>


                  </v-col>
                  <v-col class="text-center" cols="12" md="6">
                    <div style="display: inline-block; max-width: 128px;">
                      <v-img height="128" src="../assets/img2.png" width="128"/>
                      <v-radio style="display: inline-block; margin: 0" class="ma-2" value="2"/>
                    </div>
                  </v-col>

                  <v-col class="text-center" cols="12" md="6">
                    <div style="display: inline-block; max-width: 128px;">
                      <v-img height="128" src="../assets/img3.png" width="128"/>
                      <v-radio style="display: inline-block; margin: 0" class="ma-2" value="3"/>
                    </div>
                  </v-col>

                  <v-col class="text-center" cols="12" md="6">
                    <div style="display: inline-block; max-width: 128px;">
                      <v-img height="128" src="../assets/img4.png" width="128"/>
                      <v-radio style="display: inline-block; margin: 0" class="ma-2" value="4"/>
                    </div>
                  </v-col>

                  <v-col class="text-center" cols="12" md="6">
                    <div style="display: inline-block; max-width: 128px;">
                      <v-img height="128" src="../assets/img5.png" width="128"/>
                      <v-radio style="display: inline-block; margin: 0" class="ma-2" value="5"/>
                    </div>
                  </v-col>

                  <v-col class="text-center" cols="12" md="6">
                    <div style="display: inline-block; max-width: 128px;">
                      <v-img height="128" src="../assets/img6.png" width="128"/>
                      <v-radio style="display: inline-block; margin: 0" class="ma-2" value="6"/>
                    </div>
                  </v-col>


                </v-row>
              </v-radio-group>
            </v-card>
            <div class="mb-4">
              <v-btn class="mr-4" color="second" @click="drawAvatar">
                下一步
              </v-btn>
              <v-btn depressed @click="stepNow = 1">
                返回
              </v-btn>
            </div>
          </v-stepper-content>

          <v-stepper-step step="3">
            大功告成！
          </v-stepper-step>
          <v-stepper-content step="3">
            <v-card class="mb-4" color="grey lighten-3" outlined>
              <canvas class="my-4" id="canvas" width="128" height="128">

              </canvas>
            </v-card>
            <div class="mb-4">
              <v-btn class="mr-4" color="second" @click="showDownloadDialog = true">
                保存
              </v-btn>
              <v-btn depressed @click="reload">
                再制作一张
              </v-btn>
            </div>

          </v-stepper-content>
        </v-stepper>

      </v-col>
      <v-col md="4" cols="6" center>
        <v-img src="../assets/logo.jpg"></v-img>
      </v-col>
      <v-col cols="12" center>
        <p style="font-size: small; color: gray">Developed by <a href="https://github.com/ssYanhuo">ssYanhuo</a></p>
      </v-col>
    </v-row>
    <v-dialog v-model="showDownloadDialog" max-width="290">
      <v-card>
        <v-card-title>
          下载头像
        </v-card-title>

        <v-card-text>
          <v-card color="grey lighten-3" outlined class="text-center pa-4 mb-4">
            <img height="128" width="128" :title="new Date().getTime()" :src="avatarData">
          </v-card>
          <p>如果你的浏览器不支持下载文件，请按住或右键单击图片并保存。</p>
        </v-card-text>

        <v-card-actions class="mt-n10">
          <v-spacer></v-spacer>

          <v-btn :href="avatarURL" :download="new Date().getTime()" color="second" text @click="showDownloadDialog = false">
            下载
          </v-btn>

          <v-btn text @click="showDownloadDialog = false">
            取消
          </v-btn>
        </v-card-actions>
      </v-card>
    </v-dialog>
  </v-container>
</template>

<script>
import img1 from '../assets/img1.png'
import img2 from '../assets/img2.png'
import img3 from '../assets/img3.png'
import img4 from '../assets/img4.png'
import img5 from '../assets/img5.png'
import img6 from '../assets/img6.png'
export default {
  name: 'Generator',

  data: () => ({
    stepNow: 1,
    imagePath: '',
    selected: '1',
    avatarURL: '',
    avatarData: '',
    showDownloadDialog: false,
  }),

  methods: {
    uploadFile: function () {
      document.getElementById('uploadBox').click()
    },
    setImagePath: function () {
      let that = this
      let input = document.getElementById('uploadBox')
      let image = input.files[0]
      let reader = new FileReader()
      reader.readAsDataURL(image)
      reader.onload = function (reader) {
        that.imagePath = reader.target.result
      }
    },

    getPixelRatio: function (context) {
      let backingStore = context.backingStorePixelRatio ||
          context.webkitBackingStorePixelRatio ||
          context.mozBackingStorePixelRatio ||
          context.msBackingStorePixelRatio ||
          context.oBackingStorePixelRatio ||
          context.backingStorePixelRatio || 1;
      return (window.devicePixelRatio || 1) / backingStore;
    },

    calculatePic: function (pw, ph) {
      let px = 0;
      let py = 0;
      if(pw < 128 && ph < 128){
        px = 0.5 * 128 - 0.5 * pw;
        py = 0.5 * 128 - 0.5 * ph;
      }else if (ph / pw < 128 / 128) {
        let uu = ph;
        ph = 128
        pw = pw * ph / uu
        px = -(0.5 * pw - 0.5 * 128);
      } else {
        let uu = pw;
        pw = 128;
        ph = ph * pw / uu
        py = -(0.5 * ph - 0.5 * 128);
      }
      return {px, py, pw, ph}
    },
    drawAvatar: function (){
      let that = this
      this.stepNow = 3
      let canvas = document.getElementById('canvas')
      let ctx = canvas.getContext('2d')
      let ratio = this.getPixelRatio(ctx)
      canvas.style.width = canvas.width + 'px';
      canvas.style.height = canvas.height + 'px';
      canvas.width = canvas.width * ratio;
      canvas.height = canvas.height * ratio;
      let bg = new Image()
      bg.src = this.imagePath
      bg.onload = function (){
        let iw = this.width
        let ih = this.height
        let local = that.calculatePic(iw, ih)
        ctx.drawImage(this, local.px * ratio, local.py * ratio, local.pw * ratio, local.ph * ratio)
        let fg = new Image()
        switch (that.selected) {
          case '1':
            fg.src = img1
                break
          case '2':
            fg.src = img2
                break
          case '3':
            fg.src = img3
                break
          case '4':
            fg.src = img4
                break
          case '5':
            fg.src = img5
            break
          case '6':
            fg.src = img6
            break
          default:
            fg.src = img1
                break
        }
        fg.onload = function (){
          let iw = this.width
          let ih = this.height
          let local = that.calculatePic(iw, ih)
          ctx.drawImage(this, local.px * ratio, local.py * ratio, local.pw * ratio, local.ph * ratio)
          that.avatarData = canvas.toDataURL('image/png')
          canvas.toBlob(function (blob){
            that.avatarURL = URL.createObjectURL(blob)
          })
        }
      }
    },
    reload: function (){
      location.reload()
    }
  }
}
</script>
