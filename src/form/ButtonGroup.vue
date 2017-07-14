<!-- template for the modal component -->
<template>
    <div class="btn-group">
        <button v-for="(item, key) in options" type="button"
                class="btn btn-xs btn-white" :class="{'active':innerValue == getOptionValue(key, item)}"
                @click="onClick"
                :value="getOptionValue(key, item)">{{getOptionLabel(key, item)}}
        </button>
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
            options: {
                type: [Array, Object],
                default: []
            },

            elName: {
                type: String,
                default: 'el_' + Math.random().toString(36).slice(2)
            },
            elId: {
                type: String,
                default: ''
            },
            optionsLabel: {
                type: String,
                default: 'label'
            },
            optionsValue: {
                type: String,
                default: 'value'
            },
        },
        data: function () {
            return {
                innerValue: this.value
            };
        },
        methods: {
            onClick(event){
                this.innerValue = event.target.value;
                this.$emit('input', event.target.value);
            },
            updateValue: function (value) {
                this.$emit('input', value);
            },
            getOptionLabel(key, item){
                if (typeof item[this.optionsLabel] !== "undefined") {
                    return item[this.optionsLabel];
                } else {
                    return item;
                }
            },
            getOptionValue(key, item){
//                console.log(item[this.optionsValue]);
                if (typeof item[this.optionsValue] !== "undefined") {
                    return item[this.optionsValue];
                } else {
                    return key;
                }
            }
        }
    };
</script>