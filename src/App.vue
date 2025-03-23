<template>
    <div id='root'>
        <div class='header'>
            <div class='button-app' v-if='page!=0' @click="page=0">
                <img src='./assets/back.svg'/>
            </div>
            <div class='title home' v-if='page==0'>Fake Yiban</div>
            <div class='title' v-if='page==-1'>关于</div>
            <div class='title' v-else-if='page==1'>{{studentName}}的请假</div>
            <div class='title' v-else-if='page==2'>请假</div>
            <div class='button-app' v-if='page==0' @click='page=-1'>
                <img src='./assets/about.svg'/>
            </div>
            <div class='button-app' v-if='page>0'>
                <img src='./assets/close.svg'/>
            </div>
            <div class='button-app' v-if='page>0'>
                <img src='./assets/menu.svg'/>
            </div>
        </div>
        <div class='content' v-if='page==0'>
            <div class="edit-title-detail">
                <div class="edit-title-detail-dot"></div>申请人
            </div>
            <div class="edit-detail-content">
                <div class='row no-delete'>
                    <div class='label'>姓名</div>
                    <div class='label'>学号</div>
                </div>
                <div class='row no-delete'>
                    <input type='text' v-model='studentName'>
                    <input type='text' v-model='studentID'>
                </div>
            </div>
            <div class="edit-title-detail">
                <div class="edit-title-detail-dot"></div>申请内容
            </div>
            <div class="edit-detail-content">
                <div class='row no-delete'>
                    <div class='label'>项目</div>
                    <div class='label'>内容</div>
                </div>
                <div v-for='item, num in items' :key='item'>
                    <div class='row' v-if='deletingItem==num' @blur="deletingItem=-1">
                        <button class='confirm' @click="items.splice(num, 1);deletingItem=-1">删除</button>
                        <button class='cancel' @click="deletingItem=-1">取消</button>
                    </div>
                    <div class='row' v-else>
                        <input type='text' v-model='item.name'>
                        <input type='text' v-model='item.value'>
                        <button class='delete' @click="deletingItem=num">
                            <img src='./assets/delete.svg'/>
                        </button>
                    </div>
                </div>
                <button class='add' @click="items.push({name: '', value: ''})">
                    <img src='./assets/add.svg'/>
                </button>
            </div>
            <div class="edit-title-detail">
                <div class="edit-title-detail-dot"></div>审批进度
            </div>
            <div class="edit-detail-content">
                <div class='row no-delete'>
                    <div class='label'>姓名</div>
                    <div class='label'>编号</div>
                    <div class='label'>时间</div>
                </div>
                <div v-for='approver, num in approvers' :key='approver'>
                    <div class='row' v-if='deletingApprover==num'>
                        <button class='confirm' @click="approvers.splice(num, 1);deletingApprover=-1">删除</button>
                        <button class='cancel' @click="deletingApprover = -1">取消</button>
                    </div>
                    <div class='row' v-else>
                        <input type='text' v-model='approver.name'>
                        <input type='text' v-model='approver.id'>
                        <input type='text' v-model='approver.time'>
                        <button class='delete' @click="deletingApprover = num">
                            <img src='./assets/delete.svg'/>
                        </button>
                    </div>
                </div>
                <button class='add' @click="approvers.push({name: '', id: '', time: ''})">
                    <img src='./assets/add.svg'/>
                </button>
            </div>
            <div class="edit-title-detail">
                <div class="edit-title-detail-dot"></div>已抄送
            </div>
            <div class="edit-detail-content">
                <div class='row no-delete'>
                    <div class='label'>姓名</div>
                    <div class='label'>编号</div>
                </div>
                <div v-for='cc, num in ccs' :key='cc'>
                    <div class='row' v-if='deletingCC==num'>
                        <button class='confirm' @click="ccs.splice(num, 1);deletingCC=-1">删除</button>
                        <button class='cancel' @click="deletingCC=-1">取消</button>
                    </div>
                    <div class='row' v-else>
                        <input type='text' v-model='cc.name'>
                        <input type='text' v-model='cc.id'>
                        <button class='delete' @click="deletingCC=num">
                            <img src='./assets/delete.svg'/>
                        </button>
                    </div>
                </div>
                <button class='add' @click="ccs.push({name: '', id: ''})">
                    <img src='./assets/add.svg'/>
                </button>
            </div>
        </div>
        <div class='final' v-if='page==0'>
            <button @click='go'>表单详情</button>
            <div class='separator'></div>
            <button @click='page=2'>离返校码</button>
        </div>
        <fake v-if='page == 1' :student-name='studentName' :studentID="studentID"
            :items='items' :approvers="approvers" :ccs="ccs" @exit='page=0'></fake>
        <QRCode v-if='page == 2' @exit='page=0'></QRCode>
        <div class='about' v-if='page == -1'>
            <img src='./assets/logo.png'>
            <h2>Fake Yiban 1.0.0</h2>
            <p>By <a href='https://github.com/supertsy5/'>SUPERTSY5</a></p>
            <p>警告：本程序仅供演示，请勿滥用。一切后果自负。</p>
        </div>
    </div>
</template>

<script>
import Fake from './components/fake/Fake.vue'
import QRCode from './components/fake/QRCode.vue'
export default {
    name: 'App',
    components: {
        Fake,
        QRCode
    },
    data(){
        return {
            page: 0,
            studentName: '王婧婧',
            studentID: '202213501110',
            items: [
                {
                    name: "审批编号",
                    value: "20210101080000123456"
                },
                {
                    name: "是否离校",
                    value: "是"
                },
                {
                    name: "开始时间",
                    value: "2021-01-01 08:00"
                },
                {
                    name: "结束时间",
                    value: "2021-01-02 08:00"
                },
                {
                    name: "时长",
                    value: "1"
                },
                {
                    name: "请假类型",
                    value: "病假"
                },
                {
                    name: "性别",
                    value: "女"
                },
                 {
                    name: "本人联系电话",
                    value: "13190594210"
                },
                 {
                    name: "家长姓名",
                    value: "代新梅"
                },
                 {
                    name: "家长有效联系方式（手机，固话，邮箱）",
                    value: "13245172084"
                },
                 {
                    name: "学生现住宿舍号",
                    value: "307"
                },
                 {
                    name: "请假期间是否离校",
                    value: "是"
                },
                 {
                    name: "请假期间是否在宿舍住宿",
                    value: "是"
                },
                 {
                    name: "佐证材料",
                    value: ""
                },
                 {
                    name: "请假事由",
                    value: "病假，医院就医"
                },
                 {
                    name: "请假详细原因",
                    value: "急性肠胃炎"
                },
                {
                    name: "请假去向具体地址",
                    value: "内蒙古医科大学附属医院"
                }
            ],
            approvers: [
                 {
                name: '候睿',
                id: '20070000035',
                time: '2021-01-01 08:00'
                  },

            ],
            ccs: [ {
                name: '贾金同',
                id: '20060000237'
            },
            {
                name: '李雨衡',
                id: '202013501064'
            },
            {
                name: '丛彧哲',
                id: '20230000066'
            }
            ],
            deletingItem: -1,
            deletingApprover: -1,
            deletingCC: -1
        }
    },
    methods: {
        go(){
            window.localStorage.setItem('last-data', JSON.stringify(this.$data));
            this.page = 1;
        },
        setButtonCancel(el){
            if(el){
                this.buttonCancel = el;
            }
        }
    },
    mounted(){
    // 此处不再从 localStorage 读取数据
}

}
</script>

<style src='./app.css'></style>
