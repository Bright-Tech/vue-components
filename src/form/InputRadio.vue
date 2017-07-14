<template>
    <div>
        <label class="radio-inline" v-for="(item,key) in options">
            <input :name="elName" type="radio"
                   :value="getOptionValue(key,item)" @change="onInput($event.target.value)"
                   v-model="componentValue">
            {{ getOptionLabel(key, item) }}
        </label>
    </div>
</template>


<script>
    module.exports = {
        props: {
            options: {
                type: [Array, Object],
                default: []
            },
            optionsValue: {
                type: String,
                default: 'value'
            },
            optionsLabel: {
                type: String,
                default: 'label'
            },
            value: {
                type: [String, Number],
                default: ''
            },
            elName: {
                type: String,
                default: 'el_' + Math.random().toString(36).slice(2)
            },
        },
        data: function () {
            return {
                componentValue: this.value
            };
        },
        methods: {
            onInput: function (value) {
                this.$emit('input', value)
            },
            getOptionLabel(key, item){
                if (typeof item[this.optionsLabel] == "String") {
                    return item[this.optionsLabel];
                } else {
                    return item;
                }
            },
            getOptionValue(key, item){
                if (typeof item[this.optionsValue] == "String") {
                    return item[this.optionsValue];
                } else {
                    return key;
                }
            }
        },
        watch: {
            value: function (val) {

                this.componentValue = val;
            }
        }

    };
</script>