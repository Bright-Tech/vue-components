<template>
    <div class="form-group" :class="{ 'has-error': errorMessage !== '' && errorMessage !== null }">
        <slot name="label"></slot>
        <div class="">
            <slot  name="input"></slot>
            <span class="help-block m-b-none"
                  v-if="errorMessage!==''">{{errorMessage}}</span>
        </div>
    </div>
</template>
<script>
    module.exports = {
        props: {
            errorMessage: {
                type: String,
                default: ''
            }
        }
    };
</script>