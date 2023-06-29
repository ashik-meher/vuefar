<template>

    <div style="background-color: skyblue; text-align: center;padding: 10px;">

        <h1>Test View For Laravel Laracaset Vue 3 Unity</h1>

        <button @click="decrement" class="decrement">-</button>
         <span class="counter">{{ counter }}</span>
        <button @click="increment" class="increment">+</button>

    </div>



</template>


<script type="module">

//https://www.smashingmagazine.com/2020/10/authentication-in-vue-js/

export default{
    data(){
        return {
            counter: 0
        }
    },

    methods: {
        increment(){
            this.counter++;
        },
        decrement() {
            if(this.counter > 0){

                this.counter--;
            }
           
        }
    }
}
</script>


<style scoped>
.counter{
    padding: 0 10px;
    font-weight: bold;
}
.increment{
    color: #FFFFFF;
    background-color: green;
    text-transform: uppercase;
    padding: 5px 15px;
    border: 1px solid green;
    border-radius: 2px;
}
.decrement{
    color: #FFFFFF;
    background-color: brown;
    text-transform: uppercase;
    padding: 5px 15px;
    border: 1px solid green;
    border-radius: 2px;
}
</style>