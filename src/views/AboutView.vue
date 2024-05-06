<template>
  <div class="node-list">
    <el-input
        v-model="filterText"
        style="width: 240px"
        placeholder="Filter keyword"
    />

    <el-tree
        ref="treeRef"
        style="max-width: 600px"
        class="tree-line"
        :data="data"
        :indent="0"
        :props="defaultProps"
        default-expand-all
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
        children: [
          {
            id: 99,
            label: 'Level two 2-2-1',
          },
          {
            id: 100,
            label: 'Level two 2-2-2',
          },
        ]
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
        children: [
          {
            id: 23,
            label: 'Level two 3-1-1',
          },
          {
            id: 34,
            label: 'Level two 3-1-2',
          },
        ]
      },
      {
        id: 8,
        label: 'Level two 3-2',
        children: [
          {
            id: 29,
            label: 'Level two 3-2-1',
          },
          {
            id: 39,
            label: 'Level two 3-2-2',
          },
        ]
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
  backdrop-filter: blur(130px);
  background-color: rgba(58, 55, 56, 0.3);
  :deep(.el-tree){
    background: transparent;
    margin-top: 40px;
    .el-tree-node{
      position: relative;
      padding-left: 16px; // 缩进量
      &::before {
        content: "";
        height: 100%;
        width: 1px;
        position: absolute;
        left: 28px;
        top: -4px;
        border-width: 1px;
        border-left: 1px solid #52627C;
      }
      &::after {
        content: "";
        width: 10px;
        height: 20px;
        position: absolute;
        left: 29px;
        top: 12px;
        border-width: 1px;
        border-top: 1px solid #52627C;
      }
      &:last-child::before {
        height: 17px; // 可以自己调节到合适数值
      }

      .el-tree-node__children {
        padding-left: 5px; // 缩进量

        //> .el-tree-node{
        //  &::before{
        //    left: 27px;
        //    top: -17px;
        //  }
        //  &::after{
        //    left: 27px;
        //    top: -17px;
        //  }
        //}

      }
      .el-tree-node__content{
        padding-left: 3px !important;
        .el-icon{
          display: none;
        }
        .custom-tree-node{
          span{
            padding-left: 25px;
            position: relative;
          }
        }
      }
    }
    > :nth-last-child(2) {
      &::before {
        height: 14px;
      }
    }
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
