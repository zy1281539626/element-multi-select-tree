<template>
  <div class="hello">
    <p>已选择data: {{selectData}}</p>
    <el-select 
      v-model="selectData" 
      clearable multiple
      @change="changeSelect"
    >
      <el-option
        v-for="item in selectOptions"
        :key="item.value"
        :label="item.label"
        :value="item.value"
        style="display:none;"
      >
      </el-option>

      <el-tree
        :data="treeData"
        show-checkbox
        node-key="value"
        @check="handleCheckChange"
        ref="treeRef"
      >
      </el-tree>
    </el-select>
  </div>
</template>

<script>
export default {
  name: "HelloWorld",
  data(){
    return {
      selectData: [],
      selectOptions: [{}], // 隐藏的select多选的数据集
      treeData: [ // 树形结构
        {
          label: '一级 1',
          value: '一级 1 value',
          children: [{
            label: '二级 1-1',
            value: '二级 1-1 value',
            children: [
              { label: '三级 1-1-1', value: '三级 1-1-1 value' },
              { label: '三级 1-1-2', value: '三级 1-1-2 value', }
            ]
          }]
        }
      ]
    }
  },
  methods: {
    handleCheckChange(){
      const checkedNodes = this.$refs.treeRef.getCheckedNodes(true)
      this.selectOptions = checkedNodes.length > 0 ? checkedNodes : [{}]
      this.selectData = this.selectOptions.filter(item=>!!item.value).map(item=>item.value)
    },
    changeSelect(data){
      this.$refs.treeRef.setCheckedKeys(data)
    }
  }
};
</script>

<style scoped>

</style>
