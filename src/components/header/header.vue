<template> 
    <div class = 'app'>
        <div class='content'>
            <div class='avatar'>
                <img width='64' height='64' :src="seller.avatar" >
            </div>
        </div>
        <div class='bulletin'></div>
    </div>
    
</template>

<script>
export default {
    props(){
        seller:{
           type:Object
        }
    }
}
</script>

<style lang='stylus' rel='stylesheet/stylus'>

</style>
