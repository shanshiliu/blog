---
title: element-ui组件el-cascader懒加载回显问题
date: 2021-12-30 13:46:00
tags: element-ui、组件、el-cascader
---

###  使用element-ui组件的级联选择器时，业务中用到了懒加载的方式，但是编辑时的回显就出了问题，官方文档也没有好给出示例。
* 实现回显的思路：
如果级联选择器el-cascader当前有绑定的数据，则多请求一个层级的数据，如果没有则自动执行懒加载
两种情况：
    1、如果绑定数据需要异步获取 采用v-if方案即可
    2、如果绑定数据是 初始化定义或 props传入数据 手动执行懒加载节点数据后、绑定到children属性中 （ps：下面代码实现中注释的代码）
* 代码实现：
* 
	 ```html
		<el-cascader v-if="showCascader" :props="props" v-model="cascaderList"></el-cascader>
		
		<script>
		let id = 0
		export default {
		  name: 'Documentation',
		  components: {  },
		  data() {
		    return {
		      cascaderList: [],
		      showCascader: false,
		      props: {
		        lazy: true,
		        lazyLoad: this.lazyLoad
		      }
		    }
		  },
		  mounted(){
		    this.showCascader = false
		    setTimeout(() => {
		      this.cascaderList = [1,4]
		      this.showCascader = true
		    },200)
		  },
		  methods:{
		    async lazyLoad (node, resolve) {
		      const { level } = node;
		      console.log(this.cascaderList)
		    	// 在懒加载里做判断
		    	let list = []
		    	if ( /* this.cascaderList.length && level == 0 && */ false) {
		
		        // 如果是2个层级的数据 较简单只需多调用一次
		    	// const currentValue = this.cascaderList[level]
		      //   list = await this.handleData(level)
		      //   let currentIndex
		      //   list.forEach((item, i) => {
		      //   	if (item.value == this.cascaderList[level]) {
		      //   		currentIndex = i
		      //   	}
		      //   })
		      //   list[currentIndex].childern =  await this.handleData(level+1)
		      //   resolve(list)
		      // 如果多层数据 使用2次for循环实现 一次请求数据 一次添加到children属性
		        /* const dataList = []
		        for (var i = 0; i < this.cascaderList.length; i++) {
		          dataList.push(await this.handleData(i))
		        }
		        console.log(dataList)
		        for (var i = dataList.length-1; i >= 0; i--) {
		          let dataIndex
		          dataList[i].forEach((item, index) => {
		            console.log(item.value)
		            if (this.cascaderList[i] == item.value) {
		              dataIndex = index
		            }
		          })
		          console.log(dataIndex)
		          if (dataIndex) {
		            dataList[i][dataIndex].children = dataList[i+1]
		          }
		        }
		        console.log(dataList[0], 999)
		        resolve(dataList[0]) */

		    	} else {
		    		this.handleData(level).then(nodes => {
					  resolve(nodes)
					})
		    	}
		    },
		    handleData(level) {
		    	// 模拟获取数据
		    	let nodes = []
		    	return new Promise((resolve, reject) => {
		        setTimeout(() => {
		          nodes = Array.from({ length: level + 2 })
		            .map((item, i) => ({
		              value: ++id,
		              label: `选项${id}`,
		              leaf: level >= 1
		            }));
		          resolve(nodes)
		          // 通过调用resolve将子节点数据返回，通知组件数据加载完成
		        }, 1000);
		      })
		      // console.log(nodes)
		    }
		  }
		}
		</script>
	 ```
	

 
 



