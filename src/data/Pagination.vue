<!-- template for the modal component -->
<template>
    <nav aria-label="Page navigation">
        <div class="row">
            <div class="col-xs-12 col-sm-2">
                <p class="total">总计：<span class="label label-primary">{{total}}</span></p>
            </div>
            <div class="col-xs-12 col-sm-10">
                <ul class="pagination pull-right">
                    <li :class="{disabled: currentPage==1}">
                        <a @click.stop="clickPrev" aria-label="Previous">
                            <span aria-hidden="true">上一页</span>
                        </a>
                    </li>
                    <li v-for="page in pages" :class="{active: currentPage==page}">
                        <span v-if="page == -1">...</span>
                        <a v-else @click.stop="goPage(page)">{{page}}</a>
                    </li>

                    <li :class="{disabled: currentPage==pageCount}">
                        <a @click.stop="clickNext" aria-label="Next">
                            <span aria-hidden="true">下一页</span>
                        </a>
                    </li>
                </ul>
            </div>
        </div>
    </nav>
</template>

<script>
    module.exports = {
        props: {
            'pageCount': {type: Number, required: true},
            'total': {
                type: Number
            },
            'pageRange': {
                type: Number,
            },
            'marginPages': {type: Number, default: 1},
            'currentPage': {type: Number, default: 1}
        },
//        created: function () {
//            this.refresh();
//        },
        data: function () {
            return {
                prevPage: 0,
                nextPage: 1,
                firstPage: 1,
                lastPage: this.pageCount
            };
        },
        methods: {
            clickPrev: function () {
                this.$emit('goPage', {page: this.currentPage - 1});
            },
            clickNext: function () {
                if(this.pageCount !==this.currentPage)
                    this.$emit('goPage', {page: this.currentPage + 1});
            },
            goPage: function (page) {
                this.$emit('goPage', {page: page});
            },
            refresh: function () {


            }
        },
        computed: {
            pages: function(){
                let marginPages = 2;
                let showMarginPageCount = 10;
                let page = [];
                if (this.pageCount > 10) {
                    if (this.currentPage <= marginPages + 1) {
                        page = [1, 2, 3, 4, 5, -1];
                    } else if (this.currentPage >= this.pageCount - ( marginPages + 1)) {
                        page = [-1, this.pageCount - 4, this.pageCount - 3, this.pageCount - 2, this.pageCount - 1, this.pageCount];
                    } else {
                        for (let i = this.currentPage - marginPages; i <= this.currentPage + marginPages; i++) {
                            page.push(i);
                        }
                        page.unshift(-1);
                        page.push(-1);
                    }
                } else {
                    for (let i = 1; i <= this.pageCount; i++) {
                        page.push(i);
                    }
                }

                return page;
            }
        }
    }
</script>

<style>
    .total{
        margin:25px 0;
    }
</style>