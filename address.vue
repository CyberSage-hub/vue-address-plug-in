<template>
  <div>
      <div id="info-wrap">
          <div id="pick-address">
              <label for="input-addresss" id="addr-label">所在地</label>
              <input v-model="site" @touchstart="open()" type="text" class="form-control"  readonly placeholder="请选择所在地区" id="input-addresss"/>
              <div v-show="show" class="cascading-address fadeIn">
                  <div id="pick" class="swing">
                      <div id="area-title">
                          <span id="title">所在地区</span>
                          <span @touchstart="close()" class="btn btn-link btn-sm" id="close"></span>
                      </div>
                      <div id="area-tab">
                          <span v-bind:class="[{ active: status1 }]" class="tab" v-on:touchstart="statusmethod(1)"><span id="tab1">{{ tabp }}</span></span>
                          <span v-bind:class="[{ active: status2 }]" class="tab" v-on:touchstart="statusmethod(2)"><span id="tab2">{{ tabn }}</span></span>
                          <span v-bind:class="[{ active: status3 }]" class="tab" v-on:touchstart="statusmethod(3)"><span id="tab3">{{ tabs }}</span></span>
                      </div>
                      <ul v-show="status1" class="area-wrap">
                          <li v-for="item in provinces" @touchstart="setProvince(item.p)" :class="{'label label-success': p === item.p}">{{ item.p }}</li>
                      </ul>

                      <ul  v-show="status2" class="area-wrap">
                          <li v-for="item in cities" @touchstart="setCity(item.n)" :class="{'label label-success': c === item.n}">{{ item.n }}</li>
                      </ul>

                      <ul v-show="status3" class="area-wrap">
                          <li v-for="item in areas" @touchstart="setArea(item.s,item.id)" :class="{'label label-success': a === item.s}">{{ item.s }}</li>
                      </ul>
                  </div>
              </div>
          </div>
      </div>
  </div>
</template>


<script type="text/babel">
    var Vue       = require('vue');
    import addressData from './cascadingAddressData.json'
    export default {
        data () {
            return {
                confirmData : '',
                addr_id:'',//地址id
                area_id:'',//区域id
                areaInfo:'',
                p: '',
                c: '',
                a: '',
                tabp:'请选择',
                tabn:'请选择',
                tabs:'请选择',
                provinces: addressData,
                areas: '',
                cities: '',
                show: false,
                site:'',
                status1: true,
                status2: false,
                status3: false,
                shop_id:''
            }
        },
        computed: {
            model(){
                return this.p ? (this.p + ' ' + this.c + ' ' + this.a) : '';
            }
        },
        methods: {
            //点击取消按钮
            cancel:function () {
                this.$router.go(-1);
            },
            open(){
                this.show = true;
            },
            close(){
                this.show = false;
            },
            setProvince(p){
                this.p = p;
                this.tabp = p;
                this.c = '';
                this.a = '';
                this.areas = [];
                // 根据选中的 p(省份) 值更新 cities(城市列表)
                var result = this.provinces.filter(function (v) {
                    return v.p === p;
                });
                this.cities = result[0].c || [];
                this.status1 = false;
                this.status2 = true;
                this.status3 = false;
            },
            setCity(c){
                this.c = c;
                this.a = '';
                this.tabn = c;
                var result = this.cities.filter(function (v) {
                    return v.n === c;
                });
                this.areas = result[0].a || [];
                this.status1 = false;
                this.status2 = false;
                this.status3 = true;
            },
            setArea(a,id){
                this.area_id = id;
                this.a = a;
                this.tabs = a;
                this.close();
                this.site = this.p+this.c+this.a;
            },
            statusmethod:function(index) {
                switch(index){
                    case 1:
                        this.status1=true;
                        this.status2=false;
                        this.status3=false;
                        break;
                    case 2:
                        this.status1=false;
                        this.status2=true;
                        this.status3=false;
                        break;
                    case 3:
                        this.status1=false;
                        this.status2=false;
                        this.status3=true;
                        break;
                }
            }
        },
        watch: {
            tabp: function (val, oldVal) {
                this.tabn = '请选择';
                this.tabs = '请选择';
            },
            tabp: function (val, oldVal) {
                this.tabn = '请选择';
                this.tabs = '请选择';
            }
        }
    }
</script>

<style scoped>
    #pick-address {
        margin-left: 0.4rem;
        height: 1.33333333rem;
        border-bottom: 1px solid #efefef;
        background: #fff;
        display: inline-block;
        width: 100%;
    }
    #addr-label {
        font-size: 15px;
        height: 1.33333333rem;
        line-height: 1.33333333rem;
        display: inline-block;
        vertical-align: middle;
        position: relative;
    }
    #input-addresss {
        font-size: 15px;
        vertical-align: middle;
        position: relative;
        height: 1.33333333rem;
        line-height: 1.33333333rem;
    }
    .addredit {
        background: #f5f8fb;
        height: 100%;
        position: absolute;
        width: 100%;
        overflow: hidden;
    }
    .cascading-address {
        position: absolute;
        z-index: 99999;
        background: rgba(0, 0, 0, 0.5);
        width: 100%;
        left: 0;
        top: 0;
        bottom: 0;
        right: 0;
    }
    #pick {
        width: 100%;
        bottom: 0;
        right: 0;
        left: 0;
        position: absolute;
        background: #fff;
    }
    .cascading-address ul {
        padding: 5px auto 5px 5%;
        text-align: left;
        overflow-y: auto;
        height: 200px;
    }
    .cascading-address ul li {
        font-size: 13px;
        list-style: none;
        display: block;
        margin: 3px 5px;
        cursor: pointer;
        padding: 2% 5%;
        border-radius: 3px;
        -webkit-tap-highlight-color: transparent;
    }
    .cascading-address .form-group {
        margin: 5px 10px;
    }
    .panel-footer {
        display: flex;
        justify-content: space-around;
        align-content: flex-start;
        margin: 10px 0;
        font-size: 15px;
        width: 100%;
        clear: both;
    }
    .panel-footer .btn {
        padding: 1% 2%;
        border: 0;
        background: #f95b27;
        color: #fff;
        border-radius: 2px;
    }
    @-webkit-keyframes slideInUp {
        from {
            -webkit-transform: translate3d(0, 100%, 0);
            transform: translate3d(0, 100%, 0);
            visibility: visible;
        }
        to {
            -webkit-transform: translate3d(0, 0, 0);
            transform: translate3d(0, 0, 0);
        }
    }
    @keyframes slideInUp {
        from {
            -webkit-transform: translate3d(0, 100%, 0);
            transform: translate3d(0, 100%, 0);
            visibility: visible;
        }
        to {
            -webkit-transform: translate3d(0, 0, 0);
            transform: translate3d(0, 0, 0);
        }
    }
    .swing {
        -webkit-transform-origin: top center;
        transform-origin: top center;
        -webkit-animation: slideInUp .2s;
        animation: slideInUp .2s;
    }
    @-webkit-keyframes fadeIn {
        from {
            opacity: 0;
        }
        to {
            opacity: 1;
        }
    }
    @keyframes fadeIn {
        from {
            opacity: 0;
        }
        to {
            opacity: 1;
        }
    }
    .fadeIn {
        -webkit-transform-origin: top center;
        transform-origin: top center;
        -webkit-animation: fadeIn .5s;
        animation: fadeIn .5s;
    }
    #area-tab {
        border-bottom: 1px solid #efefef;
        position: relative;
        height: 30px;
    }
    .tab {
        position: relative;
        font-size: 14px;
        width: 60px;
        display: inline-block;
        text-align: center;
        margin-left: 5%;
        height: 30px;
        line-height: 30px;
        float: left;
        overflow: hidden;
    }
    .tab.active {
        color: #f00;
    }
    .tab.active:after {
        content: "";
        background: #f00;
        display: inline-block;
        width: 60px;
        height: 1px;
        position: absolute;
        bottom: 0;
        left: 0;
        animation: move .5s;
    }
    @keyframes move {
        0% {
            left: -60px;
        }
        100% {
            left: 0;
        }
    }
    @-webkit-keyframes move {
        0% {
            left: -60px;
        }
        100% {
            left: 0;
        }
    }
    #area-title {
        font-size: 14px;
    }
    #title {
        width: 100%;
        display: inline-block;
        text-align: center;
        height: 30px;
        line-height: 30px;
    }
    #close {
        width: 0.56rem;
        height: 0.56rem;
        position: absolute;
        right: 10px;
        top: 10px;
        background: url('./h5_details_btn_close@2x.png') no-repeat;
        background-size: 100% auto;
    }
</style>
