<template>
  <div id="wrapper">
    <main>
        <div id="text2voice">
          <div class="text" align="center">在此输入文本</div>
          <el-input
            type="textarea"
            :autosize="{ minRows: 2, maxRows: 4}"
            placeholder="请输入内容"
            v-model="textarea2">
          </el-input>
          <div align="center" id="button">
            <el-button type="primary" @click="submit()">转换</el-button>
          </div>
        </div>
        <br/>
        <div id="download" align="center">
          <p>音频预览</p>
          <iframe ref="audio" width="600px">
          </iframe>
        </div>
    </main>
  </div>
</template>

<script>
import SystemInformation from './LandingPage/SystemInformation'
export default {
  name: 'landing-page',
  components: { SystemInformation },
  data () {
    return {
      textarea2: '',
      filepath: ''
    }
  },
  methods: {
    submit () {
      this.$http
        .get(
          'https://openapi.baidu.com/oauth/2.0/token?grant_type=client_credentials&client_id=1IQMBmWdz2OyyrQzwBx2BPQe&client_secret=kbeqT6PZMyHNrofLXGyHarMtpoWKVgrb',
          {
            headers: {
              'Content-Type': 'application/json'
            }
          }
        )
        .then(res => {
          const token = res.data.access_token
          this.$http
            .get('https://tsn.baidu.com/text2audio', {
              params: {
                tok: token,
                tex: this.textarea2,
                cuid: '38:f9:d3:df:1a:55',
                ctp: 1,
                lan: 'zh'
              }
            })
            .then(res => {
              var header = JSON.stringify(res.headers)
              if (header.indexOf('audio') !== -1) {
                this.$refs.audio.src =
                  'https://tsn.baidu.com/text2audio?cuid=38:f9:d3:df:1a:55&ctp=1&lan=zh&tok=' +
                  token +
                  '&tex=' +
                  this.textarea2
                console.log(this.$refs.audio.src)
              }
            })
        })
    }
  }
}
</script>

<style>
@import url('https://fonts.googleapis.com/css?family=Source+Sans+Pro');

* {
  box-sizing: border-box;
  margin: 0;
  padding: 0;
}

body {
  font-family: 'Source Sans Pro', sans-serif;
}

#wrapper {
  /* background: radial-gradient(
    ellipse at top left,
    rgba(255, 255, 255, 1) 40%,
    rgba(229, 229, 229, 0.9) 100%
  ); */
  height: 100vh;
  padding: 60px 80px;
  width: 100vw;
}

#logo {
  height: auto;
  margin-bottom: 20px;
  width: 420px;
}

/* main > div {
  flex-basis: 50%;
} */
#text2voice {
  /* float: left; */
  margin: 0 auto;
  width: 600px;
  height: 200px;
}
.text {
  margin: 0 auto;
  height: 30px;
}
#download {
  margin: 0 auto;
  width: 600px;
  height: 400px;
}
#button {
  margin: 50px auto;
}
</style>
