<template>
    <button @click="count++">Count is: {{ count }}</button>
</template>
  
<script>
export default {
    name: 'CounterItem',
    data() {
        return {
            count: 0
        }
    }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
button {
    font-weight: bold;
}
</style>
