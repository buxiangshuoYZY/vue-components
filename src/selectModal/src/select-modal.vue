<template>
  <div>
    <div class="cl-select" :class="{'translate': isOpen}">
      <div class="header">
        <span class="cancel" @click="onCancel">取消</span>
        <span class="title">{{title}}</span>
        <span class="ok" @click="onComplete">完成</span>
      </div>
      <slot name="top"></slot>
      <div class="content">
        <div class="items" ref="items" @click="clickItems($event)">
          <template v-for="(item,index) in data">
            <button :class="{'selected': item.selected}" :data-value="item.value">{{item.label}}</button>
          </template>
        </div>
      </div>
      <slot name="bottom"></slot>
    </div>
    <div class="select-mask" :class="{'transition': isOpen}"></div>
  </div>
</template>

<script>
  export default {
    props: {
      data: Array,
      title: String,
      multip: {
        type: Boolean,
        default: false
      }
    },
    data () {
      return {
        isOpen: false
      }
    },
    mounted () {
    },
    methods: {
      open () {
        this.isOpen = true
      },
      hide () {
        this.isOpen = false
      },
      clickItems (event) {
        let btns = this.$refs.items.children
        for (let i = 0; i < btns.length; i++) {
          btns[i].className = ''
        }
        event.target.className = 'selected'
      },
      clickBtn (event) {
        if (!this.multip) {
          this.clickItems(event)
        } else {
          let cls = event.target.className
          if (cls) {
            event.target.className = ''
          } else {
            event.target.className = 'selected'
          }
        }
      },
      onComplete () {
        let btns = this.$refs.items.children
        let btn
        let res = []
        for (let i = 0; i < btns.length; i++) {
          btn = btns[i]
          if (btn.className === 'selected') {
            res.push({
              label: btn.innerText,
              value: btn.dataset.value
            })
          }
        }
        this.onCancel()
        res.length === 1 ? this.$emit('on-change', res[0]) : this.$emit('on-change', res)
      },
      onCancel () {
        this.isOpen = false
      }
    }
  }
</script>

<style scoped>
  .cl-select{
    position: fixed;
    bottom: 0;
    left: 0;
    z-index: 5000;
    background: #fff;
    width: 100%;
    font-size: 14px;
    -webkit-transform: translateY(100%);
    transform: translateY(100%);
    -webkit-transition:all .3s ;
  }
  .cl-select.translate{
    -webkit-transform: translateY(0);
    transform: translateY(0);
  }
  .cl-select .header{
    height: .90rem;
    line-height: .90rem;
    font-size: 16px;
    border-bottom: 1px solid rgb(217,217,217);
    text-align: center;
  }
  .cl-select .header span{
    display: inline-block;
  }
  .cl-select .header span.cancel{
    color: rgb(159,159,159);
    width: 20%;
    text-align: left;
  }
  .cl-select .header span.ok{
    color: rgb(46,166,242);
    width: 20%;
    text-align: right;
  }
  .cl-select .header span.title{
    width: 50%;
    text-align: center;
  }
  .cl-select .content{
    display: flex;
    flex-wrap: wrap;
    justify-content: center;
    padding: 10px 0;
    overflow-y: auto;
    max-height: 500px;
    -webkit-overflow-scrolling: touch;
    overflow-scrolling: touch;
  }
  .cl-select .content .items{
    width: 82%;
    text-align: left;
  }
  .cl-select .content button{
    width: 2.29rem;
    height: .67rem;
    line-height: .67rem;
    margin: .16rem;
    border-radius: 6px;
    border: 1px solid rgb(217,217,217);
    background: #fff;
    font-size: 16px;
  }
  .cl-select .content button.selected{
    background: rgb(237,248,255);
    border-color: rgb(46,166,242);
    color: rgb(46,166,242);
  }
  .c-select .desc{
    text-align: center;
    margin-bottom: 10px;
    color: rgb(159,159,159);
  }
  .select-mask{
    position: absolute;
    top: 0;
    width: 100%;
    height: 0%;
    left: 0;
    background: rgba(0,0,0,0);
    z-index: 1000;
    -webkit-transition:all .3s ;
    transition:all .3s ;
  }
  .select-mask.transition{
    height: 100%;
    background: rgba(0,0,0,.6);
  }
</style>