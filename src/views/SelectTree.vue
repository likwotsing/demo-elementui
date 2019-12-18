<template>
  <div class="st">
    <el-select class="select" v-model="value1" multiple placeholder="请选择" :popper-append-to-body="false" @visible-change="handleVisibleChange" @blur="handleBlur" @focus="handleFocus">
      <el-option value="1" disabled="">提示：以下可以多选</el-option>
      <el-tree
        class="tree"
        :class="{'active': isActive}"
        ref="tree"
        :data="data"
        node-key="id"
        :props="defaultProps"
        show-checkbox
        @node-click="handleNodeClick"
        @check="handleCheck"
      ></el-tree>
    </el-select>
    <div class="tag-wrap">
      <el-tag
        size="mini"
        v-for="tag in selectLeafNodes"
        :key="tag.label"
        closable
        :disable-transitions="false"
        @close="handleTagClose(tag)">
        {{tag.label}}
      </el-tag>
    </div>
  </div>
</template>

<script>
export default {
  name: 'SelectTree',
  data () {
    return {
      value1: [],
      isActive: false,
      checkedNodes: [],
      selectLeafNodes: [],
      data: [{
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
      }],
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
    handleBlur (a) {
    },
    handleFocus (a) {
    },
    handleNodeClick (data) {
    },
    handleCheck (checkedNodes, checkedKeys, halfCheckedNodes, halfCheckedKeys) {
      this.checkedNodes.push(checkedNodes)
      this.selectLeafNodes = this.$refs.tree.getCheckedNodes().filter((c, i, a) => c.children ? '' : c)
    }
  }
}
</script>

<style lang="stylus" scoped>
.st
  position: relative;
  width: 200px;
  margin: 50px auto;
  .select
    display: block;
  .tree
    display: none;
  .active
    display: block;
  .tag-wrap
    position: absolute;
    top: 10px;
    left: 6px;
    max-width: 100%;
    >>> .el-tag
      margin-right: 5px;
</style>
