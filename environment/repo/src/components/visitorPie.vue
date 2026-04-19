<template>
    <div class="visitorpie">
        <div id="visitorpie" class="" style="width: 90%;height:450px;"></div>
    </div>
</template>

<script>
    import echarts from 'echarts/lib/echarts';
    import 'echarts/lib/chart/pie';
    import 'echarts/lib/component/title';
    import 'echarts/lib/component/legend';
    
    export default {
        mounted(){
            this.myChart = echarts.init(document.getElementById('visitorpie'));
            if (this.pieData && Object.keys(this.pieData).length > 0) {
                this.initData();
            }
        },
        props: ['pieData'],
        methods: {
            initData(){
                if (!this.pieData || Object.keys(this.pieData).length === 0) {
                    return;
                }
                const option = {
                    title : {
                        text: '用户分布',
                        subtext: '',
                        x:'center'
                    },
                    tooltip : {
                        trigger: 'item',
                        formatter: "{a} <br/>{b} : {c} ({d}%)"
                    },
                    legend: {
                        orient: 'vertical',
                        left: 'left',
                        data: ['北京','上海','深圳','杭州','其他']
                    },
                    series : [
                        {
                            name: '访问来源',
                            type: 'pie',
                            radius : '55%',
                            center: ['50%', '60%'],
                            data:[
                                {value:this.pieData.beijing || 0, name:'北京'},
                                {value:this.pieData.shanghai || 0, name:'上海'},
                                {value:this.pieData.shenzhen || 0, name:'深圳'},
                                {value:this.pieData.hangzhou || 0, name:'杭州'},
                                {value:this.pieData.qita || 0, name:'其他'}
                            ],
                            itemStyle: {
                                emphasis: {
                                    shadowBlur: 10,
                                    shadowOffsetX: 0,
                                    shadowColor: 'rgba(0, 0, 0, 0.5)'
                                }
                            }
                        }
                    ]
                };

                this.myChart.setOption(option);
            }
        },
        watch: {
            pieData: {
                handler: function (newVal) {
                    if (newVal && Object.keys(newVal).length > 0) {
                        this.$nextTick(() => {
                            this.initData();
                        });
                    }
                },
                deep: true,
                immediate: true
            }
        }
    }
</script>

<style lang="less">
	@import '../style/mixin';
    .visitorpie{
        display: flex;
        justify-content: center;
        margin-top: 20px;
    }
</style>
