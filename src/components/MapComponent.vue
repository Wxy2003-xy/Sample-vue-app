<template>
    <div>
        <p>Count: {{ count }}</p>
        <button @click="g">Increment Count</button>
    </div>
</template>

<script lang = 'ts'>
    import Vue from 'vue';
    export default {
        name: "MapComponent",
        components: {

        },
        data() {
            return {
                count: 0,
            };
        },
        mounted() {
            console.log("Component has been mounted!");
            const savedCount = localStorage.getItem("count");
            if (savedCount) {
                this.count= JSON.parse(savedCount);
            }
        },
        computed: {

        },
        methods: {
            f: (input: number) => {
                return input++;
            },
            g() {
                this.count++;
                console.log("inc")
                this.saveCount();
            },
            saveCount() {
                localStorage.setItem("count", JSON.stringify(this.count));
            },
        }
    }
</script>

<style>

</style>