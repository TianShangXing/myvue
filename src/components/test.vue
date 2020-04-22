<template>
    <div>
        <!-- 解析html语法 -->
        <div v-html="msg"></div>

        <!-- 原样输出 -->
        {{ msg }}

        <br>

        {{ reverse_msg }}

        <br>

        {{ reverse_msg2() }}

        <h3 v-show="ok">Hello World</h3>

        <!-- 判断 -->
        <h4 v-if="ok">xxx</h4>
        <div v-if="type === 'A'">A</div>
        <div v-else-if="type === 'B'">B</div>
        <div v-else>啥也不是</div>

        <!-- 列表 -->
        <ul>
            <li v-for="(item, index) in tlist" :key="index" :class="{on:index%2==0, off:index%2!==0}">
                {{ item.text }}
            </li>
        </ul>

        <!-- 语法糖 v-on: -> @  v-bind: -> : -->
        <button @click='counter++'>点我加1</button>

        <!-- 购物车 -->
        <table border="1">
            <tr>
                <td>名称</td>
                <td>数量</td>
                <td>价格</td>
            </tr>
            <tr v-for="(item,index) in glist">
				<td>{{ item.name }}</td>
				<td>
					<button @click="calculate(index, '-')">-</button>
					<input type="number" v-model="item.num" >
					<button  @click="calculate(index, '+')" >+</button>
				</td>
				<td>	
					{{ item.price }}
				</td>
			</tr>
        </table>
        总数量：{{ totalCount(0) }}
        <br>
        总价： {{ totalCount(1) }}
    </div>
</template>


<script>
export default {
    data() {
        return {
            msg: '<h1>变量</h1>',
            ok: 0, // 0:false  1:true
            type: 'A',
            tlist: [{text: '家电'}, {text: '生鲜'}, {text: '果蔬'}],
            counter: 0,
            glist: [
                {name: 'iPhone7', num: 1, price: 5099},
                {name: 'iPhone8', num: 1, price: 6099},
                {name: 'iPhonex', num: 1, price: 7099}
            ]
        }
    },

    // 监听属性
    watch: {
        counter: function (nval, oval) {
            console.log('计数器由' + oval + '变成了' + nval)
        }
    },

    // 计算属性
    computed: {
        // 反转
        reverse_msg: function (){
            return this.msg.split('').reverse().join('');
        }
    },

    // 钩子方法
    mounted() {
        
    },

    // 自定义方法
    methods: {
        // 汇总数量
        totalCount: function (type) {
            // 默认数量
            let total = 0;

            // 默认价格
            let totalprice = 0;

            // 遍历
            for(let i=0,l=this.glist.length;i<l;i++){
                // 总数量累加 parseInt：强转成整形
                total += parseInt(this.glist[i].num);
                // 总价累加
                totalprice += (this.glist[i].num * this.glist[i].price);
            }

            if (type == 0) {
                return total;
            } else {
                return totalprice;
            }
            
        },

        // 购物车减法
        minus: function (index) {

            if (this.glist[index].num > 0) {

                this.glist[index].num--;

            }
        },

        // 购物车加法
        add: function (index) {
    
            this.glist[index].num++;

        },

        // 加减法复用
        calculate: function (index, type) {
            if (type == '-') {
                if (this.glist[index].num > 0) {
                    this.glist[index].num--;

            } }else {
                this.glist[index].num++;
            }
        },

        //自定义计算属性
        reverse_msg2: function () {

            return this.msg.split('').reverse().join('');

        }
    },
}
</script>

<style>
    .on {background: rebeccapurple; color: white;}
    .off {background: red; color: black;}
</style>