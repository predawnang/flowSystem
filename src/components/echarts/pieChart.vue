<template>
    <div :id="id" class="orderArea"></div>
</template>

<script>
	import echarts from 'echarts'
    import echartsTheme from "cps/echarts/theme/westeros.json";
    
    export default {
        data(){
            return {
				 id:'ordertype',
                 myChart:null,
            }
        },
		props: ['type'],
        mounted(){
			this.loadChart();
		},
		beforeDestroy() {
			if (!this.myChart) {
				return
			}
			this.myChart.dispose();
			this.myChart = null;
        },
        methods: {
			 loadChart(){
                this.$nextTick(() => {
                    echarts.registerTheme('westeros', echartsTheme)
                    this.myChart = echarts.init(document.getElementById(this.id),'westeros');
                    this.myChart.setOption(this.initOption(this.type));
                })
            },
         	initOption(type){
				let text,legend_data,series_data;
				if(type == "ordertype"){
					text = "企业用户地区分布";
					legend_data = ['湖北','云南','河南','山东','广东'];
					series_data = [
						{value:10, name:'湖北'},
						{value:45, name:'云南'},
						{value:30, name:'河南'},
						{value:25, name:'山东'},
						{value:60, name:'广东'}
					]
				}else{
					// text = "激活设备区域分布";
					// legend_data = ['华东区','华南区','华中区','华北区','西南区','东北区','台港澳'];
					// series_data = [
					// 	{value:335, name:'华东区'},
					// 	{value:310, name:'华南区'},
					// 	{value:234, name:'华中区'},
					// 	{value:835, name:'华北区'},
					// 	{value:1548, name:'西南区'},
					// 	{value:335, name:'东北区'},
					// 	{value:454, name:'台港澳'}
					// ]
				}
				let data = {
					  title : {
						text: text,
						x:'center'
					},
					tooltip : {
						trigger: 'item',
						formatter: "{a} <br/>{b} : {c} ({d}%)"
					},
					legend: {
						orient: 'vertical',
						left: 'left',
						data: legend_data
					},
					series : [
						{
							name: '访问来源',
							type: 'pie',
							radius : '50%',
                            center: ['50%', '60%'],
							data:series_data,
							itemStyle: {
								emphasis: {
									shadowBlur: 10,
									shadowOffsetX: 0,
									shadowColor: 'rgba(0, 0, 0, 0.5)'
								}
							}
						}
					]
				}
				return data;
			},
        },
        watch: {
            type:(v)=>{
                this.initOption(v)
            }
        }
    }
</script>

<style lang="less">

</style>
