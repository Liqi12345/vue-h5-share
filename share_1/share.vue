<template>
    <div id="nativeShare">
        <div class="label">分享到</div>
        <div class="list">
      <img src="../../../assets/images/weixin_friend.png" class="i" @click="show()">微信
        <div id="qrcode" v-show="flag">
            <span>
                长按保存二维码<br>或<br>长按复制链接分享好友<span class="wzShare">http://www.bodesports.com/shareerweima.html</span>
                <img class="closeBtndwz" src="../../../assets/images/closeBtnz.png" alt="" @click="show()">
                <img :src="url" alt="">
            </span>
        </div>
      <span class="list-item" v-for="(button, index) in shareButtons" :key="index">
        <img :src="button.src" class="i" @click="call(button)">
        {{button.text}}
      </span>
        </div>
    </div>
</template>

<script>



    //https://github.com/fa-ge/NativeShare
    const nativeshare = () => import ('nativeshare') //这种引入方式nativeshare是Promise对象
    //https://github.com/backToNature/m-share
    const m_share = () => import ('m-share')
    var NativeShare, mShare
    export default {
        name: 'the-sharebar',
        data() {
            return {
                flag:false,
                url:'',
                shareButtons:[
                    {text: '朋友圈', nativeshare:'wechatTimeline', m_share: 'wxline', src: require('../../../assets/images/weixin.png')},
                    {text: '新浪微博', nativeshare:'weibo', m_share: 'sina', src: require('../../../assets/images/weibo.png')},
                    {text: 'QQ好友', nativeshare:'qqFriend', m_share: 'qq', src: require('../../../assets/images/QQ.png')},
                    {text: 'QQ空间', nativeshare:'qZone', m_share: 'qzone', src: require('../../../assets/images/qqzone.png')},
                    {text: '更多', nativeshare:'', m_share: 'qq', src: require('../../../assets/images/more.png')},
                ]
            }
        },
        computed: {
            config () {
                return {
                    title: '2018啵嘚一夏，千人足球赛激战正酣。',
                    desc:'2018啵嘚一夏，千人足球赛激战正酣。',
                    img:'http://www.bodesports.com/images/logo.png',
                    img_title:'logo',
                    link: 'http://www.bodesports.com/shareerweima.html'
                }
            }
        },
        created(){
         this.qrcode('http://www.bodesports.com/shareerweima.html');
},
        methods: {
            call(command) {
                let shareData = {  //nativeShare的参数模型
                    title: '2018啵嘚一夏，千人足球赛激战正酣。',
                    desc:'2018啵嘚一夏，千人足球赛激战正酣。',
                    // 如果是微信该link的域名必须要在微信后台配置的安全域名之内的。
                    link: 'http://www.bodesports.com/shareerweima.html',
                    icon: 'http://www.bodesports.com/images/logo.png',
                    // 不要过于依赖以下两个回调，很多浏览器是不支持的
                    success: function() {
                        alert('success')
                    },
                    fail: function() {
                        alert('fail')
                    }
                }
                let mShareData = {  //m-share的'http://www.bodesports.com/shareerweima.html'参数模型
                    title: '2018啵嘚一夏，千人足球赛激战正酣。',
                    desc:'2018啵嘚一夏，千人足球赛激战正酣。', // 描述, 默认读取head标签：<meta name="description" content="desc" />
                    link: 'http://www.bodesports.com/shareerweima.html', // 网址，默认使用window.location.href
                    imgUrl:'http://www.bodesports.com/images/logo.png', // 图片, 默认取网页中第一个img标签
                    fnDoShare(type) {
                        console.log('success')
                    }
                }
                let nativeShare = new NativeShare()
                nativeShare.setShareData(shareData)
                try {
                    nativeShare.call(command.nativeshare)
                } catch(e) {
                    //iphone的qq浏览器调用此api
                    //除iphone的qq浏览器外其他浏览器调用的api
                    //在iphone的qq浏览器中比较奇葩，第一次调用nativeShare.call()会报错，第二次之后不报，这里是让每次调用nativeShare.call()之后都报错，然后统一去调m-share.to()方法
                    mShare.to(command.m_share, mShareData)
                }
            },
            show(){
                this.qrcode('http://www.bodesports.com/shareerweima.html')
                this.flag = !this.flag
            },
            //获取二维码
            qrcode(text) {
                const vm = this;
                var QRCode = require('qrcode')
                QRCode.toDataURL(text, function (err, url) {
                    vm.url = url
                })

            }

        },
        mounted() {
            // ES6 标准
            nativeshare().then(res =>  {NativeShare = res.default} )
            // CommonJS 标准
            m_share().then(res => {mShare = res})
            //alert(browser)
        }
    }
</script>

<style scoped>
    #nativeShare {
        font-size: 13px;
    }

    .label {
        font-size: 18px;
    }

    .list-item {
        width: 30%;
        display: inline-block;
        text-align: center;
        margin: 10px 0;
    }

    .i {
        width: 40px;
        height: 40px;
        display: block;
        margin: 0 auto;
        margin-bottom: 5px;
        background-size: cover;
    }
    #qrcode{
        position: absolute;
        top:0;
        left: 0;
        right: 0;
        bottom: 0;
        margin: auto;
        width: 6rem;
        height: 6.5rem;
        background-color: #fff;
        text-align: center;
        padding: 0.5rem;
    }
    #qrcode img{
        width: 4rem;
        height: 4rem;
        margin: 0.5rem auto;
    }
    #qrcode img.closeBtndwz{
        width: 0.8rem;
        height: 0.8rem;
        position: absolute;
        right: 0.15rem;
        top: -0.3rem;
    }
    #qrcode span{
        font-size: 0.26rem;
        text-align: center;
        color: #333;
    }
    .wzShare{
        font-size: 0.26rem;
    }
</style>