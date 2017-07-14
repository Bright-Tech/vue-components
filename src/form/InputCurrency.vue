<!-- template for the modal component -->
<template>
    <div class="form-group" :class="{ 'has-error': errorMessage!=='' && errorMessage !== null}">
        <label :for="elName" class="control-label">{{label}}</label>
        <div class="">
            <input :name="elName" type="text" class="form-control"
                   v-bind:value="value" @input="updateValue($event.target.value)">
            <span class="help-block m-b-none"
                  v-if="errorMessage!==''">{{errorMessage}}</span>
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
            value: {
                type: [String, Number],
                default: ''
            },
            elName: {
                type: String,
                default: 'el_' + Math.random().toString(36).slice(2)
            },
            errorMessage: {
                type: String,
                default: ''
            },
        },
        methods: {
            updateValue: function (value) {
                this.$emit('input', value)
            }
        }
    };
</script>