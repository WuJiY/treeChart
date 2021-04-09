<template>
  <div id="app">
    <tree-chart :model="tree" @on-add="add" @on-update="update" @on-remove="remove"></tree-chart>
  </div>
</template>
<script>
  import TreeChart from '@/components/TreeChart';

  /**
   * 使用方式
   * <TreeChart :model="data" >
   * data 参数格式为树形结构
   *
   * {
   *  name: "xxx",
   *  x: "xxx",
   *  y: "xxx",
   *  children: [{
   *    name: "xxx",
   *    x: "xxx",
   *    y: "xxx",
   *    children: []
   *  }]
   * }
   */


  const dataTree = {
    'id': "root",
    'name': "根节点",
    'x': "xxx",
    'y': "xxx",
    'children': [{
      'id': "1",
      'name': "Card",
      'x': "xxx",
      'y': "xxx",
      'children': []
    }]
  }
  export default {
    data() {
      return {
        tree: dataTree
      }
    },
    components: {
      TreeChart
    },
    methods: {
      // 添加
      add: function (node) {
        const newNode = {
          name: 'Initial value',
          id: new Date().getTime(),
          x: '',
          y: '',
          children: []
        }
        node.children.push(newNode)
      },
      // 修改
      update: function (node) {

      },
      // 删除
      remove: function (node) {
        if (node.id == 'root') {
          this.$message('跟节点不能删除')
          return false
        }
        const deepSearch = (tree) => {
          for (let i = tree.length - 1; i >= 0; i--) {
            if (tree[i].id == node.id) {
              tree.splice(i, 1)
            } else if (tree[i].children) {
              deepSearch(tree[i].children)
            }
          }
        }
        deepSearch(this.tree.children)
      }
    },
  }
</script>

<style lang="scss">
  * {
    padding: 0;
    margin: 0;

    #app {
      height: 100vh;
      width: 100vw;
    }
  }
</style>
