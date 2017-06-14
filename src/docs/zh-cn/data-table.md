<script>
    import axios from 'axios'
    export default {
        data: function () {
            return {
                loadData(pramas) {
                    return axios.get("static/static/datatable.json",pramas).then(res =>{
                        return res.data;
                    });
                },
                simpleColumns:[
                    {title:"姓名",field:'name'},
                    {title:"性别",field:'sex'},
                    {title:"编号",field:'id'}
                ],
                columns:[
                    {title:"姓名",field:'name'},
                    {title:"性别",field:'sex'},
                    {title:"姓名",field:'name'},
                    {title:"姓名",field:'name',sort:true,width:"200px"},
                    {title:"姓名",field:'name'},
                    {title:"姓名",field:'name',sort:true},
                    {title:"姓名",field:'name',sort:true},
                    {title:"姓名",field:'name',sort:true},
                    {title:"姓名",field:'name',sort:true},
                    {title:"姓名",field:'name',sort:true},
                    {title:"姓名",field:'name',sort:true},
                    {title:"姓名",field:'name',sort:true},
                    {title:"id",field:'id',className:"test dd"},
                    {title:"姓名",field:'name',sort:true},
                    {title:"姓名",field:'name',sort:true},
                    {title:"id",field:'id',className:"test dd"},
                    {title:"姓名",field:'name',sort:true},
                    {title:"id",field:'id',className:"test dd"}
                ],
                pageno:1,
                checkAllMsg:null,
                clickRowMsg:null
            }
        },
        methods:{
            checkAll:function(value){
                this.checkAllMsg = "当前全选状态是："+value
            },
            clickRow:function(obj){
                console.log(obj);
                this.clickRowMsg = "当前点击第"+obj.index+"行";
            },
            refreshTable:function(){
                this.$refs.xtable.refresh();
            },
            reloadTable:function(){
                this.$refs.xtable.reload();
            },
            go2:function(){
                this.$refs.xtable.goto(2);
            }
        }
    }
</script>

# Datatable 数据表格

数据表格，用于展现大量结构化数据。

## 何时使用
- 当有大量结构化的数据需要展现时；
- 当需要对数据进行排序、分页、自定义操作等复杂行为时。

## 代码演示

### 数据-datatable.json

````javascript
    {
        "result":[
            {"name":"杰克","sex":"男","id":1},
            {"name":"露丝","sex":"女","id":2},
            {"name":"杰瑞","sex":"男","id":3},
            {"name":"苏可","sex":"男","id":4},
            {"name":"玛丽","sex":"女","id":5},
            {"name":"杰西卡","sex":"女","id":6},
            {"name":"贝利","sex":"男","id":7},
            {"name":"路易斯","sex":"男","id":8},
            {"name":"艾伦","sex":"男","id":9},
            {"name":"三笠","sex":"女","id":10}
        ],
        "totalCount":11,
        "pageSize":10,
        "pageNo":1
    }
````

::: demo
<summary>
  #### 基本
  最基本用法，配置好data和columns即可。
</summary>

```html
<v-data-table :data='loadData' :columns='simpleColumns'></v-data-table>

<script>
    import axios from 'axios'
    export default {
        data: function () {
            return {
                loadData(pramas) {
                    return axios.get("static/static/datatable.json",pramas).then(res =>{
                        return res.data;
                    });
                },
                columns:[
                    {title:"姓名",field:'name'},
                    {title:"性别",field:'sex'},
                    {title:"编号",field:'id'}
                ]
            }
        },
        methods:{
            
        }
    }
</script>
```
:::

::: demo
<summary>
  #### 边框和斑马线
  边框表格和斑马线效果
</summary>

```html
<v-data-table :data='loadData' :columns='simpleColumns' stripe bordered></v-data-table>

<script>
    import axios from 'axios'
    export default {
        data: function () {
            return {
                loadData(pramas) {
                    return axios.get("static/static/datatable.json",pramas).then(res =>{
                        return res.data;
                    });
                },
                columns:[
                    {title:"姓名",field:'name'},
                    {title:"性别",field:'sex'},
                    {title:"编号",field:'id'}
                ]
            }
        },
        methods:{
            
        }
    }
</script>
```
:::

::: demo
<summary>
  #### 显示html
  在表头或表格中显示普通html内容。命名为th的slot表示表头作用域插槽，命名为td的slot表示单元格的作用域插槽。
</summary>

```html
<v-data-table :data='loadData' :columns='columns' bordered>
    <template slot="th" scope="props">
        <strong v-html="props.title"></strong>
    </template>
    <template slot="td" scope="props">
        <span v-html="props.content"></span>
    </template>     
</v-data-table>
```
:::

::: demo
<summary>
  #### 特定列使用组件
  在表头或表格中显示普通html内容。命名为th的slot表示表头作用域插槽，命名为td的slot表示单元格的作用域插槽。
</summary>

```html
<v-data-table :data='loadData' :columns='columns' size="middle">
    <template slot="th" scope="props">
        <strong v-if="props.cindex==0">操作操作按钮</strong>
        <strong v-else v-html="props.title"></strong>
    </template>
    <template slot="td" scope="props">
        <v-button-group v-if="props.cindex==0" size="small">
            <v-button type="primary" icon="cloud"></v-button>
            <v-button type="primary" icon="cloud-download"></v-button>
        </v-button-group>
        <span v-else v-html="props.content"></span>
    </template>     
</v-data-table>
```
:::

::: demo
<summary>
  #### 固定高度
  设置height属性即可。
</summary>

```html
<v-data-table :data='loadData' :columns='columns' :height="300" bordered>
      
</v-data-table>
```
:::

::: demo
<summary>
  #### 自适应高度
  设置bottomGap，表示表格底部距离viewport底部的间距，进而实现表格自适应高度。为了保证表格数据可见，自适应计算的最小高度是200px。
  注意：bottomGap仅对第一屏显示的table有效果；height属性和bottomGap同时使用时，height属性优先。
</summary>

```html
<v-data-table :data='loadData' :columns='columns' :bottom-gap="30">
    
</v-data-table>
```
:::

::: demo
<summary>
  #### 禁用分页
  不显示分页组件
</summary>

```html
<v-data-table :data='loadData' :columns='columns' :pagination="false">
     
</v-data-table>
```
:::

::: demo
<summary>
  #### 行选择和点击事件
  选择行的事件演示；行点击的事件演示
</summary>

```html

<v-alert v-if="checkAllMsg" :message="checkAllMsg"></v-alert>
<v-alert v-if="clickRowMsg" :message="clickRowMsg"></v-alert>

<v-data-table :data='loadData' :columns='columns' check-type="checkbox" @checkall="checkAll" @clickrow="clickRow">
      
</v-data-table>

<script>
    import axios from 'axios'
    export default {
        data: function () {
            return {
                loadData(pramas) {
                    return axios.get("static/static/datatable.json",pramas).then(res =>{
                        return res.data;
                    });
                },
                columns:[
                    {title:"姓名",field:'name'},
                    {title:"性别",field:'sex'},
                    {title:"姓名",field:'name'},
                    {title:"姓名",field:'name',sort:true,width:"200px"},
                    {title:"姓名",field:'name'},
                    {title:"姓名",field:'name',sort:true},
                    {title:"姓名",field:'name',sort:true},
                    {title:"姓名",field:'name',sort:true},
                    {title:"姓名",field:'name',sort:true},
                    {title:"姓名",field:'name',sort:true},
                    {title:"姓名",field:'name',sort:true},
                    {title:"姓名",field:'name',sort:true},
                    {title:"id",field:'id',className:"test dd"},
                    {title:"姓名",field:'name',sort:true},
                    {title:"姓名",field:'name',sort:true},
                    {title:"id",field:'id',className:"test dd"},
                    {title:"姓名",field:'name',sort:true},
                    {title:"id",field:'id',className:"test dd"}
                ],
                checkAllMsg:null,
                checkRowMsg:null,
            }
        },
        methods:{
            checkAll:function(value){
                this.checkAllMsg = "当前全选状态是："+value
            },
            clickRow:function(obj){
                console.log(obj);
                this.clickRowMsg = "当前点击第"+obj.index+"行";
            }
        }
    }
</script>
```
:::

::: demo
<summary>
  #### 无数据时提示文案
  无数据时显示的提示文案，可根据需要定制，使用名为emptytext的slot。
</summary>

```html

<v-data-table :data='loadData' :columns='columns'>
    <template slot="emptytext" scope="props">
        <v-tag color="orange">我去，这是几个意思？</v-tag>
    </template>    
</v-data-table>
```
:::

::: demo
<summary>
  #### 调用API方法
  可通过在datatable实例上调用API方法实现某些特定操作，如刷新数据。
</summary>

```html

<v-data-table ref="xtable" :data='loadData' :columns='columns' :page-num="pageno">
      
</v-data-table>
<br>
<v-button @click="refreshTable">刷新表格</v-button>
<v-button @click="reloadTable">重载表格</v-button>
<v-button @click="go2">跳转到第二页</v-button>
<script>
    import axios from 'axios'
    export default {
        data: function () {
            return {
                loadData(pramas) {
                    return axios.get("static/static/datatable.json",pramas).then(res =>{
                        return res.data;
                    });
                },
                columns:[
                    {title:"姓名",field:'name'},
                    {title:"性别",field:'sex'},
                    {title:"姓名",field:'name'},
                    {title:"姓名",field:'name',sort:true,width:"200px"},
                    {title:"姓名",field:'name'},
                    {title:"姓名",field:'name',sort:true},
                    {title:"姓名",field:'name',sort:true},
                    {title:"姓名",field:'name',sort:true},
                    {title:"姓名",field:'name',sort:true},
                    {title:"姓名",field:'name',sort:true},
                    {title:"姓名",field:'name',sort:true},
                    {title:"姓名",field:'name',sort:true},
                    {title:"id",field:'id',className:"test dd"},
                    {title:"姓名",field:'name',sort:true},
                    {title:"姓名",field:'name',sort:true},
                    {title:"id",field:'id',className:"test dd"},
                    {title:"姓名",field:'name',sort:true},
                    {title:"id",field:'id',className:"test dd"}
                ],
                pageno:1
            }
        },
        methods:{
            refreshTable:function(){
                this.$refs.xtable.refresh();
            },
            reloadTable:function(){
                this.$refs.xtable.reload();
            },
            go2:function(){
                this.$refs.xtable.goto(2);
            }
        }
    }
</script>

```
:::


## API

### Datatable Props
| 参数      | 说明          | 类型      | 默认值  |
|---------- |-------------- |---------- |-------- |
| data | 获取表格数据的函数，返回值必须是Promise对象,该函数默认接收一个请求参数，参数构造请见data arguments | Function | - |
| bordered | 是否展示外边框和列边框 | Boolean | true |
| stripe | 是否显示间隔斑马纹 | Boolean | false |
| size | 尺寸，`large`、`middle`、`small` | String | large |
| columns | 表头配置，类型为对象数据，具体请见下表：Columns Object | Array | - |
| pagination | 是否启用分页 | Boolean | false |
| pageSize | 分页大小 | Number | 10 |
| pageNum | 当前页数 | Number | 1 |
| pageSizeOptions | 分页大小可选参数 | Array | [10, 20, 30, 40, 50] |
| checkType | 选择框类型 | 'checkbox'或'radio' | - |
| height | 表格高度，注意是指表格整体高度（包含表头、表格和底部分页） | Number | - |
| bottomGap | 距离viewport底部的间隙距离 | Number | - |
| responseParamsName | 接口数据的关键字段命名，目前支持total和results,分别表示总数字段和结果字段 | Object | {total:'totalCount',results: 'result'} |

### data arguments
| 参数      | 说明          | 类型      | 默认值  |
|---------- |-------------- |---------- |-------- |
| pageNo | 页数 | Number | - |
| pageSize | 分页大小 | Number | 10 |
| sortColumns | 排序列 | 'field order field order' | false |

### Columns Object
| 参数      | 说明          | 类型      | 默认值  |
|---------- |-------------- |---------- |-------- |
| title | 列名 | String | - |
| field | 字段名（对应data中的字段名） | String | - |
| sort | 是否排序 | Boolean | false |
| width | 列宽 | 合法的CSS尺寸,如120px或5% | - |
| className | 自定义类名 | String | - |

### th Slot Props
| 参数      | 说明          | 类型      | 默认值  |
|---------- |-------------- |---------- |-------- |
| cindex | 当前列索引 | Number | - |
| column | 当前列对象 | Object | - |
| title | 当前列标题 | String | - |

### td Slot Props
| 参数      | 说明          | 类型      | 默认值  |
|---------- |-------------- |---------- |-------- |
| index | 当前行索引 | Number | - |
| cindex | 当前列索引 | Number | - |
| column | 当前列对象 | Object | - |
| content | 当前单元格内容 | String | - |
| item | 当前行内容对象 | Object | - |

### Datatable Events
| 事件        | 说明           | 参数        | 参数说明        |
|------------|----------------|------------|------------|
| checkall    | 全选时触发 | value | 是否选中，布尔值 |
| clickrow    | 点击某一行时触发 | object | {index:选中行的索引,checked:是否选中,row:行数据} |

### API Methods
| method        | 说明           | 参数        | 参数说明        |
|------------|----------------|------------|------------|
| refresh    | 刷新表格数据（使用datatable的当前参数） | - | - |
| reload    | 重新加载数据（重置到第一页） | - | - |
| goto    | 跳转页数 | pageNumber | 整数 |

