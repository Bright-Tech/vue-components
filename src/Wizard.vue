<template>
    <div class="vue-wizard">
        <div class="wizard-header">
            <slot name="title"></slot>
            <h2 class="tab-content-title">{{ currentTab.title }}</h2>
        </div>
        <div class="wizard-navigation">
            <ul class="wizard-nav">
                <li v-for="(tab, index) in tabs" :class="{active:tab.active}">
                        <div class="icon-circle"
                             :class="{active:tab.active}"
                             :style="[isChecked(index)? stepCheckedStyle : {}, tab.validationError ? errorStyle : {}]">
                            <div v-if="tab.active" class="icon-container"
                                 :style="[tab.active ? iconActiveStyle: {}, tab.validationError ? errorStyle : {}]">
                                <i v-if="tab.icon" :class="tab.icon" class="icon"></i>
                                <i v-else class="icon">{{index + 1}}</i>
                            </div>
                            <div v-if="index < activeTabIndex " class="icon-changed" :style="iconActiveStyle">
                                <i class="icon fa fa-check"></i>
                            </div>
                            <i v-if="index > activeTabIndex && tab.icon" :class="tab.icon" class="icon"></i>
                            <i v-if="index > activeTabIndex && !tab.icon" class="icon">{{index + 1}}</i>
                        </div>
                </li>
            </ul>
            <slot name="subTitle"></slot>
            <hr class="hr-line-solid" style="color: #eeeeee;">
            <div class="tab-content">
                <slot>
                </slot>
            </div>
            <hr class="hr-line-solid" style="color: #eeeeee;">
        </div>

        <div class="card-footer">
            <template>
        <span @click="prevTab" v-if="displayPrevButton" class="pull-left">
          <slot name="prev">
            <button type="button" class="btn btn-default btn-wd" :style="prevButtonStyle" :disabled="loading">
              {{backButtonText}}
            </button>
          </slot>
        </span>
            </template>

            <template>
         <span @click="finish" class="pull-right" v-if="isLastStep">
           <slot name="finish">
             <button type="button" class="btn btn-fill btn-wd btn-next" :style="fillButtonStyle" id="finishButton">
              {{finishButtonText}}
            </button>
          </slot>
        </span>
            </template>

            <template>
        <span @click="nextTab" class="pull-right" v-if="!isLastStep">
         <slot name="next">
           <button type="button" class="btn btn-fill btn-wd btn-next" :style="fillButtonStyle" :disabled="loading">
            {{nextButtonText}}
           </button>
         </slot>
       </span>
            </template>

        </div>
    </div>
</template>
<script>
    export default{
        props: {
            nextButtonText: {
                type: String,
                default: 'Next'
            },
            backButtonText: {
                type: String,
                default: 'Back'
            },
            finishButtonText: {
                type: String,
                default: 'Finish'
            },
            /***
             * Applies to text, border and circle
             */
            color: {
                type: String,
                default: '#e74c3c'
            },
            errorColor: {
                type: String,
                default: '#8b0000'
            },
            shape: {
                type: String,
                default: 'circle'
            },
            /**
             * Name of the transition when transition between steps
             * */
            transition: {
                type: String,
                default: ''
            },
            /***
             *
             * Index of the initial tab to display
             */
            startIndex: {
                type: Number,
                default: 0,
                validator: (value) => {
                    return value >= 0
                }
            },
            goToTab: {
                status: {
                    type: Boolean,
                    default: false
                },
                tabIndex: {
                    type: Number,
                    default: 0
                }
            }
        },
        data () {
            return {
                activeTabIndex: 0,
                isLastStep: false,
                currentPercentage: 0,
                maxStep: 0,
                loading: false,
                tabs: [],
                currentTab: {
                    title: ''
                }
            }
        },
        computed: {
            tabCount () {
                return this.tabs.length
            },
            displayPrevButton () {
                return this.activeTabIndex !== 0
            },
            stepPercentage () {
                return 1 / (this.tabCount * 2) * 100
            },
            iconActiveStyle () {
                return {
                    backgroundColor: this.color
                }
            },
            stepCheckedStyle () {
                return {
                    borderColor: this.color
                }
            },
            errorStyle () {
                return {
                    borderColor: this.errorColor,
                    backgroundColor: this.errorColor
                }
            },
            stepTitleStyle () {
                var isError = this.tabs[this.activeTabIndex].validationError
                return {
                    color: isError ? this.errorColor : this.color
                }
            },
            fillButtonStyle () {
                return {
                    backgroundColor: this.color,
                    borderColor: this.color,
                    color: 'white'
                }
            },
            prevButtonStyle () {
                return {
                    backgroundColor: 'white',
                    borderColor: this.color,
                    color: this.color
                }
            }
        },
        methods: {
            isChecked (index) {
                return index <= this.maxStep
            },
            navigateToTab (index) {
                if (index <= this.maxStep) {
                    let cb = () => {
                        this.changeTab(this.activeTabIndex, index)
                    }
                    this.beforeTabChange(this.activeTabIndex, cb)
                }
            },
            setLoading (value) {
                this.loading = value
                this.$emit('on-loading', value)
            },
            setValidationError (error) {
                this.tabs[this.activeTabIndex].validationError = error
                this.$emit('on-error', error)
            },
            validateBeforeChange (promiseFn, callback) {
                this.setValidationError(null)
                // we have a promise
                if (promiseFn.then && typeof promiseFn.then === 'function') {
                    this.setLoading(true)
                    promiseFn.then((res) => {
                        this.setLoading(false)
                        let validationResult = res === true
                        this.executeBeforeChange(validationResult, callback)
                    }).catch((error) => {
                        this.setLoading(false)
                        this.setValidationError(error)
                    })
                    // we have a simple function
                } else {
                    let validationResult = promiseFn === true
                    this.executeBeforeChange(validationResult, callback)
                }
            },
            executeBeforeChange (validationResult, callback) {
                this.$emit('on-validate', validationResult, this.activeTabIndex)
                if (validationResult) {
                    callback()
                } else {
                    this.tabs[this.activeTabIndex].validationError = 'error'
                }
            },
            beforeTabChange (index, callback) {
                if (this.loading) {
                    return
                }
                let oldTab = this.tabs[index]
                if (oldTab && oldTab.beforeChange !== undefined) {
                    let tabChangeRes = oldTab.beforeChange()
                    this.validateBeforeChange(tabChangeRes, callback)
                } else {
                    callback()
                }
            },
            changeTab (oldIndex, newIndex) {
                let oldTab = this.tabs[oldIndex]
                let newTab = this.tabs[newIndex]
                if (oldTab) {
                    oldTab.active = false
                }
                if (newTab) {
                    newTab.active = true
                }
                this.activeTabIndex = newIndex
                this.currentTab = newTab
                this.checkStep()
                this.tryChangeRoute(newTab)
                this.increaseMaxStep()
                return true
            },
            tryChangeRoute (tab) {
                if (this.$router && tab.route) {
                    this.$router.push(tab.route)
                }
            },
            checkStep () {
                if (this.activeTabIndex === this.tabCount - 1) {
                    this.isLastStep = true
                } else {
                    this.isLastStep = false
                }
            },
            increaseMaxStep () {
                if (this.activeTabIndex > this.maxStep) {
                    this.maxStep = this.activeTabIndex
                }
            },
            nextTab () {
                let cb = () => {
                    if (this.activeTabIndex < this.tabCount - 1) {
                        this.changeTab(this.activeTabIndex, this.activeTabIndex + 1)
                    } else {
                        this.isLastStep = true
                        this.$emit('finished')
                    }
                }
                this.beforeTabChange(this.activeTabIndex, cb)
            },
            prevTab () {
                let cb = () => {
                    if (this.activeTabIndex > 0) {
                        this.changeTab(this.activeTabIndex, this.activeTabIndex - 1)
                        this.isLastStep = false
                    }
                }
                this.beforeTabChange(this.activeTabIndex, cb)
            },
            finish () {
                let cb = () => {
                    this.$emit('on-complete')
                }
                this.beforeTabChange(this.activeTabIndex, cb)
            }
        },
        mounted () {
            this.tabs = this.$children.filter((comp) => comp.$options.name === 'tab-content')
            if (this.tabs.length > 0 && this.startIndex === 0) {
                this.currentTab = this.tabs[this.activeTabIndex]
                this.currentTab.active = true
                this.tryChangeRoute(this.currentTab)
            }
            if (this.startIndex < this.tabs.length) {
                this.activeTabIndex = this.startIndex
                this.currentTab = this.tabs[this.startIndex]
                this.currentTab.active = true
                this.maxStep = this.startIndex
                this.tryChangeRoute(this.currentTab)
            } else {
                console.warn(`Prop startIndex set to ${this.startIndex} is greater than the number of tabs - ${this.tabs.length}. Make sure that the starting index is less than the number of tabs registered`)
            }
        },
        watch: {
            'goToTab.status': function (newStatus) {
                if (newStatus === true){
                    this.navigateToTab(this.goToTab.tabIndex);
                    this.goToTab.status = false;
                }
            }
        }
    }
</script>
<style>
    * {padding:0;marging:0}
    .vue-wizard{
        box-sizing: border-box;
    }

    .tab-content-title{
        text-align: center;
        color: #7f7f7f;
    }
    .wizard-nav{
        width: 50%;
        margin: 20px auto;
        position: relative;
        text-align: center;
        display: -webkit-box;
        display: -ms-flexbox;
        display: flex;
        -ms-flex-wrap: wrap;
        flex-wrap: wrap;
        list-style: none;
    }
    .wizard-nav li{
        -webkit-box-flex: 1;
        -ms-flex: 1;
        flex: 1;
        -webkit-box-align: center;
        -ms-flex-align: center;
        align-items: center;
        -ms-flex-wrap: wrap;
        flex-wrap: wrap;
        -ms-flex-positive: 1;
        flex-grow: 1;
        position: relative;
        display: block;
    }
    .vue-wizard .icon-circle{
        font-size: 18px;
        border: 3px solid #f3f2ee;
        border-radius: 50%;
        width: 50px;
        height: 50px;
        background-color: #f3f2ee;
        position: relative;
        display: -webkit-box;
        display: -ms-flexbox;
        display: flex;
        -webkit-box-pack: center;
        -ms-flex-pack: center;
        justify-content: center;
        -ms-flex-line-pack: center;
        align-content: center;
        margin: 0 auto;
    }
    .vue-wizard .icon-circle .icon-container{
        display: -webkit-box;
        display: -ms-flexbox;
        display: flex;
        -webkit-box-pack: center;
        -ms-flex-pack: center;
        justify-content: center;
        -webkit-box-flex: 1;
        -ms-flex: 1;
        flex: 1;
        border-radius: 45%;
    }
    .vue-wizard .icon-circle .icon-changed{
        display: -webkit-box;
        display: -ms-flexbox;
        display: flex;
        -webkit-box-pack: center;
        -ms-flex-pack: center;
        justify-content: center;
        -webkit-box-flex: 1;
        -ms-flex: 1;
        flex: 1;
        border-radius: 45%;
    }
    .vue-wizard .icon-circle .icon{
        display: -webkit-box;
        display: -ms-flexbox;
        font-size: 22px;
        display: flex;
        -webkit-box-align: center;
        -ms-flex-align: center;
        align-items: center;
        -webkit-box-pack: center;
        -ms-flex-pack: center;
        justify-content: center;
    }
    .wizard-nav .icon-container .icon{
        color: #fff;
        font-size: 26px;
    }
    .wizard-nav .icon-changed .icon{
        color: #fff;
        font-size: 26px;
    }
    .card-footer{
        margin: 10px auto;
    }
    .card-footer .btn-wd{
        min-width:140px;
        margin-bottom: 20px;
    }

</style>