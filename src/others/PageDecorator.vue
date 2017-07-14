<template>
    <transition name="page" enter-active-class="animated fadeInRight" leave-active-class="animated fadeOutRight">
        <div class="wrapper wrapper-content">
            <slot></slot>
        </div>
    </transition>
</template>

<script>
    export default {}
</script>
