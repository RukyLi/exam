<template>
  <div class="detail">
    <el-row type="flex" align="middle" class="header">
      <el-col :span="6">
        <img class="img-style" :src="detail.platLogoUrl" alt="">
        <div style="text-align:center;margin-top:24px">
          <span>
            <span class="label">{{detail.onlineDate}}上线</span>
            <span class="label">{{detail.location}}</span>
          </span>
        </div>
      </el-col>
      <el-col :span="17" :offset="1">
        <div class="flex-box">
          <div class="plat-name">{{detail.platName}}</div>
          <span class="type">{{detail.platBackgroundDetailExpand}}</span>
          <span class="type" style="background:#F56C6C">{{score}}</span>
        </div>
        <!-- <div>首次出借</div> -->
        <el-row>
          <el-col :span="4">
            <div class="data">{{detail.platEarningsCode}}</div>
            <div class="label">参考回报(%)</div>
          </el-col>
          <el-col :span="4">
            <div class="data">{{detail.termWeight}}</div>
            <div class="label">出借日期(月)</div>
          </el-col>
          <el-col :span="4">
            <div class="data">{{detail.score|numFilter}}</div>
            <div class="label">点评(分)</div>
          </el-col>
          <el-col :span="4">
            <div class="data">{{detail.day_amount}}</div>
            <div class="label">昨日成交量(万)</div>
          </el-col>
          <el-col :span="4">
            <div class="data">{{detail.money_stock}}</div>
            <div class="label">昨日待还余额(万)</div>
          </el-col>
        </el-row>
        <el-popover
          placement="top-start"
          title=""
          width="400"
          trigger="hover"
        >
          <span slot="reference" class="type" style="margin:14px 0;cursor:pointer">近30日详情</span>
          <slot>
            <el-row type="flex">
              <el-col :span="10">成交量(万元):</el-col>
              <el-col :span="8">{{dayThirty.amount}}</el-col>
              <el-col :span="6">{{dayThirty.amount_percent|numFilter}}%</el-col>
            </el-row>
            <el-row type="flex">
              <el-col :span="10">投资人数:</el-col>
              <el-col  :span="8">{{dayThirty.bidder_num}}</el-col>
              <el-col :span="6">{{dayThirty.bidder_num_percent|numFilter}}%</el-col>
            </el-row>
            <el-row>
              <el-col :span="10">借款人数:</el-col>
              <el-col :span="8">{{dayThirty.borrower_num}}</el-col>
              <el-col :span="6">{{dayThirty.borrower_num_percent|numFilter}}%</el-col>
            </el-row>
            <el-row>
              <el-col :span="10">日资金流入(万元):</el-col>
              <el-col :span="8">{{dayThirty.net_inflow}}</el-col>
              <el-col :span="6">{{dayThirty.net_inflow_percent|numFilter}}%</el-col>
            </el-row>
            <el-row>
              <el-col :span="10">前十大借款人待还占比:</el-col>
              <el-col :span="8">{{dayThirty.bor_top10}}</el-col>
              <el-col :span="6">{{dayThirty.bor_top10_change|numFilter}}%</el-col>
            </el-row>
            <el-row>
              <el-col :span="10">前十大投资人待收占比:</el-col>
              <el-col :span="8">{{dayThirty.bid_top10}}</el-col>
              <el-col :span="6">{{dayThirty.bid_top10_change|numFilter}}%</el-col>
            </el-row>
          </slot>
        </el-popover>
      </el-col>
    </el-row>
    <div class="data-self">昨日核心数据</div>
    <el-row class="last-data">
      <el-col :span="12">
        <v-chart class="radar-chart" :options="radarChart" />
      </el-col>
      <el-col :span="12">
        <el-row class="main-data">
          <el-col :span="6">
            <div class="label">成交量(万元)</div>
            <div class="data">{{lastData['成交量']}}</div>
          </el-col>
          <el-col :span="6">
            <div class="label">待还余额(万元)</div>
            <div class="data">{{lastData['待还余额']}}</div>
          </el-col>
          <el-col :span="6">
            <div class="label">参考收益率(%)</div>
            <div class="data">{{lastData['参考收益率']}}</div>
          </el-col>
          <el-col :span="6">
            <div class="label">平均借款期限(月)</div>
            <div class="data">{{lastData['平均借款期限']}}</div>
          </el-col>
          <el-col :span="6">
            <div class="label">投资人数(人)</div>
            <div class="data">{{lastData['投资人数']}}</div>
          </el-col>
          <el-col :span="6">
            <div class="label">人均投资金额(万元)</div>
            <div class="data">{{lastData['人均投资金额']}}</div>
          </el-col>
          <el-col :span="6">
            <div class="label">待收投资人数(人)</div>
            <div class="data">{{lastData['待收投资人数']}}</div>
          </el-col>
          <el-col :span="6">
            <div class="label">借款人数(人)</div>
            <div class="data">{{lastData['借款人数']}}</div>
          </el-col>
          <el-col :span="6">
            <div class="label">人均借款金额(万元)</div>
            <div class="data">{{lastData['人均借款金额']}}</div>
          </el-col>
          <el-col :span="6">
            <div class="label">借款标数(个)</div>
            <div class="data">{{lastData['借款标数']}}</div>
          </el-col>
          <el-col :span="6">
            <div class="label">资金净流入(万元)</div>
            <div class="data">{{lastData['资金净流入']}}</div>
          </el-col>
        </el-row>
      </el-col>
    </el-row>
    <el-row>
      <div class="data-self">基础数据</div>
      <el-col :span="16">
        <v-chart style="width:800px" class="bar-chart" :options="barChart" />
      </el-col>
      <el-col :span="8">
        <div
          v-for="(item,index) in basicSelectList"
          :key="index"
          :class="basicSelect.value === item.value?'select-active':'select'"
          class="select"
          @click="onSelectBasic(item)"
        >{{ item.label }}</div>
      </el-col>
    </el-row>
    <el-row>
      <div class="data-self">数据对比</div>
      <el-col :span="16">
        <v-chart style="width:800px" class="line-chart" :options="lineChart" />
      </el-col>
      <el-col :span="8">
        <div
          v-for="(item,index) in dataSelectList"
          :key="index"
          :class="dataSelect.value === item.value?'select-active':'select'"
          class="select"
          @click="onSelectData(item)"
        >{{ item.label }}</div>
      </el-col>
    </el-row>
  </div>
</template>

<script>
import axios from '@/utils/request'
const request = axios.apiAxios
const colors={
  yellow:{normal:{color:'#E6A23C'}},
  green:{normal:{color:'#67C23A'}},
  red:{normal:{color:'#F56C6C'}},
  blue:{normal:{color:'#409EFF'}},
  dark:{nomral:{color:'#2f54eb'}}
}
export default {
  data() {
    return {
      infoDate:[],
      infoData:[],
      contrastDate:[],
      contrastData:[],
      basicSelect: {
        value: 1,
        label: '成交量',
        type: 1,
        target1: 1,
        target2: 0
      },
      dataSelect: {
        value: 1,
        label: '投资人数VS借款人数',
        type: 1,
        target1: 5,
        target2: 6
      },
      hoverContent: 'hoverContent',
      detail:{},
      dayThirty:{},
      lastData:{},
      score:'',
      basicSelectList: [
        {
          value: 1,
          label: '成交量',
          type: 1,
          target1: 1,
          target2: 0
        },
        {
          value: 2,
          label: '参考收益率',
          type: 1,
          target1: 2,
          target2: 0
        },
        {
          value: 3,
          label: '投资人数分级',
          type: 3,
          target1: 16,
          target2: 1
        },
        {
          value: 4,
          label: '借款人数分级',
          type: 3,
          target1: 16,
          target2: 2
        },
        {
          value: 5,
          label: '不同期限标的参考收益率',
          type: 3,
          target1: 17,
          target2: 1
        },
        {
          value: 6,
          label: '不同期限标的满标用时',
          type: 3,
          target1: 17,
          target2: 2
        }
      ],
      dataSelectList: [
        {
          value: 1,
          label: '投资人数VS借款人数',
          type: 1,
          target1: 5,
          target2: 6
        },
        {
          value: 2,
          label: '人均投资金额VS人均借款金额',
          type: 1,
          target1: 7,
          target2: 8
        },
        {
          value: 3,
          label: '新投资人数VS老投资人数',
          type: 1,
          target1: 19,
          target2: 20
        },
        {
          value: 4,
          label: '新投资总额VS老投资总额',
          type: 1,
          target1: 21,
          target2: 22
        },
        {
          value: 5,
          label: '平均借款期限VS行业平均期限',
          type: 1,
          target1: 10,
          target2: 23
        }
      ]

    }
  },
  computed: {
    radarChart() {
      const option = {
        title: {
          text: '基础雷达图'
        },
        legend: {
          data: ['']
        },
        tooltip:{},
        radar: {
            splitArea: {
              show:false
        },
        splitLine:{
          lineStyle:{
            color: [
                    'rgba(47,84,235, 0.1)', 'rgba(47,84,235, 0.2)',
                    'rgba(47,84,235, 0.4)', 'rgba(47,84,235, 0.6)',
                    'rgba(47,84,235, 0.8)', 'rgba(47,84,235, 1)'
                ]
          }
        },
          name: {
            textStyle: {
              color: '#fff',
              backgroundColor: '#2f54eb',
              borderRadius: 3,
              padding: [3, 5]
            }
          },
          indicator: [
            { name: '成交量'},
            { name: '投资人数' },
            { name: '平均借款期限'},
            { name: '参考收益率'},
            { name: '待还余额'},
            { name: '资金净流入' }
          ]
        },
        series: [{
          name: '昨日核心数据',
          type: 'radar',
          itemStyle:{
            lineStyle:{
              color:"#3fd2e0"
            },
            color:'#3fd2e0'
          },
          data: [
            {
              value: [
                this.lastData['成交量'], 
                this.lastData['投资人数'], 
                this.lastData['平均借款期限'], 
                this.lastData['参考收益率'], 
                this.lastData['待还余额'], 
                this.lastData['资金净流入']],
            },
          ]
        }]
      }
      return option
    },
    barChart() {
      const {series,legend} = this.barSeries()
      const index = this.basicSelect.value-1
      const unit = ['万元','%','人','人','%','秒']
      const option = {
        title: {
          text: '基础数据'
        },
        tooltip: {},
        legend: {data:legend},
        xAxis: {
          data: this.infoDate,
        },
        yAxis: {
          name: this.basicSelect.label+'('+unit[index]+')'
        },
        dataZoom: [{
          type: 'inside',
          startValue:0,                      
          endValue:10,
        }, {
          type: 'slider',
          startValue:0,                      
          endValue:10, 
        }],
        series:series
       
      }
      return option
    },
    lineChart() {
      const name1 = this.dataSelect.label.split('VS')[0]
      const name2 =  this.dataSelect.label.split('VS')[1]
       const index = this.dataSelect.value-1
      const unit = ['人','万元','人','万元','月']
      const option = {
        
        title: {
          text: '折线图堆叠'
        },
        tooltip: {
          trigger: 'axis'
        },
        legend: {
          data: [name1, name2]
        },
        grid: {
          left: '100px',
          right: '4%',
          bottom: '3%',
          containLabel: true
        },
        dataZoom: [{
          type: 'inside',
          startValue:0,                      
          endValue:10,
        }, {
          type: 'slider',
          startValue:0,                      
          endValue:10, 
        }],
        toolbox: {
          feature: {
            saveAsImage: {}
          }
        },
        xAxis: {
          type: 'category',
          boundaryGap: false,
          data: this.contrastDate
        },
        yAxis: {
          padding:'10px',
          type: 'value',
          name:  this.dataSelect.label+'('+unit[index]+')'
        },
        series: [
          {
            name: name1,
            type: 'line',
            stack: '总量',
            data: this.contrastData.data1,
            lineStyle:{
              color:'#F56C6C'
            },
          },
          {
            name: name2,
            type: 'line',
            stack: '总量',
            data: this.contrastData.data2,
            lineStyle:{
              color:'#67C23A'
            },
          }
        ]
      }
      return option
    }
  },
  filters:{
    numFilter(val){
      if(val){
        return val.toFixed(2)
      }
    }
  },
  created(){
    this.getDetailData()
    this.getInfo()
    this.getContrast()
  },
  methods: {
    onSelectBasic(item){
      this.basicSelect = item
      this.getInfo()
    },
    onSelectData(item){
      this.dataSelect = item
      this.getContrast()
    },
     barSeries(){
      const emphasisStyle = {
        itemStyle: {
          barBorderWidth: 1,
          shadowBlur: 10,
          shadowOffsetX: 0,
          shadowOffsetY: 0,
          shadowColor: 'rgba(0,0,0,0.5)'
        }
      }
      const seriesConfig = {
        type: 'bar',
        stack: 'one',
        emphasis: emphasisStyle
      }
      let value = this.basicSelect.value;
      let series = []
      let legend = []
      if(value===1||value===2){
        series = [
          {
            type: 'bar',
            data: this.infoData,
            barWidth: 25,
            itemStyle: {
              normal: {
                color: ['#67C23A']
              }
            },
          }
        ]
        legend = ''
      }else if(value===3){
        const chartSeries = [
          {data:'y1',color:'yellow',name:'0-1万投资人'},
          {data:'y2',color:'red',name:'1-10万投资人'},
          {data:'y3',color:'green',name:'10-100万投资人'},
          {data:'y4',color:'blue',name:'100万以上投资人'},
        ]
        series = this.getSeriesConfig(chartSeries,seriesConfig)
        legend = ['0-1万投资人','1-10万投资人','10-100万投资人','100万以上投资人']
      }else if(value===4){
        const chartSeries = [
          {data:'y5',color:'yellow',name:'0-20万借款人'},
          {data:'y6',color:'red',name:'20-100万借款人'},
          {data:'y7',color:'green',name:'100万以上借款人'},
        ]
        series = this.getSeriesConfig(chartSeries,seriesConfig)
         legend =  ['0-20万借款人','20-100万借款人','100万以上借款人']
      }else if(value===5){
          const chartSeries = [
          {data:'y1',color:'yellow',name:'0-1个月（%）'},
          {data:'y2',color:'red',name:'1-2个月（%）'},
          {data:'y3',color:'green',name:'2-3个月（%）'},
          {data:'y4',color:'blue',name:'3-6个月（%）'},
          {data:'y5',color:'dark',name:'6个月及以上（%）'}
        ]
        series = this.getSeriesConfig(chartSeries,seriesConfig)
         legend = ['0-1个月（%）','1-2个月（%）','2-3个月（%）','3-6个月（%）','6个月及以上（%）']

      }else if(value===6){
          const chartSeries = [
          {data:'y6',color:'yellow',name:'0-1个月（s）'},
          {data:'y7',color:'red',name:'1-2个月（s）'},
          {data:'y8',color:'green',name:'2-3个月（s）'},
          {data:'y9',color:'blue',name:'3-6个月（s）'},
          {data:'y10',color:'dark',name:'6个月及以上（s）'}
        ]
        series = this.getSeriesConfig(chartSeries,seriesConfig)
         legend = ['0-1个月（s）','1-2个月（s）','2-3个月（s）','3-6个月（s）','6个月及以上（s）']

       
      }
      return {series,legend}
    },
    getSeriesConfig(configArr,seriesConfig){
      let seriesArr = []
      for(let item of configArr){
        seriesArr.push(
           {...seriesConfig,name:item.name,itemStyle:colors[item.color],data: this.infoData[item.data]},
        )
      }
      return seriesArr
    },
    async getDetailData(){
      let platId = this.$route.query.platId
      try {
        let data={
          platId
        }
      const result = await request({ url: '/head',method:'post', data })
      this.detail = result.data.data[0]
      this.dayThirty = result.data.data[0].day_thirty
      this.score = result.data.data[2].score
      this.lastData = result.data.data[1]
      } catch (error) {
        
      }
    },
     async getInfo(){
      let platId = this.$route.query.platId
      let {type,target1,target2} = this.basicSelect
      try {
        let data={
          wdzjPlatId:platId,
          type,
          target1,
          target2
        }
      const result = await request({ url: '/info',method:'post', data })
      this.infoDate = result.data.date;
      this.infoData = result.data.data1;
      } catch (error) {
        
      }
    },
     async getContrast(){
      let platId = this.$route.query.platId
      let {type,target1,target2} = this.dataSelect
      try {
        let data={
          wdzjPlatId:platId,
          type,
          target1,
          target2
        }
      const result = await request({ url: '/contrast',method:'post', data })
      this.contrastDate = result.data.data.date
      this.contrastData = result.data.data
      } catch (error) {
        
      }
    }
  }

}
</script>

<style lang="scss" scoped>
.detail{
  .img-style{
    width: 230px;
    height: 80px;
    object-fit: contain;
    margin: 0 auto;
    display: block;
  }
  .select{
    color: #2f54eb;
    padding: 8px 100px 8px 16px;
    border-radius: 15px;
    font-size: 14px;
    width: fit-content;
    font-weight: 500;
    margin: 10px 0;
    cursor: pointer;
  }
  .select-active{
    background: #2f54eb;
    color: #fff;
    padding: 8px 100px 8px 16px;
    border-radius: 15px;
    font-size: 14px;
    width: fit-content;
    font-weight: 500;
    margin: 10px 0;
     cursor: pointer;
  }
  .data-self{
    font-size: 24px;
    border-bottom: 1px solid;
    color: #343874;
    padding: 12px;
    font-weight: 550;
    margin: 10px 0;
  }
  .header{
    padding: 24px 0;
  }
  .type{
    background: #2f54eb;
    color: #fff;
    border-radius: 30px;
    padding: 4px 12px;
    margin: 0 15px;
    width: fit-content;
    display: inline-block;
    font-size: 12px;
  }
  .flex-box{
    display: flex;
    align-items: center;
  }
  .plat-name{
    width: fit-content;
    color: #343874;
    font-size: 30px;
    font-weight: 600;
  }
  .data{
    font-size: 28px;
    font-weight: 500;
    line-height: 52px;
    margin-top: 10px;
    color: #343874;
    font-family: Helvetica;
  }
  .label{
    font-size: 14px;
    font-weight: 500;
    line-height: 18px;
    color: #343874;
  }
  .last-data{
    .label{
    font-size: 14px;
    font-weight: 500;
    line-height: 18px;
    color: #343874;
    position: relative;
    padding-left: 16px;;
    &:before{
      content: '';
      display: block;
      width: 10px;
      height: 10px;
      background: #3fd2e0;
      border-radius: 50%;
      position: absolute;
      left: 0;
      top: 4px;
    }
    
  }
  }
  .main-data{
    .el-col{
      // border-bottom: 1px dashed #999;
      padding: 15px 0 30px;
    }
  }
}
</style>
