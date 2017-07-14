<!-- template for the modal component -->
<template>
    <transition name="modal" enter-active-class="animated fadeIn" leave-active-class="animated fadeOut">

        <div class="modal" style="display: block;">
            <div class="modal-dialog inmodal" v-bind:class="modalSizeClass">
                <div class="modal-content">
                    <div class="modal-header">
                        <slot name="header">
                            <button type="button" class="close"  @click="$emit('cancel')"><span
                                    aria-hidden="true">&times;</span></button>
                            <h4 class="modal-title">{{ modalTitle }}</h4>
                        </slot>
                    </div>
                    <div class="modal-body">
                        <slot></slot>
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
            modelSize: {
                type: String,
                default: ''
            },
            modalTitle: {
                type: String,
                default: 'Modal title'
            }
        },
        data: function () {
            return {
//                showModal: false,
            };
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
        computed: {
            modalSizeClass: function () {
                return {
                    'modal-lg': this.modelSize == 'large',
                    'modal-sm': this.modelSize == 'small'
                }
            }
        }
    }
</script>