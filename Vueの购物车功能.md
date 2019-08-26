# Vueの购物车功能

--实现了增减功能、删除功能、单选功能、全选/取消功能、 总价结算功能

1.数据绑定

```.vue
html：
<tr v-for="(item,index) in productList">
<td>
<input type="checkBox"
v-bind:checked="item.isSelect"     				      @click="item.isSelect=!item.isSelect"/>
</td>
<td>{{item.proName}}</td>
<td>
<span>
<a href="#" @click="item.proNum>0?item.proNum--:''">-</a>
</span>
<strong>{{item.proNum}}</strong>
<span><a href="#" @click="item.proNum++">+</a></span>
</td>
<td>{{item.proPrice}}</td>
<td>{{item.proPrice*item.proNum}}</td>
<td><a href="#" @click="deletePro(index)">删除</a></td>
</tr>
 
js：
data : {
		productList:[
				{
					'proName' :'剃须刀',
					'proNum' : 2,
					'proPrice' :1000,
				},
				{
					'proName' :'小米耳机',
					'proNum' : 10,
					'proPrice' :100,
				},
				{
					'proName' :'小米鼠标',
					'proNum' : 5,
					'proPrice' :100,
				}
			],
		},
 ———————————————— 

```

2.实现+和-数量功能

操作proNum的加和减。

3.删除功能

遍历data集合上下标index

```vue
deletePro : function(index){
				alert("你正在删除"+this.productList[index].proName);
				this.productList.splice(index,1);
			},

 ———————————————— 

```

4.设置选中功能

```vue

mounted:function () {
        var _this=this;
        //为productList添加select（是否选中）字段，初始值为true
        this.productList.map(function (item) {
            _this.$set(item, 'isSelect', true);
        })
    	}

 ———————————————— 
为productList挂载isSelect属性。
```

5.总商品数量和总价格

```

getTotal:function(){
				var prolist = this.productList.filter(function (val) { return val.isSelect}),
				totalPri = 0;
				for (var i = 0,len = prolist.length; i < len; i++) {
					totalPri+=prolist[i].proPrice*prolist[i].proNum;
				}
				return {totalNum:prolist.length,totalPrice:totalPri}
			},

 ———————————————— 
遍历商品中选中的商品进行计算总数量和价格
```

6.删除所选产品

```
deleteProduct:function () {
                this.productList=this.productList.filter(function (item) {return !item.isSelect})
            },

```

7.全选功能

```vue
isSelectAll:function(){
                //如果productList中每一条数据的isSelect都为true，返回true，否则返回false;
                return this.productList.every(function (val) { return val.isSelect});
            },

 ———————————————— 
selectProduct:function(_isSelect){
                //遍历productList，全部取反
                for (var i = 0, len = this.productList.length; i < len; i++) {
                    this.productList[i].isSelect = !_isSelect;
                }
            },

 ———————————————— 

```

```vue

<!DOCTYPE html>
<html lang="en">
<head>
	<meta charset="UTF-8">
    <title>Title</title>
</head>
<style type="text/css">
table{width: 1200px;}
	table th{width: 100px}
	table td{width: 200px;text-align: center;}
	a{text-decoration:none;color: black}
	span{font-size: 20px;margin: 10px 10px}
	strong{border: 1px solid;}
.checkPro{
	background-color: gray;
}
.leftConent {
	float: left;
}
.rightConent{
	float: right;
}
</style>
<script src="vue.min.js"></script>
<body>
	<div id="test">
		<table id="mytable">
			<tr>
				<th><input type="checkBox"  @click="selectProduct(isSelectAll)" v-bind:checked="isSelectAll" />全选</th>
				<th>商品</th>
				<th>数量</th>
				<th>单价(元)</th>
				<th>金额(元)</th>
				<th>操作</th>
			</tr>
			<tr v-for="(item,index) in productList">
				<td><input type="checkBox"   v-bind:checked="item.isSelect" @click="item.isSelect=!item.isSelect"/></td>
				<td>{{item.proName}}</td>
				<td><span><a href="#" @click="item.proNum>0?item.proNum--:''">-</a></span><strong>{{item.proNum}}</strong><span><a href="#" @click="item.proNum++">+</a></span></td>
				<td>{{item.proPrice}}</td>
				<td>{{item.proPrice*item.proNum}}</td>
				<td><a href="#" @click="deletePro(index)">删除</a></td>
			</tr>
		</table>
		<div class="checkPro">
			<div class="leftConent">
				<span><a href="#" @click="deleteProduct">删除所选产品</a></span>
			</div>
			<div class="rightConent">
				<span>{{getTotal.totalNum}}件商品总计：￥{{getTotal.totalPrice}}</span>
			</div>
			
		</div>
	</div>
</body>
<script >
	new Vue({
		el : "#test",
		data : {
			productList:[
				{
					'proName' :'剃须刀',
					'proNum' : 2,
					'proPrice' :1000,
				},
				{
					'proName' :'小米耳机',
					'proNum' : 10,
					'proPrice' :100,
				},
				{
					'proName' :'小米鼠标',
					'proNum' : 5,
					'proPrice' :100,
				}
			],
		},
		methods:{
			selectProduct:function(_isSelect){
                //遍历productList，全部取反
                for (var i = 0, len = this.productList.length; i < len; i++) {
                    this.productList[i].isSelect = !_isSelect;
                }
            },
			deletePro : function(index){
				alert("你正在删除"+this.productList[index].proName);
				this.productList.splice(index,1);
			},
			//删除已经选中(isSelect=true)的产品
            deleteProduct:function () {
                this.productList=this.productList.filter(function (item) {return !item.isSelect})
            },
		},
		computed:{
			//检测是否全选
            isSelectAll:function(){
                //如果productList中每一条数据的isSelect都为true，返回true，否则返回false;
                return this.productList.every(function (val) { return val.isSelect});
            },
			getTotal:function(){
				var prolist = this.productList.filter(function (val) { return val.isSelect}),
				totalPri = 0;
				for (var i = 0,len = prolist.length; i < len; i++) {
					totalPri+=prolist[i].proPrice*prolist[i].proNum;
				}
				return {totalNum:prolist.length,totalPrice:totalPri}
			},
		},
		mounted:function () {
        var _this=this;
        //为productList添加select（是否选中）字段，初始值为true
        this.productList.map(function (item) {
            _this.$set(item, 'isSelect', true);
        })
    	}
	})
</script>
</html>
 
 ———————————————— 

```

