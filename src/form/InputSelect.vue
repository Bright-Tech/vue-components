<!-- template for the modal component -->
<template>
    <select :name="elName" :id="elementId" class="chosen-select" :data-placeholder="placeholder"
            :data-vv-name="elName"
            :data-vv-as="label"
            :value="innerValue"
            @change="updateValue($event.target.value)">
        <option></option>
        <option v-for="(item, key) in currentOptions" :value="getOptionValue(key, item)">
            {{getOptionLabel(key, item)}}
        </option>
    </select>
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
            placeholder: {
                type: String,
                default: '请选择'
            },
        },
        mounted: function () {
            let vm = this;
            let el = "#" + vm.elementId;
            $(el).chosen({width: "100%"});
            $(el).on('change', function () {
                vm.$emit('input', $(el).val());
            });
        },
        methods: {
            updateValue: function (value) {
                this.$emit('input', value);
            },
            getOptionLabel(key, item){
                if (typeof item[this.optionsLabel] != "undefined") {
                    return item[this.optionsLabel];
                } else {
                    return item;
                }
            },
            getOptionValue(key, item){
//                console.log(item[this.optionsValue]);
                if (typeof item[this.optionsValue] != "undefined") {
                    return item[this.optionsValue];
                } else {
                    return key;
                }
            }
//            checked: function (pick) {
//                if (pick == this.value) {
//                    return 'selected';
//                } else {
//                    return '';
//                }
//            }
        },
        computed: {
            innerValue: function () {
                let value = this.value;
                let vm = this;
                this.$nextTick(function () {
                    $("#" + vm.elementId).trigger("chosen:updated");
                });
                return value;
            },
            currentOptions: function () {
                let options = this.options;
                let self = this;
                this.$nextTick(function () {
                    $("#" + self.elementId).trigger("chosen:updated");
                });
                return options;
            },
            elementId(){
                if (this.elId === '') {
                    return 'el_' + Math.random().toString(36).slice(2);
                } else {
                    return this.elId;
                }
            },

        }
    };
</script>
