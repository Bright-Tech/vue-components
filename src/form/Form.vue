<template>
    <form class="">
        <slot></slot>
    </form>
</template>
<script>
    module.exports = {
        props: {
            formClass: {
                type: String,
                default: ''
            },
        },
    };
</script>