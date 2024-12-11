<template>
    <div>
        <div class="header">
            欢迎点餐
        </div>
        <div class="body">
            <el-input
                v-model="searchQuery"
                placeholder="请输入餐品名称进行搜索"
                @input="handleSearch"
                :prefix-icon="Search"
                style="width: 300px; margin-bottom: 20px;">
            </el-input>

            <el-table :data="filteredTableData" style="width: 100%" class="table" border>
                            <el-table-column prop="shop_name" label="菜品名称" width="200" align="center">
                            </el-table-column>
                            <el-table-column prop="price" label="菜品单价" width="200" align="center">
                            </el-table-column>
                            <el-table-column prop="sale" label="月销量" width="200" align="center">
                            </el-table-column>
                            <el-table-column prop="operate" label="操作" width="208" align="center">
                                <template slot-scope="scope">
                                    <el-button icon="el-icon-plus" size="small" type="success" @click="showdia(scope.row)">订餐
                                    </el-button>
                                </template>
                            </el-table-column>
                        </el-table>

            <el-dialog title="订餐表单" :visible.sync="dialog" class="dialog" width="40%">
                <div>
                    <el-form ref="form" :model="form" label-width="100px">
                        <el-form-item label="菜品名称：">
                            <span>{{ form.shop_name }}</span>
                            <!-- <el-input v-model="form.shop_name"></el-input> -->
                        </el-form-item>

                        <el-form-item label="菜品单价：">
                            <span>{{ form.order_money }}</span>
                            <!-- <el-input v-model="form.order_money"></el-input> -->
                        </el-form-item>

                        <el-form-item label="订餐方式：">
                            <el-select v-model="form.order_way" placeholder="请选择订餐方式">
                                <el-option label="人工订餐" value="人工订餐"></el-option>
                                <el-option label="网上订餐" value="网上订餐"></el-option>
                            </el-select>
                        </el-form-item>

                        <!-- <el-form-item label="客户电话：">
                            <el-input v-model="form.cons_phone"></el-input>
                        </el-form-item> -->

                        <el-form-item label="客户姓名：">
                            <el-input v-model="form.cons_name"></el-input>
                        </el-form-item>

                        <el-form-item label="送餐地址：">
                            <el-input v-model="form.cons_addre"></el-input>
                        </el-form-item>

                    </el-form>
                    <div style="text-align: center;">
                        <el-button type="primary" @click="add">
                            提交
                        </el-button>
                    </div>
                </div>
            </el-dialog>
        </div>
    </div>
</template>

<script>
export default {
    created() {
        this.getdata();
    },
    data() {
        return {
            tableData: [],
            filteredTableData: [],
            searchQuery: "", // 搜索框绑定数据
            dialog: false,
            form: {
                shop_name: '',
                order_money: '',
                order_way: '',
                cons_name: '',
                cons_addre: '',
            }
        }
    },
    methods: {
        getdata() {
            this.$axios.get("/api/user/shop").then((res) => {
                console.log(res.data);
                if (res.data.status == 200) {
                    this.tableData = res.data.tabledata;
                     this.filteredTableData = res.data.tabledata;  // 初始时不过滤，显示所有数据
                }
            })
        },

        // 处理搜索功能
            handleSearch() {
                        const query = this.searchQuery.trim().toLowerCase();
                        if (query) {
                            this.filteredTableData = this.tableData.filter(item => {
                                return item.shop_name.toLowerCase().includes(query);
                            });
                        } else {
                            this.filteredTableData = this.tableData;  // 如果没有输入内容，显示全部数据
                        }
                    },
        showdia(row) {
            this.form.shop_name = row.shop_name;
            this.form.order_money = row.price;
            this.dialog = true;
        },
        add() {
            this.$axios.post("/api/user/addorder", this.form).then((res) => {
                console.log(res.data);
                if (res.data.status == 200) {
                    this.$message({
                        message: "成功下单",
                        type: "success"
                    })
                    this.dialog = false;
                    this.getdata();
                }
            })
        }
    }
}
</script>
import { Search } from '@element-plus/icons-vue'

<style scoped>
.header {
    width: 100%;
    height: 10%;
    text-align: center;
    line-height: 64px;
    font-size: 20px;
    font-weight: 800;
    border-bottom: 1px solid #e3e3e3;
}

.body {
    width: 62%;
    margin: auto;
    margin-top: 30px;
}

/* .table {
    margin-left: 120px;
} */

.dialog {
    /* width: 700px; */
}
</style>