<!-- template for the modal component -->
<template>
    <div class="form-group" :class="{ 'has-error': errorMessage!=='' && errorMessage !== null}">
        <label :for="elName" class="control-label">{{label}}</label>
        <div class="">
            <textarea :id="elementId" :name="elName" type="text" class="form-control"
                      v-bind:value="value" @input="updateValue($event.target.value)"></textarea>
            <span class="help-block m-b-none"
                  v-if="errorMessage!==''">{{errorMessage}}</span>
        </div>
    </div>
</template>

<script>
    require('codemirror/lib/codemirror.css');
    require('codemirror/theme/monokai.css');
    require('summernote/dist/summernote.css');
    require('codemirror/lib/codemirror.js');
    require('codemirror/mode/xml/xml.js');
    require('summernote');
    require('summernote/lang/summernote-zh-CN');


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
        mounted: function () {

            let vm = this;
            let el = "#" + vm.elementId;
            $(el).summernote({
                height: 300,
                codemirror: {
                    mode: 'text/html',
                    htmlMode: true,
                    lineNumbers: true,
                    theme: 'monokai'
                },
                callbacks: {
                    onInit: function() {
//                        $placeholder.show();
                    },
                    onFocus: function() {
//                        $placeholder.hide();
                    },
                    onBlur: function() {
//                        var $self = $(this);
//                        setTimeout(function() {
//                            if ($self.summernote('isEmpty') && !$self.summernote('codeview.isActivated')) {
//                                $placeholder.show();
//                            }
//                        }, 300);
                    }
                }
            });
        },
        methods: {
            updateValue: function (value) {
                this.$emit('input', value)
            }
        },
        computed: {
            elementId(){
                return 'el_' + Math.random().toString(36).slice(2);
            }
        }
    };
</script>

<style lang="scss">
    /*@import "node_modules/summernote/src/less/summernote.scss";*/
</style>