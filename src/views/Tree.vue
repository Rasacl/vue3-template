<template>
  <div class="node-list">
    <el-tree
        :props="defaultProps"
        id="tree"
        style="margin-top:20px"
        ref="tree"
        :data="data"
        class="tree-line content"
        :indent="0"
        :filter-node-method="filterNode"
    >
      <template #default="{node, data}">
        <div class="custom-tree-node">
          <span>
          <i
              v-if="data.children && data.children.length!==0"
              :class="[
            node.expanded ? 'el-icon-folder-remove': 'el-icon-folder-add'
          ]"
          ></i>
          {{ node.label }}
        </span>
        </div>
      </template>
    </el-tree>
  </div>
</template>

<script setup>
import { ref, watch } from 'vue'


const filterText = ref('')
const treeRef = ref()

const defaultProps = {
  children: 'children',
  label: 'label',
}

watch(filterText, (val) => {
  treeRef.value.filter(val)
})

const filterNode = (value, data) => {
  if (!value) return true
  return data.label.includes(value)
}

const data = [
  {
    id: 1,
    label: 'Level one 1',
    children: [
      {
        id: 4,
        label: 'Level two 1-1',
        children: [
          {
            id: 9,
            label: 'Level three 1-1-1',
          },
          {
            id: 10,
            label: 'Level three 1-1-2',
          },
        ],
      },
    ],
  },
  {
    id: 2,
    label: 'Level one 2',
    children: [
      {
        id: 5,
        label: 'Level two 2-1',
      },
      {
        id: 6,
        label: 'Level two 2-2',
      },
    ],
  },
  {
    id: 3,
    label: 'Level one 3',
    children: [
      {
        id: 7,
        label: 'Level two 3-1',
      },
      {
        id: 8,
        label: 'Level two 3-2',
      },
    ],
  },
]
</script>


<style scoped lang="less">
.node-list{
  width: 500px;
  height: 1000px;
  padding: 10px;
  box-sizing: border-box;

  :deep(.tree-line){
    padding: 10px;
    .el-tree-node {
      position: relative;
      padding-left: 16px; // 缩进量
    }
    .el-tree-node__children {
      padding-left: 16px; // 缩进量
      transition: all 0.5s; // 关键动画！！！
    }

    // 竖线
    .el-tree-node::before {
      content: "";
      height: 100%;
      width: 1px;
      position: absolute;
      left: -3px;
      top: -26px;
      border-width: 1px;
      border-left: 1px solid #E9E9E9;
    }
    // 当前层最后一个节点的竖线高度固定
    .el-tree-node:last-child::before {
      height: 48px; // 可以自己调节到合适数值
    }

    // 横线
    .el-tree-node::after {
      content: "";
      width: 18px;
      height: 20px;
      position: absolute;
      left: -3px;
      top: 20px;
      border-width: 1px;
      border-top: 1px solid #E9E9E9;
    }

    // 去掉最顶层的虚线，放最下面样式才不会被上面的覆盖了
    & > .el-tree-node::after {
      border-top: none;
    }
    & > .el-tree-node::before {
      border-left: none;
    }

    // 展开关闭的icon
    .el-tree-node__expand-icon{
      font-size: 16px;
      display: none;// 也可以去掉
      // 叶子节点（无子节点）
      &.is-leaf{
        color: transparent;
        display: none; // 也可以去掉
      }
    }
    .el-tree-node__content{
      height: 40px;
      .custom-tree-node {
        flex: 1;
        display: flex;
        align-items: center;
        justify-content: space-between;
        font-size: 14px;
        /* padding-right: 8px;  // 按钮右边的缩进*/
        .custom-tree-node-button{ // 宽度62左右目前
          display: none;
        }
      }
      .custom-tree-node:hover .custom-tree-node-button{
        display:block;
        background-color: rgb(245,247,250);
        padding: 0 10px;
        position:absolute;
        right:0px;
        z-index: 666;
      }
      .custom-tree-node i {
        margin-left: 8px;
      }
      .nodeLabel{
        width: 100px;
        margin-left: 10px;
      }
      .nodeLabel-isChoose{
        width: 100px;
        margin-left: 10px;
        color: #7FB545;
      }
      .dialog-title{
        color: #222222;
        font-weight: 500;
        font-size: 20px;
        display: flex;
        align-items: center;
        span{
          margin-left: 10px;
        }
      }
      .dialog-content{
        display: flex;
        align-items: center;
        justify-content: center;
        height: 20vh;
        font-size: 24px;
      }

    }
  }
  .content{
    padding-right: 7px;
    overflow: hidden;
    overflow-y: scroll;
    height: calc(100vh - 300px);
  }
  .content::-webkit-scrollbar {
    // display: none;
    width: 2px;
    height: 1px;
  }
  .content::-webkit-scrollbar-thumb {
    /*滚动条里面小方块*/
    border-radius: 10px;
    box-shadow   : inset 0 0 5px rgba(0, 0, 0, 0.2);
    background   : rgba(127, 181, 69, 1);
  }
  .content::-webkit-scrollbar-track {
    /*滚动条里面轨道*/
    // box-shadow   : inset 0 0 5px rgba(0, 0, 0, 0.2);
    border-radius: 10px;
    background   : transparent;
  }
}
.el-icon-folder-remove{
  display:inline-block;
  width: 15px;
  height: 15px;
  background-image: url("../assets/reduce.png");
  background-repeat: no-repeat;
  background-size: 100% 100%;
  position: absolute;
  left: 1px;
  top:0
}
.el-icon-folder-add{
  display:inline-block;
  width: 15px;
  height: 15px;
  background-image: url("../assets/add.png");
  background-repeat: no-repeat;
  background-size: 100% 100%;
  position: absolute;
  left: 1px;
  top:0
}
</style>
