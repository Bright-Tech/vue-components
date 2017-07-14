<template>
    <div class="vue-table-container">
        <div class="row form-horizontal">
            <div class="col-sm-12">
                <div class="input-group">
                    <input placeholder="请输入关键词" class="input-sm form-control" name="keyword" type="text"
                           v-model="queryString">
                    <span class="input-group-btn">
                        <button type="button" class="btn btn-sm btn-primary" @click="search">检索</button>
                    </span>
                </div>
            </div>
        </div>
        <el-table :data="dataSource.items" style="width: 100%">
            <slot name="columns"></slot>
        </el-table>
    </div>
</template>

<script>
    export default {
        props: {
            dataSource: {
                type: Object,
                default: []
            },
        },
        data: function () {
            return {
                queryString: ''
            }
        },
        methods: {
            search: function () {
                let self = this;
                self.dataSource.fetch(self.queryString);
            }
        }
    }
</script>
