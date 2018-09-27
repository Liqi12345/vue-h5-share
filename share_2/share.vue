<template>
    <div>
        <v-header flag1="true">
            <span slot="center">分享</span>
        </v-header>
        <main class="test test-1 " :class="togclass">

            <img :src="togclass == 'share_z' ? require('../../../assets/images/blueTitle.png') : require('../../../assets/images/greenTitle.png') "
                 alt="" :class="togclass == 'share_z' ? 'titleImg_z' : 'titleImg_z2' ">

            <img src="../../../assets/images/consTitle.png" alt="" class="consTitle_z">
            <img src="../../../assets/images/cons.png" alt="" class="cons_z">

            <div class="q_app">
                <div class="bdsharebuttonbox">

                    <!-- appShare -->
                    <div :class="{app_share:true,aaa:flag2 == true}" to="#" name="share" @click="active">
                        <img src="../../../assets/images/app_share.png" alt="" >
                    </div>

                    <div id="share_box" >
                        <h1 class="share_title">分享到</h1>
                        <div class="social-share" data-initialized="true" style="text-align: center;" data-url="http://www.bodesports.com/shareerweima.html"  data-title="2018啵嘚一夏，千人足球赛激战正酣。" data-image="http://www.bodesports.com/images/logo.png">
                            <a href="http://service.weibo.com/share/share.php" class="social-share-icon icon-weibo">微博</a>
                            <a href="#" class="social-share-icon icon-wechat">微信</a>
                            <a href="https://connect.qq.com/widget/shareqq/index.html" class="social-share-icon icon-qq">qq</a>
                            <a href="https://sns.qzone.qq.com/cgi-bin/qzshare/cgi_qzshare_onekey" class="social-share-icon icon-qzone">空间</a>
                        </div>
                    </div>
                </div>
            </div>
        </main>
        <v-footer></v-footer>
    </div>
</template>
<style>
    .social-share-icon{
        font-size: 0.3rem;
    }

</style>

<script>
    import VHeader from '../../public/vheader/v-header'
    import VFooter from '../../public/vfooter/v-footer'
    export default {
        name: "share",
        data() {
            return {
                flag2:false
            }
        },
        mounted(){
            this.isapp()

        },
        created(){
               this.init()
        },
        computed: {
            togclass() {
                const random = Math.round(Math.random());
                if (random == 1) {
                    return 'share_z2';
                } else {
                    return 'share_z'
                }

            }
        },
        methods:{
            //判断是否在云打包的应用中
            isapp() {
                var YundabaoUA = navigator.userAgent;//获取当前的useragent
                if (YundabaoUA.indexOf('CK 2.0') > -1){//判断当前的ua中是否包含"CK 2.0"，如果包含"CK 2.0"标识当前在云打包的应用中
                    // alert("您当前是在云打包的应用中");//根据需求可自行修改。
                    this.flag2 = true;
                    alert('app')
                }else{
                    this.flag2 = false;
                    // alert('web')
                }
            },
            active(){
                console.log(document.domain)
                var YDB = new YDBOBJ();
                YDB.Share(
                    "2018啵嘚一夏，千人足球赛激战正酣。",
                    "2018啵嘚一夏，千人足球赛激战正酣。平台竞猜50万大奖送不停，欧洲五大豪门俱乐部深度双人游等你来拿！激情俄罗斯，共享激情时刻，快来啵嘚一夏吧！",
                    'http://'+document.domain+'/images/logo.png',
                    'http://'+document.domain+'/shareerweima.html?'
                );
            },
               init: function () {
                   let url = 'https://cdnjs.cloudflare.com/ajax/libs/social-share.js/1.0.16/js/social-share.min.js'
                   let script = document.createElement('script')
                   script.setAttribute('src', url)
                   document.getElementsByTagName('head')[0].appendChild(script)
               }
        },
        components: {
            VHeader,
            VFooter
        }
    }
</script>

<style scoped src="./share.css"></style>