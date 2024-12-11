<template>
    <div>
        <div class="header">
            &nbsp;&nbsp; 派送员后台管理平台
        </div>
        <div class="body">
            <div class="liner">
                <el-menu default-active="1" class="el-menu-vertical-demo" background-color="#545c64" text-color="#fff"
                    active-text-color="#ffd04b" @select="handleselect">

                    <el-menu-item index="1">
                        <i class="el-icon-s-order"></i>
                        <span slot="title">接单管理</span>
                    </el-menu-item>

                    <el-menu-item index="2">
                        <i class="el-icon-s-management"></i>
                        <span slot="title">订单管理</span>
                    </el-menu-item>

                </el-menu>
            </div>
            <div class="main">
                <!-- 接单管理 -->
                <div id="orderReception" v-show="active == 1">
                    <order-reception :orders="orders" @acceptOrder="acceptOrder"></order-reception>
                </div>

                <!-- 订单管理 -->
                <div id="orderManagement" v-show="active == 2">
                    <order-management :orders="acceptedOrders" @updateStatus="updateOrderStatus"></order-management>
                </div>
            </div>
        </div>
    </div>
</template>

<script>
import orderReception from '@/components/ManageOrder/Reception.vue';
import ordersended from '@/components/ManageOrder/BeSended.vue'
import ordersending from '@/components/ManageOrder/BeSending.vue'
import orderunsend from '@/components/ManageOrder/UnSend.vue'

export default {
    components: {
        orderReception,
        orderManagement
    },
    data() {
        return {
            active: 1,  // 默认显示接单管理
            orders: [
                { id: 1, customer: '张三', address: '某地1号', status: '待接单' },
                { id: 2, customer: '李四', address: '某地2号', status: '待接单' },
                { id: 3, customer: '王五', address: '某地3号', status: '待接单' },
            ],
            acceptedOrders: []
        };
    },
    methods: {
        handleselect(index) {
            this.active = index;
        },
        acceptOrder(order) {
            // 接单操作
            order.status = '进行中';
            this.acceptedOrders.push(order);
            // 从待接单列表中移除已接订单
            this.orders = this.orders.filter(o => o.id !== order.id);
        },
        updateOrderStatus(order, status) {
            // 更新订单状态
            order.status = status;
        }
    },
}
</script>

<style scoped>
.header {
    width: 100%;
    height: 10vh;
    display: flex;
    line-height: 10vh;
    font-size: 25px;
    font-weight: 800;
    background-color: gray;
}

.body {
    width: 100%;
    height: 648px;
    display: flex;
    justify-content: space-around;
}

.liner {
    width: 15%;
    height: 100%;
    background-color: #545c64;
}

.main {
    width: 85%;
}
</style>
