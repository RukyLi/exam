<template>
  <div class="analyse-container">
    <div class="search-params">
      <el-row type="flex" align="middle">
        <el-col :span="2" class="label">数据时间:</el-col>
        <el-col :span="22">
          <span
            v-for="(item,index) in timeList"
            :key="index"
            :class="searchObj.time === item.value?'select-active':'select'"
            @click="onSelectTime(item)"
          >{{ item.label }}</span>
        </el-col>
      </el-row>
      <el-row type="flex" align="middle">
        <el-col :span="2" class="label">平台背景:</el-col>
        <el-col :span="22">
          <span
            v-for="(item,index) in flatList"
            :key="index"
            :class="searchObj.flat === item.value?'select-active':'select'"
            @click="onSelectFlat(item)"
          >{{ item.label }}</span>
        </el-col>
      </el-row >
      <el-row class="data-self">数据自选指标</el-row>
      <el-row type="flex" align="middle">
        <el-col :span="2" class="label">按维度:</el-col>
        <el-col :span="22">
          <span
            v-for="(item,index) in dimensionList"
            :key="index"
            :class="searchObj.dimension === item.value?'select-active':'select'"
            @click="onSelectDimension(item)"
          >{{ item.label }}</span>
        </el-col>
      </el-row>
      <el-row type="flex" align="middle">
        <el-col :span="2" class="label">按数据项:</el-col>
        <el-col :span="22">
          <span
            v-for="(item,index) in dataList"
            :key="index"
            :class="containData(index)?'select-active':'select'"
            @click="onSelectData(item,index)"
          >{{ item.label }}</span>
        </el-col>
      </el-row>
    </div>
    <el-table
      ref="propsRef"
      v-loading="loading"
      :empty-text="'暂无数据'"
      :data="tableData"
    >
      <el-table-column label="序号">
        <template slot-scope="scope">
          <div>{{ scope.$index +1 }}</div>
        </template>
      </el-table-column>
      <el-table-column label="平台">
        <template slot-scope="scope">
          <div class="flat-link" @click="goFlatDetail(scope.row.wdzjPlatId)">{{ scope.row.platName }}</div>
        </template>
      </el-table-column>
      <el-table-column label="成交量（万元）" v-if="containData(0)">
        <template slot-scope="scope">
          <div>{{ scope.row.amount }}</div>
        </template>
      </el-table-column>
      <el-table-column label="平均参考收益率（%）" v-if="containData(1)">
        <template slot-scope="scope">
          <div>{{ scope.row.incomeRate }}</div>
        </template>
      </el-table-column>
       <el-table-column label="平均借款期限（月）" v-if="containData(2)">
        <template slot-scope="scope">
          <div>{{ scope.row.loanPeriod }}</div>
        </template>
      </el-table-column>
      <el-table-column label="资金净流入（万元）" v-if="containData(3)">
        <template slot-scope="scope">
          <div>{{ scope.row.netInflowOfThirty }}</div>
        </template>
      </el-table-column>
       <el-table-column label="待还余额（万元）" v-if="containData(4)">
        <template slot-scope="scope">
          <div>{{ scope.row.stayStillOfTotal }}</div>
        </template>
      </el-table-column>
      <el-table-column label="满标用时（分）" v-if="containData(5)">
        <template slot-scope="scope">
          <div>{{ scope.row.fullloanTime }}</div>
        </template>
      </el-table-column>
      <el-table-column label="注册资本（万元）" v-if="containData(6)">
        <template slot-scope="scope">
          <div>{{ scope.row.regCapital }}</div>
        </template>
      </el-table-column>
      <el-table-column label="运营时间（月）" v-if="containData(7)">
        <template slot-scope="scope">
          <div>{{ scope.row.timeOperation }}</div>
        </template>
      </el-table-column>
      <el-table-column label="借款标数（个）" v-if="containData(8)">
        <template slot-scope="scope">
          <div>{{ scope.row.totalLoanNum }}</div>
        </template>
      </el-table-column>
      <el-table-column label="投资人数（人）" v-if="containData(9)">
        <template slot-scope="scope">
          <div>{{ scope.row.bidderNum }}</div>
        </template>
      </el-table-column>
      <el-table-column label="人均投资金额（万元）" v-if="containData(10)">
        <template slot-scope="scope">
          <div>{{ scope.row.avgBidMoney }}</div>
        </template>
      </el-table-column>
      <el-table-column label="前十大土豪待收金额占比（%）" v-if="containData(11)">
        <template slot-scope="scope">
          <div>{{ scope.row.top10DueInProportion }}</div>
        </template>
      </el-table-column>
      <el-table-column label="借款人数（人）" v-if="containData(12)">
        <template slot-scope="scope">
          <div>{{ scope.row.borrowerNum }}</div>
        </template>
      </el-table-column>
      <el-table-column label="人均借款金额（万元）" v-if="containData(13)">
        <template slot-scope="scope">
          <div>{{ scope.row.avgBorrowMoney }}</div>
        </template>
      </el-table-column>
      <el-table-column label="前十大借款人待还金额占比（%）" v-if="containData(14)">
        <template slot-scope="scope">
          <div>{{ scope.row.top10StayStillProportion }}</div>
        </template>
      </el-table-column>
      <el-table-column label="发展指数排名" v-if="containData(15)">
        <template slot-scope="scope">
          <div>{{ scope.row.developZhishu|developFilter }}</div>
        </template>
      </el-table-column>
    </el-table>
  </div>
</template>

<script>
import axios from '@/utils/request'
import { isExternal } from '../../utils/validate'
const request = axios.apiAxios
const INIT_ITEMS=[
  [0,1,2,4],
  [5,9,10,11],
  [0,6,7],
  [8,12,13,14]
]
export default {
  name: 'Dashboard',
  data() {
    return {
      loading:false,
      tableData:[],
      dataCopy:[],
      searchObj: {
        time: 1,
        flat: '',
        dimension: 1,
        data: 1
      },
      activeName: 'user',
      currentIndex: 0,
      timeList: [
        {
          value: 1,
          label: '昨日'
        },
        {
          value: 7,
          label: '近7日'
        },
        {
          value: 30,
          label: '近30日'
        }
      ],
      flatList: [
        {
          value: '',
          label: '全部'
        },
        {
          value: 1,
          label: '民营系'
        },
        {
          value: 2,
          label: '银行系'
        },
        {
          value: 3,
          label: '上市系'
        },
        {
          value: 4,
          label: '国资系'
        },
        {
          value: 5,
          label: '风投系'
        }
      ],
      dataList: [
        {
          label: '成交量',
          value: 1
        },
        {
          label: '平均参考收益率',
          value: 2
        },
        {
          label: '平均借款期限',
          value: 3
        },
        {
          label: '资金净流入',
          value: 4
        },
        {
          label: '待还余额',
          value: 5
        },
        {
          label: '满标用时',
          value: 6
        },
        {
          label: '注册资本',
          value: 7
        },
        {
          label: '运营时间',
          value: 8
        },
        {
          label: '借款标数',
          value: 9
        },
        {
          label: '投资人数',
          value: 10
        },
        {
          label: '人均投资金额',
          value: 11
        },
        {
          label: '前十大土豪待收金额占比',
          value: 12
        },
        {
          label: '借款人数',
          value: 13
        },
        {
          label: '人均借款金额',
          value: 14
        },
        {
          label: '前十大借款人待还金额占比',
          value: 15
        },
        {
          label: '发展指数排名',
          value: 16
        }
      ],
      dimensionList: [
        {
          value: 1,
          label: '按成交量',
          dataItems:[0,1,2,4],
        },
        {
          value: 2,
          label: '按人气',
          dataItems:[5,9,10,11],
        },
        {
          value: 3,
          label: '按平台运营',
          dataItems:[0,6,7],
        },
        {
          value: 4,
          label: '按分散性',
          dataItems:[8,12,13,14],
        }
      ]
    }
  },
  created(){
    this.getTableData()
  },
  filters:{
    developFilter(val){
      if(val===0){
        return '-'
      }else{
        return val
      }
    }
  },
  methods: {
    containData(index){
      let isContain = this.dimensionList[this.searchObj.dimension-1].dataItems.includes(index)
      return isContain
    },
    async getTableData(){
      this.loading = true
      try {
        const data = {
          date:this.searchObj.time
        }
        const result = await request({ url: '/list',method:'post', data })
        this.tableData = result.data.data
        this.dataCopy = JSON.parse(JSON.stringify(result.data.data))
        this.loading = false
      } catch (e) {

      }
    },
    handleClick() {

    },
    onSelectData(item,dataIndex){
      let dataItems = this.dimensionList[this.searchObj.dimension-1].dataItems
      if(dataItems.includes(dataIndex)){
        let deleteIndex = dataItems.findIndex(f=>f===dataIndex)
        dataItems.splice(deleteIndex,1)
      }else{
        dataItems.push(dataIndex)
      }
      console.log(dataItems,'dtaitem',dataIndex)
      this.dimensionList[this.searchObj.dimension-1].dataItems = dataItems
    },
    onSelectTime(time){
      this.searchObj.time = time.value
      this.getTableData()
    },
    onSelectFlat(flat){
      this.searchObj.flat = flat.value
      if(flat.value!==''){
        let dataCopy = this.dataCopy
        this.tableData = dataCopy.filter(item=>item.newbackground===flat.label)
      }else{
        this.getTableData()
      }
    },
    onSelectDimension(item){
      this.searchObj.dimension = item.value
      let index = this.searchObj.dimension-1
      this.dimensionList[index].dataItems = JSON.parse(JSON.stringify(INIT_ITEMS[index]))
      // console.log(this.dimensionList[index].dataItems,INIT_ITEMS[index])
    },
    goFlatDetail(platId) {
      this.$router.push({ path: '/detail', query: { platId }})
    }
  }
}
</script>

<style lang="scss" scoped>
.analyse-container{
  .el-row{
    padding:5px 0;
  }
  /deep/.el-table th>.cell{
    color:#343874!important;
    font-weight: 600;
  }
  
  .el-table td div{
    color: #343874;;
  }
  .data-self{
    text-align: center;
    background: #2f54eb;
    color: #fff;
    font-size: 14px;
    font-weight: 500;
    padding: 13px 0;;
  }
  .label{
    color: #343874;
    font-weight: 500;
    font-size: 14px;
    width:80px;
  }
  .select{
    font-size: 14px;
    margin: 8px;
    display: inline-block;
    cursor: pointer;
    border-radius: 30px;
     padding: 4px 10px;
    width: fit-content;
    color:#343874;

  }
  .select-active{
    color: #fff;
    background-color: #2f54eb;
    border-radius: 30px;
    padding: 4px 10px;
    width: fit-content;
    font-size: 14px;
    margin: 8px;
    display: inline-block;
    cursor: pointer;
  }
  .flat-link{
    color:#2f54eb!important;
    cursor: pointer;
  }

}
</style>
