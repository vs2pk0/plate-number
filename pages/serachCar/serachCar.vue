<template>
    <!-- 车辆查询 -->
    <view class="search-car-all">
        <!-- 背景图 -->
		 <view class="bg-icon" :class="set">
		    <image :src="carBg" mode="" class="bac" />
		    <view class="bg-text">
		        <view class="bg-text-top">输 入 车 牌 号</view>
		        <view class="bg-text-bottom">输入完整车牌号，查询充电停车信息</view>
		    </view>
		</view>
		
        <!-- 输入的车牌 -->
        <view class="search-car-input" @click="carInputClick">
            <view class="car-input">
                <view class="energy" v-show="plateNum.length>7">
                    <image :src="energy" mode="" />
                </view>
                <view class="car-input-text">
                    {{plateNum}}
                </view>
                <view class="cursor" v-show="isCursor">
                    <image :src="cursor" mode="" />
                </view>
            </view>
            <view class="search-car" @click.stop="addSerach">添加并查询</view>
        </view>
		
		<!-- 引用车牌组件 -->
        <plate-number ref="plate" v-model="plateNum" @showOrHide="showOrHide"></plate-number>
		
    </view>
</template>

<script>
	import cursor from './cursor.gif'
	import energy from './energy.png'
	import carBg from './carBg.png'
	import plateNumber from '@/components/plate-number/plateNumber.vue'
    export default {
		components:{plateNumber},
        data() {
            return {
				carBg:'',
                plateNum: '', //输入的车牌号
                cursor: '', //输入焦点gif地址
                isCursor: true, //是否显示焦点
                energy: '', //新能源图标的地址
            };
        },
        onLoad(option) {},
        onHide() {
            //恢复初始化
            this.plateNum = '';
        },
        onShow() {
            //初始化
            // this.$refs.plate.init();
            this.cursor = cursor;
            this.energy = energy;
            this.carBg = carBg;
            this.carInputClick();
        },
        methods: {
            /**
             * @desc 车牌选择关闭和打开
             */
            showOrHide(falg) {
                this.isCursor = falg;
            },
            /**
             * @desc 显示车牌选择器
             */
            carInputClick() {
                this.$refs.plate.show();
            },
            /**
             * @desc 添加车牌方法
             */
            addSerach() {
                //输入了正确的车牌才执行请求
                if (this.plateNum.length > 6) {
                    this.$refs.plate.close();
					
                    //增加车牌方法
					uni.showToast({
						title: '车牌:'+this.plateNum,
						duration: 2000
					});
                } else {
					uni.showToast({
						title: '请输入正确的车牌号',
						duration: 2000
					});
                }
            }
        }
    }
</script>

<style lang="scss">
    @import "./serachCar.scss";
	
	.bg-icon {
	    width: 100vw;
	    position: relative;
	    display: flex;
		height: vww(174);
	    .bac {
	        width: 100%;
	        position: absolute;
	        left: 0;
	        top: 0;
	        height: 100%;
	        z-index: 1;
	        display: block;
	        @include bgCenterCustom($bg_Cover: cover);
	    }
	
	    .icon {
	        position: absolute;
	        display: block;
	        left: vww((375 - 75)/2);
	        z-index: 99;
	        width: vww(75);
	        height: vww(110);
	        top: vww((186 - 110)/2);
	        @include bgCenterCustom();
	    } 
	
	    //文字
	    .bg-text {
	        position: absolute;
	        left: 0;
	        top: 0;
	        z-index: 99;
	
	        .bg-text-top {
	            width: 100vw;
	            font-size: vww(26);
	            color: #FFFFFF;
	            height: vww(37);
	            line-height: vww(37);
	            text-align: center;
	            margin-top: vww(52);
	        }
	
	        .bg-text-bottom {
	            width: 100vw;
	            font-size: vww(12);
	            color: #FFFFFF;
	            height: vww(18);
	            line-height: vww(18);
	            text-align: center;
	        }
	    }
	}
</style>
