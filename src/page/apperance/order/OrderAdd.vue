<template>
    <div>
        <CrossWiseTimeline :shiproute="shipRoute"></CrossWiseTimeline>
        <transition mode="out-in">
            <router-view
                :form="form"
                @changeForm="changeForm"
                @submit="submit"
            ></router-view>
        </transition>
    </div>
</template>

<script>
import axios from "axios";
import CrossWiseTimeline from "@/components/apperance_order/CrossWiseTimeline";
import setCargoInfo from "@/components/apperance_order/setCargoInfo.vue";
import setBoxesInfo from "@/components/apperance_order/setBoxesInfo.vue";
import setShipcompanyInfo from "@/components/apperance_order/setShipcompanyInfo.vue";
import setRelatedStaff from "@/components/apperance_order/setRelatedStaff.vue"

import portlist from "@/assets/data/portlist.ts";
import shipcompany from "@/assets/data/shipcompanydata.ts";
export default {
    name: "OrderAdd",
    data() {
        return {
            shipRoute: [
                {
                    site: "",
                    type: "porting",
                },
                {
                    site: "",
                    type: "end",
                },
                {
                    site: "",
                    type: "end",
                },
                {
                    site: "",
                    type: "end",
                },
            ],
            addOrderVisible: false,
            form: {
                id: "",
                name: "",
                model: "",
                type: "",
                qua: "",
                allTime: "",
                unit: "",
                Line: portlist.line, //路线列表
                isSelectLine: true, //控制是否可选港口
                LinePorts: {
                    ports: [], //从下面中选取
                    portlist: portlist.portlist,
                    value1: "",
                    value2: "",
                    value3: "",
                },
                shipcompany: shipcompany.company, //船公司列表
                shipInfo: {
                    ships: [], //从下面中选取
                    shiplist: shipcompany.shiplist,
                    value: "",
                },
                shipcompanyid: "",
                shipid: "",
                boxesData: [
                    {
                        last: true,
                    },
                ],
                //form数据的终点，得放进store里
            },
        };
    },
    mounted() {
        this.$router.push("setCargoInfo");
    },
    components: {
        axios,
        CrossWiseTimeline,
        setCargoInfo,
        setBoxesInfo,
        setShipcompanyInfo,
        shipcompany,
        setRelatedStaff,
    },
    methods: {
        changeForm(form) {
            this.$router.push(form);
        },

        submit() {
            let form = this.form;
            form.boxesData.pop();
            let datas = {
                id: form.id, //货物id
                name: form.name, //货物名称
                qua: form.qua, //数量
                model: form.model,
                unit: form.unit, //单位
                boxQua: form.boxQua, //集装箱数量
                allTime: form.allTime, //周期
                Line: form.Line, //航线代码
                LinePorts: [
                    //港口代码
                    form.LinePorts.value1,
                    form.LinePorts.value2,
                    form.LinePorts.value3,
                ],
                boxesData: form.boxesData,
            };
            axios({
                headers: {
                    "Content-Type": "application/json",
                },
                method: "POST",
                url: "http://baidu.com",
                data: datas,
            });
        },
    },
    setup() {
        const state = reactive({
            value1: "",
            value2: "",
        });
        return toRefs(state);
    },
};
</script>
<style scoped>
.el-button--mini {
    display: none;
}
.v-enter {
    opacity: 0;
}
.v-enter-active {
    transition: 0.25s;
}
.v-enter-to {
    opacity: 1;
}
.v-leave {
    opacity: 1;
}
.v-leave-to {
    opacity: 0;
}
.v-leave-active {
    transition: 0.25s;
}
</style>
