<template lang='jade'>
    h1#comp-title {{ compTitle }}
    h2.hinting-title {{ hintingTitle }}
    <pre-code :code-string="codeString"></pre-code>
    h2.hinting-title {{ hintingTitle2 }}
    a(href='javascript:;' @click="onCallDemoClick" class='demonstrate-a') {{ btnText }}
    div.comp-area
        normal-dialog
        popup-toast
</template>

<script type="text/javascript">
import normalDialog from 'normalDialog'
import popupToast from 'popupToast'
import preCode from './preCode.vue'

export default {
    data () {
        return {
            hintingTitle2: "表现示例:",
            compTitle : "Component Name",
            hintingTitle: "使用示例:",
            btnText: "Call NormalDialog",
            codeString: `
    import normalDialog from 'normalDialog'; //引入组件

    var dlgMsgObj = {
        titleText: "&温馨提示&",            //不传(则不显示 title)
        bodyText: text,
        confirmText: "确认",               //可不传，默认 “确认”
        callBackFunc: null,               //可不传，默认null
        isShowCloseXFlag: true            //可不传，默认 true
    }
    // var dlgMsgObj = text;  //如不需更改Dlg默认文案等，可只 文本内容字符串（String）
    this.$broadcast('show-normal-dlg', dlgMsgObj);`
        }
    },
    route:{
		data(transition){
            this.compTitle = transition.to.name
		}
	},
    components: {
        normalDialog,
        popupToast,
        preCode,
    },
    methods: {
        onCallDemoClick: function(){
            var dlgMsgObj = {
                titleText: "&温馨提示&",
                bodyText: "Welcome To http://www.jeffjade.com",
                confirmText: "确认",          //可不传，默认 “确认”
                callBackFunc: this.showPopupToast,
                isShowCloseXFlag: true       //可不传，默认 true
            }

            // var dlgMsgObj = text;  //如不需更改Dlg默认文案等，可只 文本内容字符串（String）
            this.$broadcast('show-normal-dlg', dlgMsgObj);
        },
        showPopupToast: function(){
            var toastMsg = {
                bodyText: "execute normal dialog callback",
                timer: 2222,           //可不传；默认2000ms
                callBackFunc: function(){console.log('callback down!')}
            }
            this.$broadcast('show-popup-toast', toastMsg);
        }
    }
}
</script>

<style media="screen">
#normal-dlg-body{
    top: 39% !important;
}
</style>
