<template>
    <div class="recommend">
        <div v-for="item in recomment" :key="item.title" class="recommend-item">
            <a :href="item.link">
                <img :src="item.image" alt="">
               <div>{{item.title}}</div>
            </a>
        </div>
    </div>
</template>

<script>
    export default {
        name: "HomeRecommend",
        props:{
            recomment:{
                type:Array,
                default(){
                    return []
                }
            }
        }
    }
</script>

<style scoped>
    .recommend{
        display: flex;
        text-align: center;
        font-size: 12px;
        padding: 15px 0 20px;
        border-bottom: 10px solid #eee;
    }
    .recommend-item{
        flex: 1;
        margin-bottom: 3px;
    }
    .recommend-item img{
        width: 70px;
        height: 70px;
    }
</style>