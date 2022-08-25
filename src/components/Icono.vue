<template>
    <span :class="nameClass" v-html="icono">
    </span>
</template>

<script lang="ts">

export default {
    name: "Icono",

    props: {
        nameClass: '',
        icono: '',
    }
}

</script>