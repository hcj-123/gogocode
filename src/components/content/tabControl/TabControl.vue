<template>
    <div class="tab-control">
        <div v-for="(item ,index) in titles"
             :key="item"
             class="tab-control-item"
             :class="{active:index === curentIndex}"
             @click="itemClick(index)">
            <span>{{item}}</span>
        </div>
    </div>
</template>

<script>
    export default {
        name: "TabControl",
        props:{
            titles: {
                type: Array,
                default(){
                    return []
                }
            }
        },
        data() {
            return {
                curentIndex: 0
            }
        },
        methods: {
            itemClick(index){
                this.curentIndex = index;
                this.$emit("tabClick",index)
            }
        }

    }
</script>

<style scoped>
    .tab-control{
        display: flex;
        text-align: center;
        font-size: 15px;
        background-color: #fff;
        z-index: 2;
    }
    .tab-control-item{
        flex: 1;
        height: 40px;
        line-height: 40px;
    }
    .tab-control-item span {
        padding: 5px;
    }
    .active{
        color: var(--color-high-text);
    }
    .active span {
        border-bottom: 3px solid var(--color-tint);
    }
</style>