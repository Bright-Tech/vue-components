<!-- template for the modal component -->
<script>
    module.exports = {
        props: {
            type: {
                type: String,
                default: 'component' // input | component (input with icon) | range
            },
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
        },
        data: function () {
            return {
                elId: 'el_' + Math.random().toString(36).slice(2)
            }
        },
        mounted: function () {
            let vm = this;
            let el = ".datepicker-container-" + vm.elId + ' .input-datepicker';
            $(el).datepicker({
                format: 'yyyy-mm-dd'
            });
            $(el).on('changeDate', function () {
                vm.onInput($(el).val());
            });
        },
        methods: {
            onInput(value){
                this.$emit('input', value);
            },
            onBlur(event){
                this.$emit('input', event.target.value);
            }

        },
        render: function (createElement) {
//            <div class="input-daterange input-group" id="datepicker">
//                <input type="text" class="input-sm form-control" name="start" />
//                <span class="input-group-addon">to</span>
//                <input type="text" class="input-sm form-control" name="end" />
//                </div>
            let childrenNode = [];
            switch (this.type) {
                case 'input':
                    childrenNode = [createElement(
                        'input', {
                            attrs: {
                                type: 'text',
                                'class': 'form-control input-datepicker',
                                value: this.value
                            },
                            on: {
                                input: this.onInput,
                                blur: this.onBlur
                            },
                        }
                    )];
                    break;
                case 'component':
                    childrenNode = [createElement(
                        'div', {
                            // 正常的 HTML 特性
                            attrs: {
                                'class': 'input-group date',
                            },
                        },
                        [
                            createElement(
                                'input', {
                                    attrs: {
                                        type: 'text',
                                        'class': 'form-control input-datepicker',
                                        value: this.value
                                    },
                                    on: {
                                        input: this.onInput,
                                        blur: this.onBlur
                                    },
                                }
                            ),
                            createElement(
                                'span', {
                                    attrs: {
                                        'class': 'input-group-addon',
                                    },
                                },
                                [
                                    createElement(
                                        'i', {
                                            attrs: {
                                                'class': 'fa fa-calendar',
                                            },
                                        })
                                ]
                            )
                        ]
                    )];
                    break;

            }
            return createElement(
                'div', {
                    // 正常的 HTML 特性
                    attrs: {
                        type: 'text',
                        'class': 'datepicker-container-' + this.elId,
                        'data-vv-name': this.elName,
                        'data-vv-as': this.label,
                    },
                }, childrenNode
            );

        },
    }
    ;
</script>