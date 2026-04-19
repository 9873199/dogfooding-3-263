<template>
    <div class="visitorpie">
        <div id="visitorpie" ref="chart" style="width: 90%;height:450px;"></div>
    </div>
</template>

<script>
    import echarts from 'echarts/lib/echarts';
    import 'echarts/lib/chart/pie';
    import 'echarts/lib/component/title';
    import 'echarts/lib/component/legend';
    
    export default {
        mounted(){
            this.$nextTick(() => {
                this.myChart = echarts.init(this.$refs.chart);
                this.initData();
            });
        },
        props: {
            pieData: {
                type: Object,
                default: () => ({
                    beijing: 0,
                    shanghai: 0,
                    shenzhen: 0,
                    hangzhou: 0,
                    qita: 0
                })
            }
        },
        methods: {
            initData(){
                const data = [
                    {value: this.pieData.beijing || 0, name: '北京'},
                    {value: this.pieData.shanghai || 0, name: '上海'},
                    {value: this.pieData.shenzhen || 0, name: '深圳'},
                    {value: this.pieData.hangzhou || 0, name: '杭州'},
                    {value: this.pieData.qita || 0, name: '其他'}
                ];
                
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
                            animationType: 'scale',
                            animationEasing: 'elasticOut',
                            animationDelay: function (idx) {
                                return Math.random() * 200;
                            },
                            data: data,
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

                this.myChart.setOption(option, true);
            }
        },
        watch: {
            pieData: {
                deep: true,
                handler: function (){
                    if (this.myChart) {
                        this.initData();
                    }
                }
            }
        },
        beforeDestroy() {
            if (this.myChart) {
                this.myChart.dispose();
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
