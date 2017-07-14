<!-- template for the modal component -->
<template>
    <input :id="elementId" :name="elName" type="text" class="form-control"
           :data-vv-name="elName"
           :data-vv-as="label"
           v-bind:value="value"
           @input="$emit('input', $event.target.value)"
           @blur="$emit('input', $event.target.value)">
</template>

<script>
    module.exports = {
        props: {
            label:{
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
            elId: {
                type: String,
                default: ''
            },
        },
        computed: {
            elementId(){
                if (this.elId === '') {
                    return 'el_' + Math.random().toString(36).slice(2);
                } else {
                    return this.elId;
                }
            }
        }
    };
</script>