<!-- template for the modal component -->
<template>
    <transition name="modal" enter-active-class="animated fadeIn" leave-active-class="animated fadeOut">

        <div class="modal inmodal">
            <div class="modal-dialog">
                <div class="modal-content">
                    <div class="modal-header">
                        <slot name="header">
                            <i class="fa fa-info modal-icon"></i>
                            <h4 class="modal-title">{{ title }}</h4>
                        </slot>
                    </div>
                    <div class="modal-footer">
                        <slot name="footer">
                            <button type="button" class="btn btn-default" @click="$emit('cancel')">取消</button>
                            <button type="button" class="btn btn-primary" @click="$emit('ok')">确定</button>
                        </slot>
                    </div>

                </div>

            </div>

        </div>
    </transition>
</template>

<script>
    module.exports = {
        props: {
            title: {
                type: String,
                default: ''
            },
//            text: {
//                type: String,
//                default: ''
//            }
        },
        mounted: function () {
            $('body').addClass("modal-open");
            $('body').css("padding-right",'15px');
            $('body').append('<div class="modal-backdrop fade in"></div>');
        },
        destroyed: function () {
            $('body').removeClass("modal-open");
            $('body').css("padding-right",'');
            $('.modal-backdrop').remove();
        },
//        methods: {
//            onCancel: function () {
//                this.$emit('cancel');
//            },
//            onConfirm: function () {
//                this.$emit('ok');
//            }
//        }
    }
</script>

<style scoped>
    .modal-header {
        border-bottom: none;
    }
    .modal{
        display: block;
        /*padding-right: 15px;*/
    }
</style>