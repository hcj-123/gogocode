<template>
    <div class="goods">
        <good-item v-for="item in goods" :key="item.iid" :good-item="item"/>
<!--        {{goods}}-->
    </div>
</template>

<script>
    import GoodItem from './GoodItem'
    export default {
        name: "GoodsList",
        components: {
            GoodItem
        },
        props: {
            goods: {
                type:Array,
                default(){
                    return []
                }
            }
        }
    }
</script>

<style scoped>
    .goods {
        /*使所有的都挤在一起*/
        display: flex;
        /*下面这个属性就是依照自身的宽度，挤在一起*/
        flex-wrap: wrap;
        /*这个属性是代表使其均匀宽度，非常good*/
        justify-content: space-around;

        padding: 2px;
    }


</style>