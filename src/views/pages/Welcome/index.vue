<!--
 * @Description: In User Settings Edit
 * @Author: your name
 * @Date: 2019-08-17 23:54:33
 * @LastEditTime: 2019-09-03 10:28:12
 * @LastEditors: Please set LastEditors
 -->
<template lang="pug">
.index.layout-column__between(v-if="userInfo.UserType !== 'Company'")
  el-row(:gutter='20')
    el-col(:span='8')
      #company.default()
    el-col(:span='8')
      #yjinfo.default()
    el-col(:span='8')
      #carkf.default()
  el-row(:gutter='20')
    el-col(:span='8')
      #project.default()
    el-col(:span='16')
      #staffs.default()
.index.layout-column__between(v-else)
  el-row(:gutter='20')
    el-col(:span='8')
      #staffs.default()
    el-col(:span='8')
      #yjinfo.default()
    el-col(:span='8')
      #carkf.default()
  el-row(:gutter='20')
    el-col(:span='8')
      #project.default()
    el-col(:span='16')
      #company.default()

</template>

<script >
import echarts from 'echarts'
import { getBaseDataInfo, getProjectDataInfo } from '@/api/dics'
import { mapGetters } from 'vuex'
export default {
  name: 'Index',
  components: {

  },
  data() {
    return {
      // 公司
      company: '',
      companyData: [],
      // 人员
      staffs: '',
      stafffsData: [],
      staffsXinfo: [],
      staffsYinfo: [],
      // 车辆库房
      carkf: '',
      carkfData: [],
      kfData: [],
      carData: [],
      baseData: {},
      // 项目
      project: '',
      projectData: {},
      prfuData: [],
      prData: [],
      // 预警
      yjinfo: '',
      yjinfoData: [],
      yjinfoXinfo: [],
      yjinfoYinfo: []
    }
  },
  computed: {
    ...mapGetters(['userInfo'])
  },
  created() {
    this.getBaseDataInfoList()
    this.getProjectDataInfoList()
  },
  mounted() {
    this.$nextTick(() => {
      if (this.userInfo.UserType !== 'Company') {
        this.company = echarts.init(document.getElementById('company'))
      }
      this.staffs = echarts.init(document.getElementById('staffs'))
      this.yjinfo = echarts.init(document.getElementById('yjinfo'))
      this.carkf = echarts.init(document.getElementById('carkf'))
      this.project = echarts.init(document.getElementById('project'))
      window.resize = () => {
        this.company.resize()
        this.staffs.resize()
        this.yjinfo.resize()
        this.carkf.resize()
        this.project.resize()
      }
      this.initCharts()
    })
  },
  methods: {
    initCharts() {
      if (this.userInfo.UserType !== 'Company') {
        this.setCompany()
      }
      this.setStaffs()
      this.setYjinfo()
      this.setCarkf()
      this.setProject()
    },
    setCompany() {
      const option = {
        color: ['#FBAF64', '#62AAEE', '#5AB1EF', '#B6A2DE', '#91c7ae', '#749f83', '#ca8622', '#bda29a', '#6e7074', '#546570', '#c4ccd3'],
        title: {
          text: '公司数量',
          fontWeight: '100',
          left: 'center',
          top: 20,
          textStyle: {
            color: '#333'
          }
        },
        tooltip: {
          trigger: 'item',
          formatter: '{a} <br/>{b} : {c} ({d}%)'
        },
        // visualMap: {
        //   show: false,
        //   min: 80,
        //   max: 600,
        //   inRange: {
        //     colorLightness: [0, 1]
        //   }
        // },
        series: [{
          name: '公司类别',
          type: 'pie',
          radius: '65%',
          center: ['50%', '55%'],
          data: this.companyData,
          itemStyle: {
            emphasis: {
              shadowBlur: 10,
              shadowOffsetX: 0,
              shadowColor: 'rgba(0, 0, 0, 0.5)'
            }
          }
        }]
      }
      this.company.setOption(option, true)
    },
    setStaffs() {
      var option
      if (this.userInfo.UserType !== 'Company') {
        option = {
          color: ['#3398DB'],
          title: {
            text: '人员类型',
            fontWeight: '100',
            left: 'center',
            top: 20,
            textStyle: {
              color: '#333'
            }
          },
          tooltip: {
            trigger: 'axis',
            axisPointer: { // 坐标轴指示器，坐标轴触发有效
              type: 'shadow' // 默认为直线，可选为：'line' | 'shadow'
            }
          },
          grid: {
            left: '3%',
            right: '4%',
            bottom: '5%',
            containLabel: true
          },
          xAxis: [
            {
              type: 'category',
              data: this.staffsXinfo,
              axisTick: {
                alignWithLabel: true
              },
              axisLabel: {
                interval: 0
              // rotate: 45
              }
            }
          ],
          yAxis: [
            {
              type: 'value'
            }
          ],
          series: [
            {
              name: '人员数量',
              type: 'bar',
              label: {
                normal: {
                  show: true,
                  position: 'inside'
                }
              },
              barWidth: '50%',
              data: this.staffsYinfo
            }
          ]
        }
      } else {
        option = {
          color: ['#3398DB'],
          title: {
            text: '人员类型',
            fontWeight: '100',
            left: 'center',
            top: 20,
            textStyle: {
              color: '#333'
            }
          },
          tooltip: {
            trigger: 'axis',
            axisPointer: { // 坐标轴指示器，坐标轴触发有效
              type: 'shadow' // 默认为直线，可选为：'line' | 'shadow'
            }
          },
          grid: {
            left: '3%',
            right: '4%',
            bottom: '5%',
            containLabel: true
          },
          xAxis: [
            {
              type: 'category',
              data: this.staffsXinfo,
              axisTick: {
                alignWithLabel: true
              },
              axisLabel: {
                interval: 1
              // rotate: 45
              }
            }
          ],
          yAxis: [
            {
              type: 'value'
            }
          ],
          series: [
            {
              name: '人员数量',
              type: 'bar',
              label: {
                normal: {
                  show: true,
                  position: 'inside'
                }
              },
              barWidth: '60%',
              data: this.staffsYinfo
            }
          ]
        }
      }
      this.staffs.setOption(option, true)
    },
    setYjinfo() {
      const option = {
        color: ['#FF2C23'],
        title: {
          text: '预警情况',
          fontWeight: '100',
          left: 'center',
          top: 20,
          textStyle: {
            color: '#333'
          }
        },
        tooltip: {
          trigger: 'axis',
          axisPointer: { // 坐标轴指示器，坐标轴触发有效
            type: 'shadow' // 默认为直线，可选为：'line' | 'shadow'
          }
        },
        grid: {
          left: '3%',
          right: '4%',
          bottom: '5%',
          containLabel: true
        },
        xAxis: [
          {
            type: 'category',
            data: this.yjinfoXinfo,
            axisTick: {
              alignWithLabel: true
            },
            axisLabel: {
              interval: 0
              // rotate: 45
            }
          }
        ],
        yAxis: [
          {
            type: 'value'
          }
        ],
        series: [
          {
            name: '预警数量',
            type: 'bar',
            barWidth: '50%',
            label: {
              normal: {
                show: true,
                position: 'top'
              }
            },
            data: this.yjinfoYinfo
          }
        ]
      }
      this.yjinfo.setOption(option, true)
    },
    setCarkf() {
      const option = {
        color: ['#FBAF64', '#62AAEE', '#B6A2DE', '#91c7ae', '#749f83', '#ca8622', '#bda29a', '#6e7074', '#546570', '#c4ccd3'],
        title: {
          text: '库房、车辆',
          fontWeight: '100',
          left: 'center',
          top: 20,
          textStyle: {
            color: '#333'
          }
        },
        tooltip: {
          trigger: 'item',
          formatter: '{a} <br/>{b}: {c} ({d}%)'
        },
        legend: {
          orient: 'vertical',
          x: '10',
          y: '30',
          data: ['自有房', '租用房', '自有车', '租用车']
        },
        series: [
          {
            name: '仓库类型',
            type: 'pie',
            selectedMode: 'single',
            radius: [0, '30%'],
            center: ['50%', '55%'],
            label: {
              normal: {
                position: 'inner'
              }
            },
            labelLine: {
              normal: {
                show: false
              }
            },
            data: this.kfData
          },
          {
            name: '车辆类型',
            type: 'pie',
            radius: ['40%', '55%'],
            center: ['50%', '55%'],
            label: {
              // normal: {
              //   formatter: '{a|{a}}{abg|}\n{hr|}\n  {b|{b}：}{c}  {per|{d}%}  ',
              //   backgroundColor: '#eee',
              //   borderColor: '#aaa',
              //   borderWidth: 1,
              //   borderRadius: 4,
              //   // shadowBlur:3,
              //   // shadowOffsetX: 2,
              //   // shadowOffsetY: 2,
              //   // shadowColor: '#999',
              //   // padding: [0, 7],
              //   rich: {
              //     a: {
              //       color: '#999',
              //       lineHeight: 22,
              //       align: 'center'
              //     },
              //     // abg: {
              //     //     backgroundColor: '#333',
              //     //     width: '100%',
              //     //     align: 'right',
              //     //     height: 22,
              //     //     borderRadius: [4, 4, 0, 0]
              //     // },
              //     hr: {
              //       borderColor: '#aaa',
              //       width: '100%',
              //       borderWidth: 0.5,
              //       height: 0
              //     },
              //     b: {
              //       fontSize: 16,
              //       lineHeight: 33
              //     },
              //     per: {
              //       color: '#eee',
              //       backgroundColor: '#334455',
              //       padding: [2, 4],
              //       borderRadius: 2
              //     }
              //   }
              // }
              rich: {
                a: {
                  color: '#999',
                  lineHeight: 22,
                  align: 'center'
                },
                // abg: {
                //     backgroundColor: '#333',
                //     width: '100%',
                //     align: 'right',
                //     height: 22,
                //     borderRadius: [4, 4, 0, 0]
                // },
                hr: {
                  borderColor: '#aaa',
                  width: '100%',
                  borderWidth: 0.5,
                  height: 0
                },
                b: {
                  fontSize: 16,
                  lineHeight: 33
                },
                per: {
                  color: '#eee',
                  backgroundColor: '#334455',
                  padding: [2, 4],
                  borderRadius: 2
                }
              }
            },
            data: this.carData
          }
        ]
      }
      this.carkf.setOption(option, true)
    },
    setProject() {
      const option = {
        color: ['#10C4EE', '#c4ccd3'],
        title: {
          text: '项目类别',
          fontWeight: '100',
          left: 'center',
          top: 20,
          textStyle: {
            color: '#333'
          }
        },
        tooltip: {
          trigger: 'axis',
          axisPointer: { // 坐标轴指示器，坐标轴触发有效
            type: 'shadow' // 默认为直线，可选为：'line' | 'shadow'
          },
          formatter: function(params) {
            var tar = params[1]
            return tar.name + '<br/>' + tar.seriesName + ' : ' + tar.value
          }
        },
        grid: {
          left: '3%',
          right: '4%',
          bottom: '4%',
          containLabel: true
        },
        xAxis: {
          type: 'category',
          splitLine: { show: false },
          data: ['总量', 'A级', 'B级', 'C级', 'D级', '非等级项目'],
          axisTick: {
            alignWithLabel: true
          },
          axisLabel: {
            interval: 0
            // rotate: 45
          }
        },
        yAxis: {
          type: 'value'
        },
        series: [
          {
            name: '辅助',
            type: 'bar',
            stack: '总量',
            itemStyle: {
              normal: {
                barBorderColor: 'rgba(0,0,0,0)',
                color: 'rgba(0,0,0,0)'
              },
              emphasis: {
                barBorderColor: 'rgba(0,0,0,0)',
                color: 'rgba(0,0,0,0)'
              }
            },
            data: this.prfuData
          },
          {
            name: '项目数量',
            type: 'bar',
            stack: '总量',
            barCategoryGap: '40%',
            label: {
              normal: {
                show: true,
                position: 'inside'
              }
            },
            data: this.prData
          }
        ]
      }
      this.project.setOption(option, true)
    },
    // 获取数据
    getBaseDataInfoList() {
      getBaseDataInfo().then(res => {
        this.baseData = res.Data
        this.companyData = [
          {
            value: res.Data.ComNum1,
            name: '营业性'
          },
          {
            value: res.Data.ComNum2,
            name: '非营业性'
          }
        ]
        this.stafffsData = [
          {
            value: res.Data.UserNum1,
            name: '技术负责人'
          },
          {
            value: res.Data.UserNum2,
            name: '治安保卫负责人'
          },
          {
            value: res.Data.UserNum3,
            name: '工程技术负责人'
          },
          {
            value: res.Data.UserNum4,
            name: '爆破员'
          },
          {
            value: res.Data.UserNum5,
            name: '安全员'
          },
          {
            value: res.Data.UserNum6,
            name: '保管员'
          },
          {
            value: res.Data.UserNum7,
            name: '驾驶员'
          },
          {
            value: res.Data.UserNum8,
            name: '押运员'
          },
          {
            value: res.Data.UserNum9,
            name: '其他人员'
          },
          {
            value: res.Data.UserNum0,
            name: '企业法人'
          }
        ]
        this.stafffsData.map(n => {
          this.staffsXinfo.push(n.name)
          this.staffsYinfo.push(n.value)
        })

        this.yjinfoData = [
          {
            value: res.Data.yujinProjectnum,
            name: '项目预警'
          },
          {
            value: res.Data.yujincarnum,
            name: '车辆预警'
          },
          {
            value: res.Data.yujincomnum,
            name: '公司预警'
          },
          {
            value: res.Data.yujinkuqunum,
            name: '库区预警'
          },
          {
            value: res.Data.yujinusernum,
            name: '人员预警'
          }
        ]
        this.yjinfoData.map(n => {
          this.yjinfoXinfo.push(n.name)
          this.yjinfoYinfo.push(n.value)
        })

        this.carData = [
          {
            name: '自有车',
            value: res.Data.CarNum1
          },
          {
            name: '租用车',
            value: res.Data.CarNum2
          }
        ]
        this.kfData = [
          {
            name: '自有房',
            value: res.Data.kufangNum1
          },
          {
            name: '租用房',
            value: res.Data.kufangNum2
          }
        ]
        this.initCharts()
      })
    },
    getProjectDataInfoList() {
      getProjectDataInfo().then(res => {
        const projectData = res.Data
        this.projectData = [
          {
            value: projectData.ProjectDengjiNum,
            name: '总量'
          },
          {
            value: projectData.ProjectNum0,
            name: 'A级'
          },
          {
            value: projectData.ProjectNum1,
            name: 'B级'
          },
          {
            value: projectData.ProjectNum2,
            name: 'C级'
          },
          {
            value: projectData.ProjectNum3,
            name: 'D级'
          },
          {
            value: projectData.ProjectNum4,
            name: '非等级项目'
          }
        ]
        this.projectData.map((n, index, arr) => {
          this.prData.push(n.value)
          // this.prfuData.push()
          if (index === 0) {
            this.prfuData.push(0)
          } else if (index === 1) {
            this.prfuData.push(parseInt(this.prData[0]) - parseInt(this.prData[1]))
          } else {
            this.prfuData.push(parseInt(this.prfuData[index - 1]) - parseInt(n.value))
          }
        })
        this.initCharts()
      })
    }
  }
}
</script>
<style lang="scss" scoped>
.index{
  background: #f0f2f5;
  height: 100%;
  padding: 15px;
  // flex-wrap: wrap;
  // &>div{
  //   width: 33%;
  //   background: #fff;
  //   height: 45%;
  // }
  /deep/ .el-row{
    height: 49%;
    .el-col{
      height: 100%;
    }
  }
}
.default{
  height:100%;
  width:100%;
  background: #fff
}
</style>
