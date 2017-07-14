<!-- template for the modal component -->
<template>
    <modal @cancel="onCancel" @ok="onConfirm" :modal-title="title">
        <div class="modal-header">
            <i class="fa fa-info modal-icon"></i>
            <h4 class="font-bold">{{text}}</h4>
        </div>
    </modal>
</template>

<script>
    module.exports = {
        props: {
            title: {
                type: String,
                default: ''
            },
            text: {
                type: String,
                default: ''
            }
        },
        methods: {
            onCancel: function () {
                this.$emit('cancel');
            },
            onConfirm: function () {
                this.$emit('ok');
            }
        }
    }
</script>

<style scoped>
    .modal-header {
        border-bottom: none;
    }
</style>