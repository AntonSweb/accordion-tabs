<template id="accordion">
    <div class="ing__tab-wrap">
        <div class="ing__tab-top" v-if="tabShowDesc">
            <span v-for="item in items" @click="openItem(item)" class="ing__tab-head" :class="{active: item.open}">@{{item.title}}</span>
        </div>
        <div class="ing__tab" v-for="item in items" @click="openItem(item)">
            <span v-if="tabShowMob" class="ing__tab-head" :class="{active: item.open}">@{{item.title}}</span>
            <transition @enter="enter" @leave="leave">
                <div v-show="item.open" v-html="item.content" class="ing__tab-body"></div>
            </transition>
        </div>
    </div>
</template>

<script>
    import Velocity from 'velocity-animate';

    export default {
        name: "tab.vue",
        data: function () {
            return{
                tabShowDesc: false,
                tabShowMob: false,
                windowWidth: 0,
                items: [{
                    id: 1,
                    title: '{{ $L->__("tab_title1") }}',
                    content: '{!! $long_desc[0] or "" !!}',
                    open: true
                }, {
                    id:2,
                    title: '{{ $L->__("tab_title2") }}',
                    content: '{!! $long_desc[1] or "" !!}',
                    open: false
                }, {
                    id:3,
                    title: '{{ $L->__("tab_title3") }}',
                    content: '{!! $long_desc[2] or "" !!}',
                    open: false
                }, {
                    id:4,
                    title: '{{ $L->__("tab_title4") }}',
                    content: '{!! $long_desc[3] or "" !!}',
                    open: false
                }, {
                    id:5,
                    title: '{{ $L->__("tab_title5") }}',
                    content: '{!! $long_desc[4] or "" !!}',
                    open: false
                }]
            }
        },
        mounted () {
            this.$nextTick(function() {
                window.addEventListener('resize', this.getWindowWidth);
                this.getWindowWidth();
            });
        },
        beforeDestroy() {
            window.removeEventListener('resize', this.getWindowWidth);
        },
        methods: {
            getWindowWidth: function (){
                var that = this;
                this.windowWidth = document.documentElement.clientWidth;
                if (this.windowWidth > 767 - 15){
                    that.tabShowDesc = true;
                    that.tabShowMob = false;
                } else {
                    that.tabShowDesc = false;
                    that.tabShowMob = true;
                }
            },
            openItem: function (item){
                var that = this;
                var index= 0;
                if(item.open !== true){
                    this.items.forEach(function(item){
                        index = item.id - 1;
                        that.items[index].open = false;
                    });
                }
                item.open = !  item.open;
            },
            enter: function(el, done){
                Velocity(el, 'slideDown', {duration: 300,
                        easing: "easeInBack"},
                    {complete: done})
            },
            leave: function(el, done){
                Velocity(el, 'slideUp', {duration: 300,
                        easing: "easeInBack"},
                    {complete: done})
            }
        }
    }
</script>

<style scoped>

</style>