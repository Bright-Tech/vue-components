<template>
    <div class="form-group" :class="{ 'has-error': hasError }">
        <label class="control-label" v-if="label !== ''">{{label}}</label>
        <div class="">
            <slot></slot>
            <span class="help-block m-b-none"
                  v-if="hasError">{{ errorMessage }}</span>
        </div>
    </div>
</template>
<script>
    module.exports = {
        props: {
            label: {
                type: String,
                default: ''
            },
            errorMessage: {
                type: String,
                default: ''
            }
        },
        computed: {
            hasError(){
                return this.errorMessage !== '' && this.errorMessage !== null;
            }
        }
    };
</script>