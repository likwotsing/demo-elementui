<template>
  <div class="st">
    <el-select
      class="select"
      v-model="value1"
      multiple
      placeholder="请选择"
      :popper-append-to-body="false"
      @visible-change="handleVisibleChange"
    >
      <el-tree
        class="tree"
        :class="{'active': isActive}"
        ref="tree"
        :data="data"
        node-key="id"
        :props="defaultProps"
        show-checkbox
        @check="handleCheck"
      ></el-tree>
      <el-option :value="Math.random()" disabled="">请选择</el-option>
      <el-option
        v-for="item of selectLeafNodes"
        :key="item.id"
        :value="item.id"
      >提示：以下可以多选</el-option>
    </el-select>
  </div>
</template>

<script>
export default {
  name: 'SelectTree',
  computed: {
    value1: {
      get: function () {
        // 设置tag
        return this.selectLeafNodes.map((c, i, a) => {
          return c.label
        })
      },
      set: function (val) {
        // 设置tree
        // 从选中的node里筛选出全部的val，用筛选的结果设置tree
        var flag = false
        // 删除点击的tag
        // selectLeafNodes需要改变, value1会跟着变化
        this.selectLeafNodes = this.selectLeafNodes.filter((c) => {
          flag = false
          for (var i = 0; i < val.length; i++) {
            if (c.label === val[i]) {
              flag = true
              break
            }
          }
          if (flag) {
            return c
          }
        })
        this.$refs.tree.setCheckedNodes(this.selectLeafNodes)
      }
    }
  },
  data () {
    return {
      isActive: false,
      selectLeafNodes: [],
      data: [
        {
          id: 1,
          label: '一级 1',
          children: [{
            id: 4,
            label: '二级 1-1',
            children: [{
              id: 9,
              label: '三级 1-1-1'
            }, {
              id: 10,
              label: '三级 1-1-2'
            }]
          }]
        }, {
          id: 2,
          label: '一级 2',
          children: [{
            id: 5,
            label: '二级 2-1'
          }, {
            id: 6,
            label: '二级 2-2'
          }]
        }, {
          id: 3,
          label: '一级 3',
          children: [{
            id: 7,
            label: '二级 3-1'
          }, {
            id: 8,
            label: '二级 3-2'
          }]
        }
      ],
      defaultProps: {
        children: 'children',
        label: 'label'
      }
    }
  },
  methods: {
    handleTagClose (tag) {
      this.selectLeafNodes.splice(this.selectLeafNodes.indexOf(tag), 1)
      this.$refs.tree.setCheckedNodes(this.selectLeafNodes)
    },
    handleVisibleChange (val) {
      this.isActive = val
    },
    handleCheck (checkedNodes, checkedKeys, halfCheckedNodes, halfCheckedKeys) {
      this.selectLeafNodes = this.$refs.tree.getCheckedNodes().filter((c, i, a) => c.children ? '' : c)
    }
  }
}
</script>

<style lang="stylus" scoped>
.st
  position: relative;
  width: 300px;
  margin: 50px auto;
  .select
    display: block;
    >>> .el-select-dropdown__item
      display: none;
  .tree
    display: none;
  .active
    display: block;
</style>
