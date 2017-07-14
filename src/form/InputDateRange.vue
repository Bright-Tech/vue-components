<!-- template for the modal component -->
<template>
    <div :class="'datepicker-container-' + this.elId" :data-vv-name="elName" :data-vv-as="label">
        <div class="input-group input-daterange">
            <input :id="elId+'_begin'" type="text" class="form-control input-sm input-datepicker" :value="beginValue"
                   @input="onBeginInput" @blur="onBlur"/>
            <span class="input-group-addon"> - </span>
            <input :id="elId+'_end'" type="text" class="form-control input-sm input-datepicker" :value="endValue"
                   @input="onEndInput" @blur="onBlur"/>
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
            beginValue: {
                type: [String, Number],
                default: ''
            },
            endValue: {
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
                elId: 'el_' + Math.random().toString(36).slice(2),
                innerBeginValue: '',
                innerEndValue: '',

            }
        },
        mounted: function () {
            let vm = this;
            let el = ".datepicker-container-" + vm.elId + ' .input-datepicker';
            $(el).datepicker({
                format: 'yyyy-mm-dd'
            });
            $(el).on('changeDate', function () {
                if ($(this).attr('id') === vm.elId + '_begin') {
                    vm.onBeginInput($(this).val());
                } else {
                    vm.onEndInput($(this).val());
                }


            });
        },
        methods: {
            onBeginInput(value){
                let vm = this;
                this.innerBeginValue = value;
                this.$emit('input', {
                    beginValue: vm.innerBeginValue,
                    endValue: vm.innerEndValue
                });
            },
            onEndInput(value){
                let vm = this;
                this.innerEndValue = value;
                this.$emit('input', {
                    beginValue: vm.innerBeginValue,
                    endValue: vm.innerEndValue
                });
            },
            onBlur(event){
                this.$emit('input', {
                    beginValue: this.innerBeginValue,
                    endValue: this.innerEndValue
                });
            }

        }
    }
    ;
</script>