<template>
    <div class="common_nav_style">
        <div class="nav_mark"></div>
        <yd-navbar :title="title" fixed>
            <router-link :to="{path:'/appartmentStyle',query:{index:tabIndex}}" slot="left">
                <span class="back"></span>
            </router-link>
        </yd-navbar>
        <section class="resolve-box" v-show="showContent" v-html="content"></section>
    </div>
</template>
<style>
.webview{width: 100%;height: 100%;position: absolute;top:0;left:0;}
#photo{width: 100%;height: 100%;margin-top: 1.5rem;}
.common_nav_style .m-navbar{z-index: 200}
</style>

<script>
	import { mapGetters } from 'vuex'
	import { mapState } from 'vuex'
	export default {
		data() {
			return {
				panoramicSrc:'',
				tabIndex:'',
                detail:'',
                pdf:'',
                video:'',
                content:'',
                showContent:false,
                title:this.$route.query.title,
			};
		},
		created:function () { 
	        this.tabIndex = this.$route.query.index;
	        this.detail = this.$route.query.detail;
	        this.pdf = this.$route.query.pdf;
	        this.video = this.$route.query.video;
            this.$store.dispatch('showLoading')
	        this.panoramicSrc = this.detail;
            var ssrBase = ''
            let _this = this;
            if(this.panoramicSrc){
                $.get(ssrBase+this.panoramicSrc,function (res) {
                    _this.content = res;
                    _this.$store.dispatch('hideLoading')
                    _this.showContent = true
                })
            } else {
                this.$store.dispatch('hideLoading')
            }

            $(function(){
                $(".navbar-center").css('marginLeft',0);
            });
        },
        mounted:function () {
            //一级页面falg
            isHomePage(0)
        }
	};
</script>
