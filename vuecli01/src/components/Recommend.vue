<template>
    <h3 class="recommend"><slot></slot></h3>
</template>

<script>
    export default {
        
    }
</script>

<style lang="less" scoped>
.recommend {
    border-left: 3px solid #d43c33;
    line-height: 1;
    padding-left: 10px;
    margin: 20px 0;
}
</style>