<template>
    <header>
        <a href="javascript:;" id="tab-bar-back" @click="onBackToIndexClick" v-if="isShowBackBtnFlag"></a>
        <nav>
            <ul id="tab-bar-ul">
                <a v-for="(index, item) in tabList" v-link="item.path" class="tab-bar-li">
                    {{ item.name }}
                </a>
            </ul>
        </nav>
    </header>
    <router-view></router-view>
</template>


<script>
export default {
    props: ['tabList','isShowBackBtnFlag'],
    data () {
        return {
        }
    },
    ready(){
        if( window.location.hash = '#/tabBarWithRouter' ){
            location.hash = location.hash + '/'
        }
    },
    methods:{
        onBackToIndexClick(){
            var href = window.location.href;
            var index = href.lastIndexOf("#");
            var actRuter = href.substring(index);
            href = href.replace(actRuter , '');
            window.location.href = href;
        }
    }
}
</script>
