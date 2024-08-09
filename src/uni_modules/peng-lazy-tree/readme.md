#### 参数说明

|     属性名      |类型|   默认值   | 说明 |
|:------------:|---|:-------:|--------------------------------------|
|    idKey     |String|   id    | 指定 Object 中 key 的值作为单条数据的唯一id|
|   nameKey    |String|  name   | 指定 Object 中 key 的值作为选择器显示的内容|
|    title     |String|    -    | 选择器的名称默认为空|
|  titleColor  |String| #757575 | 标题的颜色|
| confirmColor |String| #007aff | 确定按钮及选择框的颜色|
| cancelColor  |String| #757575 | 取消按钮的颜色|
| currentIcon  |String|  默认图标   | 折叠时的图标|
| defaultIcon  |String|  默认图标   | 打开之后的图标|
| lastIcon  |String|   默认无   | 没有子集时的图标|
| maskClick  |Boolean|   true   | 点击遮罩层是否关闭|
|   multiple   |Boolean|  true   | 是否可以多选（true:多选、false:单选）|
|   selectParent   |Boolean|  true   | 父级是否可选（true:可选、false:不可选）|
|   cascade   |Boolean|  false  | 在(多选 && 父级可选)模式下，开启级联选择（true:级联、false:不级联） |
| url  |String|   必传   | 接口地址|
| method  |String|   GET   | 接口请求方式|
| header  |Object|   {}   | 接口header传参|
| formData  |Object|   {}   | 接口body传参|
| formUrl  |String|   parentId   | url接口取数据传参，如http://xxx.xxx?parentId=bbb中的parentId|
| default  |String|   空值   | url获取数据默认值，如http://xxx.xxx?parentId=bbb中的bbb|
|    allKey    |String|  id  | 需要查询下一级的取值字段|
| success  |Array|   []   | 接口返回值取数据，只支持解析最多两层数据，解析如下（接口返回值说明）|
| @error  |Handler|    -    | 接口获取数据报错返回信息|
| @cancel  |Handler|    -    | 点击取消或点击遮罩层关闭时的回调方法|
| @confirm  |Handler|    -    | 点击确定按钮时的回调方法|

#### 接口返回值说明
```vue
	//1、接口返回值为
	{
		code:200,
		result：{
			qqq:"qqq",
			bbb:"bbb",
			records:[
				{需要展示的数据001}，
				{需要展示的数据002}，
				{需要展示的数据003}
			]
		}
	}
	success值则需要传["result", "records"]
	//1、接口返回值为
	{
		code:200,
		result：[
			{需要展示的数据001}，
			{需要展示的数据002}，
			{需要展示的数据003}
		]
	}
	success值则需要传["result"]
```

#### 返回值说明
```vue
	treeConfirm(e) {
        console.log("你点击了确定")
        console.log(e)
		/**
		 * e==>
		 * [
		 *	 {id: "100", name: "测试100", parentId: ["1"]}
		 *	 {id: "110", name: "测试110", parentId: ["1","2"]}
		 *	 {id: "111", name: "测试111", parentId: ["1","2","3"]}
		 * ]
		 * e为选中的数据数组
		 * e中的对象中有id(idKey指定的字段值)、name(nameKey指定的字段)、parentId(处自己外所有上级的id数组)
		 */
    },

```

#### 代码使用示例
```vue
<template>
	<view class="content">
		<button @click="dakai">打开</button>
		<peng-lazy-tree ref="pengLazyPeng" 
			title="选择器" 
			titleColor="red"
			cancelColor="#757575"
			confirmColor="#007aff"
			:maskClick="true"
			:selectParent="true"
			:multiple="true"
			
			url="https://admin.qingshanzhi.cn/cc-admin/qsz/speciesType/list"
			formUrl="parentId" 
			default="0"
			:header="header"
			:formData="formData"
			method="GET"
			:success="success"
			
			idKey="id"
			nameKey="name"
			allKey="id"
			
			@error="error"
			@confirm="confirm"
			@cancel="cancel" />
	</view>
</template>

<script>
	export default {
		data() {
			return {
				formData:{
					name:"",
					valuel:""
				},
				header: {
					Authorization: uni.getStorageSync('token'),
				},
				success: ["result", "records"]
			}
		},
		methods: {
			//打开选择器
			dakai() {
				this.$refs.pengLazyPeng._show()
			},
			//接口报错返回
			error(e) {
				console.log(e)
			},
			//点击确定返回
			confirm(e) {
				console.log(e)
			},
			//点击取消时间
			cancel(e) {
				console.log(e)
			}
		}
	}
</script>


```
