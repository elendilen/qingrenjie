<template>
    <div id="app">
        <StyleEditor ref="styleEditor" :code="currentStyle"></StyleEditor>
        <ResumeEditor
            ref="resumeEditor"
            :markdown="currentMarkdown"
            :enableHtml="enableHtml"
            :photos="photoNames"
            :galleryTitle="galleryTitle"
            :gallerySubtitle="gallerySubtitle"
        ></ResumeEditor>
    </div>
</template>
<script>
import StyleEditor from './components/StyleEditor'
import ResumeEditor from './components/ResumeEditor'
import './assets/reset.css'
const meetDate = '2025-08-02'
let isPc = (function() {
    var userAgentInfo = navigator.userAgent;
    var Agents = ["Android", "iPhone",
        "SymbianOS", "Windows Phone",
        "iPad", "iPod"
    ];
    var flag = true;
    for (var v = 0; v < Agents.length; v++) {
        if (userAgentInfo.indexOf(Agents[v]) > 0) {
            flag = false;
            break;
        }
    }
    return flag;
}());
let getDateDiff = function(startDate, endDate) {
    var startTime = new Date(Date.parse(startDate.replace(/-/g, "/"))).getTime();
    var endTime = new Date(Date.parse(endDate.replace(/-/g, "/"))).getTime();
    var dates = Math.abs((startTime - endTime)) / (1000 * 60 * 60 * 24);
    return dates;
}
document.title = 'Hi，Sunny！' + (getDateDiff((new Date()).getFullYear() + '-' + ((new Date()).getMonth() + 1) + '-' + (new Date()).getDate(), meetDate) + 1) + ' 天';
export default {
    name: 'app',
    components: {
        StyleEditor,
        ResumeEditor
    },
    data() {
        return {
            interval: 27,
            currentStyle: '',
            enableHtml: false,
            fullStyle: [
                `/*
* Hi，Sunny！
* 这是一张会自动写字的小小网页。
* 我把想说的话，藏在代码里。
* 先把页面打扮得舒服一点。
* 顺便根据设备做个适配。
* 你现在用的是：${isPc ? '电脑' : '手机'}
*/

/* 首先给所有元素加上过渡效果 */
* {
  -webkit-transition: all .3s;
  transition: all .3s;
}
/* 背景别太单调 */
html {
  color: rgb(222,222,222);
  background: rgb(0,43,54); 
}
/* 文字别挤在一起 */
.styleEditor {
  padding: .5em;
  border: 1px solid;
  margin: .5em;
  overflow: auto;
  ${ isPc ? 'width: 48%;height: 96%;' : 'width: 96%;height: 50%;' }
  font-size: 14px;
  line-height:1.5;
}
/* 这些代码颜色都一样。加点高亮 */
.token.selector{ color: rgb(133,153,0) }
.token.property{ color: rgb(187,137,0) }
.token.punctuation{ color: yellow }
.token.function{ color: rgb(42,161,152) }
.token.comment{ color: rgb(177,177,177) }
/* 加个 3D 效果 */
html{
  -webkit-perspective: 1000px;
          perspective: 1000px;
}
.styleEditor {
  position: fixed; 
  ${ isPc ? 'left: 0;' : 'left:0;right:0;margin:auto;'}
  top: 0; 
  -webkit-transition: none; 
  transition: none;   
  ${ isPc ? '-webkit-transform: rotateY(10deg) translateZ(-100px) ;transform: rotateY(10deg) translateZ(-100px) ;' : '-webkit-transform: rotateX(-10deg) translateZ(-100px) ;transform: rotateX(-10deg) translateZ(-100px) ;' }
  ${ isPc ? '' : '-webkit-transform-origin: 50% 0% 0;transform-origin: 50% 0% 0;' }
}

/* 再来一张信纸 */
.resumeEditor{
  position: fixed; 
  ${ isPc ? 'right: 0;' : 'left:0;right:0;margin:auto;'}
  ${ isPc ? 'top: 0;' : 'bottom:2%;'}
  padding: .5em;  
  ${ isPc ? 'margin: .5em;' : ''}
  ${ isPc ? 'width: 48%;height: 96%;' : 'width: 96%;height: 50%;' }
  border: 1px solid;
  color: #222;
  overflow: auto;
  font-size: 14px;
  line-height:1.5;
  ${ isPc ? '-webkit-transform: rotateY(-10deg) translateZ(-100px) ;transform: rotateY(-10deg) translateZ(-100px) ;' : '-webkit-transform: rotateX(10deg) translateZ(-100px) ;transform: rotateX(10deg) translateZ(-100px) ;' }
    ${ isPc ? '' : '-webkit-transform-origin: 50% 0% 0;transform-origin: 50% 0% 0;' }
  }
/* 开始写信 */


`,
                                `
/* 这是 Markdown 格式
 * 让文字更像一封信
 * 也方便排版
 */
`,
                `
/* 再加点样式 */
.resumeEditor{
  padding: 2em;
  line-height:1.8;
    background: rgba(253, 246, 227, 0.92);
    border-color: rgba(0,0,0,.25);
    border-radius: 12px;
    box-shadow: 0 10px 26px rgba(0,0,0,.18);
}
.resumeEditor h2{
  display: inline-block;
  border-bottom: 1px solid;
  margin: 1em 0 .5em;
  font-size:18px;
}
.resumeEditor ul,.resumeEditor ol{
  list-style: none;
}
.resumeEditor ul> li::before{
  content: '•';
  margin-right: .5em;
}
.resumeEditor ol {
  counter-reset: section;
}
.resumeEditor ol li::before {
  counter-increment: section;            
  content: counters(section, ".") " ";  
  margin-right: .5em;
}
.resumeEditor blockquote {
  margin: 1em;
  padding: .5em;
  background: rgba(221,221,221,.5);
}

/* 相册区域 */
.gallery {
    margin-top: 1.8em;
}
.gallery-title {
    font-size: 18px;
    margin: 0 0 .2em;
}
.gallery-subtitle {
    margin: 0 0 1em;
    color: #555;
}
.gallery-stage {
    position: relative;
    border-radius: 14px;
    overflow: hidden;
    background: radial-gradient(circle at 20% 20%, #222, #0e0e0e 60%, #070707);
    box-shadow: 0 12px 30px rgba(0,0,0,.25);
    height: 360px;
    max-height: 52vh;
    display: flex;
    align-items: center;
    justify-content: center;
    padding: 10px;
}
.gallery-main {
    width: 100%;
    height: 100%;
    object-fit: contain;
    display: block;
    border-radius: 10px;
}
.gallery-indicators {
    position: absolute;
    right: 12px;
    bottom: 10px;
    display: flex;
    gap: 6px;
    background: rgba(0,0,0,.35);
    padding: 6px 8px;
    border-radius: 999px;
}
.gallery-dot {
    width: 6px;
    height: 6px;
    border-radius: 50%;
    background: rgba(255,255,255,.5);
}
.gallery-dot.is-active {
    background: #fff;
}
.gallery-thumbs {
    margin-top: 12px;
    display: grid;
    grid-template-columns: repeat(auto-fill, minmax(72px, 1fr));
    gap: 8px;
}
.gallery-thumb-button {
    padding: 0;
    border: none;
    background: transparent;
    cursor: pointer;
}
.gallery-thumb {
    width: 100%;
    height: 72px;
    object-fit: cover;
    border-radius: 8px;
    transition: transform .2s, box-shadow .2s, filter .2s;
    filter: grayscale(.1) brightness(.95);
}
.gallery-thumb-button.is-active .gallery-thumb {
    filter: none;
    transform: scale(1.02);
    box-shadow: 0 6px 18px rgba(0,0,0,.25);
    outline: 2px solid #f7b500;
    outline-offset: 1px;
}
@media (max-width: 768px) {
    .resumeEditor{
        padding: 1.2em;
    }
    .gallery-stage {
        height: 240px;
        max-height: 45vh;
    }
}

/* 完成,请慢慢看 */

`
            ],
            currentMarkdown: '',
            fullMarkdown: `Sunny & Me
----

今天是 ${new Date().getFullYear()}年${(new Date().getMonth() + 1)}月${new Date().getDate()}日。  
我们认识于 2025年08月02日,xhs中sunny的评论区里。
到今天，已经认识 \`${getDateDiff((new Date()).getFullYear()+'-'+((new Date()).getMonth()+1)+'-'+(new Date()).getDate(), meetDate) + 1}\` 天

we are 异地恋ing 
距离把见面变得更珍贵，也把想念变得更具体。

我们一起看过的电影
----

1. 疯狂动物城2
2. La La Land

我们想一起去的地方，希望我们有一天能全部去一遍，我们还要继续添加心愿清单
----
1. 武汉
2. 杭州
3. 南京
4. 新疆
5. 加州
6. 冰岛

他们说相片是人类已知唯一的穿越时间的工具，当看到一张照片的时候，我们就能实现一次时空穿梭！
----

滑动的照片，每一张我们都比前一张的时候更亲密！

> 异地恋也没关系，我们一直在同一条路上。

`,
            galleryTitle: '我和Sunny 的相册',
            gallerySubtitle: '自动播放 · 点击缩略图可切换',
            photoNames: [
                'Weixin Image_20260214165517_65_10.jpg',
                'Weixin Image_20260214165518_66_10.jpg',
                'Weixin Image_20260214165518_67_10.jpg',
                'Weixin Image_20260214165519_68_10.jpg',
                'Weixin Image_20260214165520_69_10.jpg',
                'Weixin Image_20260214165521_70_10.jpg',
                'Weixin Image_20260214165522_71_10.jpg',
                'Weixin Image_20260214165523_72_10.jpg',
                'Weixin Image_20260214165524_73_10.jpg',
                'Weixin Image_20260214165524_74_10.jpg',
                'Weixin Image_20260214165525_75_10.jpg',
                'Weixin Image_20260214165526_76_10.jpg',
                'Weixin Image_20260214165527_77_10.jpg',
                'Weixin Image_20260214165528_78_10.jpg',
                'Weixin Image_20260214165528_79_10.jpg',
                'Weixin Image_20260214165529_80_10.jpg',
                'Weixin Image_20260214165530_81_10.jpg',
                'Weixin Image_20260214165531_82_10.jpg',
                'Weixin Image_20260214165531_83_10.jpg',
                'Weixin Image_20260214165532_84_10.jpg',
                'Weixin Image_20260214165533_85_10.jpg',
                'Weixin Image_20260214165646_86_10.jpg',
                'Weixin Image_20260214165647_87_10.jpg',
                'Weixin Image_20260214165648_88_10.jpg',
                'Weixin Image_20260214165649_89_10.jpg',
                'Weixin Image_20260214165650_90_10.jpg',
                'Weixin Image_20260214165653_91_10.jpg',
                'Weixin Image_20260214165654_92_10.jpg',
                'Weixin Image_20260214165655_93_10.jpg',
                'Weixin Image_20260214165656_94_10.jpg',
                'Weixin Image_20260214165657_95_10.jpg',
                'Weixin Image_20260214165658_96_10.jpg',
                'Weixin Image_20260214165659_97_10.jpg',
                'Weixin Image_20260214165701_98_10.jpg',
                'Weixin Image_20260214165702_99_10.jpg',
                'Weixin Image_20260214165704_100_10.jpg',
                'Weixin Image_20260214165705_101_10.jpg',
                'Weixin Image_20260214165707_102_10.jpg',
                'Weixin Image_20260214165816_103_10.jpg',
                'Weixin Image_20260214165817_104_10.jpg',
                'Weixin Image_20260214165818_105_10.jpg',
                'Weixin Image_20260214165818_106_10.jpg'
            ]
        }
    },
    created() {
        this.makeResume()
    },

    methods: {
        makeResume: async function() {
            await this.progressivelyShowStyle(0)
            await this.progressivelyShowResume()
            await this.progressivelyShowStyle(1)
            await this.showHtml()
            await this.progressivelyShowStyle(2)
        },
        showHtml: function() {
            return new Promise((resolve, reject) => {
                this.enableHtml = true
                resolve()
            })
        },
        progressivelyShowStyle(n) {
            return new Promise((resolve, reject) => {
                let interval = this.interval
                let showStyle = (async function() {
                    let style = this.fullStyle[n]
                    if (!style) {
                        return
                    }
                    // 计算前 n 个 style 的字符总数
                    let length = this.fullStyle.filter((_, index) => index <= n).map((item) => item.length).reduce((p, c) => p + c, 0)
                    let prefixLength = length - style.length
                    if (this.currentStyle.length < length) {
                        let l = this.currentStyle.length - prefixLength
                        let char = style.substring(l, l + 1) || ' '
                        this.currentStyle += char
                        if (style.substring(l - 1, l) === '\n' && this.$refs.styleEditor) {
                            this.$nextTick(() => {
                                this.$refs.styleEditor.goBottom()
                            })
                        }
                        setTimeout(showStyle, interval)
                    } else {
                        resolve()
                    }
                }).bind(this)
                showStyle()
            })
        },
        progressivelyShowResume() {
            return new Promise((resolve, reject) => {
                let length = this.fullMarkdown.length
                let interval = this.interval
                let showResume = () => {
                    if (this.currentMarkdown.length < length) {
                        this.currentMarkdown = this.fullMarkdown.substring(0, this.currentMarkdown.length + 1)
                        let lastChar = this.currentMarkdown[this.currentMarkdown.length - 1]
                        let prevChar = this.currentMarkdown[this.currentMarkdown.length - 2]
                        if (prevChar === '\n' && this.$refs.resumeEditor) {
                            this.$nextTick(() => this.$refs.resumeEditor.goBottom())
                        }
                        setTimeout(showResume, interval)
                    } else {
                        resolve()
                    }
                }
                showResume()
            })
        }
    }
}
</script>
<style scoped>
#app {
    font-family: 'Avenir', Helvetica, Arial, sans-serif;
    -webkit-font-smoothing: antialiased;
    -moz-osx-font-smoothing: grayscale;
}

html {
    min-height: 100%;
}
.styleEditor {
    -webkit-backface-visibility: hidden;
}
</style>
