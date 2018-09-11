<template>
	<div>
		<h2>{{this.msg}}的本周运动统计</h2>
		<div id="myChart" class="myChart"></div>
		<div id="myChart2" class="myChart2"></div>
	</div>
</template>
<script>
import axios from 'axios'
export default {
  name: 'Home',
  data () {
    return {
      msg: '本日运动'
    }
  },
  mounted(){
   	this.getHomeInfo()
    	console.log(this.$route.params.id)
  },
  methods: {
  	getHomeInfo () {
	      axios.get('http://www.scjuchuang.com/images/dist/index.json')
	        .then(this.getHomeInfoSucc)
	},
	getHomeInfoSucc(res){
		if(res&&res.data){
			const data=eval('(' + res.data + ')');
			var datas1=[],datas2=[],datas3=[],datas4=[],datas5=[[0,0,0,0,0],[0,0,0,0,0],[0,0,0,0,0],[0,0,0,0,0],[0,0,0,0,0]
			];
			// console.log(data.data)
			// console.log(data)
			var mydate=new Date();
		  	var myddy=mydate.getDay()-1;//获取存储当前日期
		  	var weekday=["周日","周一","周二","周三","周四","周五","周六"];
			data.data.arry.map((item,i)=>{
				if(item.keys==this.$route.params.id){
					this.msg=item.name;
					item.content.map((it,j)=>{
						if(it.name==weekday[myddy]){
							it.content.map((jj,k)=>{
								datas1.push(jj.name);
								datas2.push(jj.time*jj.nice)
							})
							// datas1.push()
						}
					})
				}
			})
			data.data.arry.map((item,i)=>{
				if(item.keys==this.$route.params.id){
						item.content.map((it,j)=>{
						it.content.map((jj,k)=>{
									console.log(jj)
								datas3[k]=jj.time*jj.nice
						})
						datas4.push(datas3)
					})
				}
				
			})
			datas4.map((item,i)=>{
				for (var j = 0; j < item.length; j++) {
					// var datas6=[];
					// datas6[j]=item[j]
					// console.log(datas6)
					datas5[j][i] =item[j]+datas5[j][i]
					// console.log(typeof(item[j]))
				}
			})
			console.log(datas5)
			this.drawLine(datas1,datas2,datas5);
		}
	},
    drawLine(datas1,datas2,datas5){
        // 基于准备好的dom，初始化echarts实例
        let myChart = this.$echarts.init(document.getElementById('myChart'))
        let myChart2 = this.$echarts.init(document.getElementById('myChart2'))
        // 绘制图表
        myChart.setOption({
            title: { text: '昨天运动' },
            color: ['#3398DB'],
		    tooltip : {
		        trigger: 'axis',
		        axisPointer : {            // 坐标轴指示器，坐标轴触发有效
		            type : 'shadow'        // 默认为直线，可选为：'line' | 'shadow'
		        }
		    },
		    grid: {
		        left: '3%',
		        right: '4%',
		        bottom: '3%',
		        containLabel: true
		    },
		    xAxis : [
		        {
		            type : 'category',
		            data : datas1,
		            axisTick: {
		                alignWithLabel: true
		            }
		        }
		    ],
		    yAxis : [
		        {
		            type : 'value'
		        }
		    ],
		    series : [
		        {
		            name:'直接访问',
		            type:'bar',
		            barWidth: '60%',
		            data:datas2
		        }
		    ]
        });
        myChart2.setOption({
		    title: {
		        text: '本周运动'
		    },
		    tooltip : {
		        trigger: 'axis',
		        axisPointer: {
		            type: 'cross',
		            label: {
		                backgroundColor: '#6a7985'
		            }
		        }
		    },
		    // legend: {
		    //     data:['墙蹲','交叉','蛙跳','平板','手腕']
		    // },
		    toolbox: {
		        feature: {
		            saveAsImage: {}
		        }
		    },
		    grid: {
		        left: '3%',
		        right: '4%',
		        bottom: '3%',
		        containLabel: true
		    },
		    xAxis : [
		        {
		            type : 'category',
		            boundaryGap : false,
		            data : ['周一','周二','周三','周四','周五','周六','周日']
		        }
		    ],
		    yAxis : [
		        {
		            type : 'value'
		        }
		    ],
		    series : [
		        {
		            name:'靠墙蹲',
		            type:'line',
		            stack: '总量',
		            areaStyle: {normal: {}},
		            data:datas5[0]
		        },
		        {
		            name:'交叉步',
		            type:'line',
		            stack: '总量',
		            areaStyle: {normal: {}},
		            data:datas5[1]
		        },
		        {
		            name:'蛙跳',
		            type:'line',
		            stack: '总量',
		            areaStyle: {normal: {}},
		            data:datas5[2]
		        },
		        {
		            name:'平板支撑',
		            type:'line',
		            stack: '总量',
		            areaStyle: {normal: {}},
		            data:datas5[3]
		        },
		        {
		            name:'绕手腕',
		            type:'line',
		            stack: '总量',
		            label: {
		                normal: {
		                    show: true,
		                    position: 'top'
		                }
		            },
		            areaStyle: {normal: {}},
		            data:datas5[4]
		        }
		    ]
		})
    }
  }
}	
</script>
<style style="less" scoped>
	.myChart{
		height: 3rem;
		width: 100%;
		margin: 0 auto
	}
	.myChart2{
		height: 3rem;
		width: 100%;
		margin: 0 auto
	}
	h2{
		font-size: .2rem;
		text-align: center;
		color: #ae188a;
		margin: .15rem 0;
	}
</style>