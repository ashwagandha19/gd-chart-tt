<template>
    <div class="custom-tooltip" v-bind:style="{ backgroundColor: color }">
         <p><span>{{ data.athlete }}</span></p>
         <p><span>Country: </span>{{ data.country }}</p>
     </div>
</template>

<script>
    export default {
        data () {
        return {
            color: null,
            athlete: null,
            country: null,
        };
    },
    beforeMount() {
        this.data = this.params.api.getDisplayedRowAtIndex(this.params.rowIndex).data;
        this.color = this.params.color || 'white';
    }
    }
</script>

<style scoped>

</style>