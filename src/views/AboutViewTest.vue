<template>
  <div class="node-list">
    <el-card shadow="never" :body-style="{ padding: '20px 0px 20px 20px' }">
      <div style="margin-bottom: 10px">
        <span style="margin-right: 5px">showLabelLine</span>
        <el-switch v-model="showLabelLine"></el-switch>
        <span style="margin-left: 20px;margin-right:5px;">indent</span>
        <el-input-number v-model="indent"></el-input-number>
      </div>
      <el-tree
          ref="tree"
          :data="data"
          :props="defaultProps"
          :indent="indent"
          :icon="iconConfig"
          @node-click="handleNodeClick"
      >
        <template v-slot:default="{ node }">
          <element-tree-line
              :node="node"
              :showLabelLine="showLabelLine"
              :indent="indent"
          >
            <!-- 自定义label的slot -->
            <template v-slot:node-label>
                        <span style="font-size: 12px">
                            {{ node.label }}
                            <i class="el-icon-eleme"></i
                            ></span>
            </template>
          </element-tree-line>
        </template>
      </el-tree>
    </el-card>
  </div>
</template>

<script setup>
import { ref, watch, h } from 'vue'
import { getElementLabelLine } from "element-tree-line"
const ElementTreeLine = getElementLabelLine(h)
import 'element-tree-line/dist/style.css'
const indent = ref(13)
const showLabelLine = ref(false)
const filterText = ref('')
const treeRef = ref()

const defaultProps = {
  children: 'children',
  label: 'label',
}
const iconConfig = {
  expand: (node) => {
    if (node.children && node.children.length > 0) {
      return 'el-icon-folder-opened'; // 有子节点时展开图标
    } else {
      return 'el-icon-document'; // 没有子节点时展开图标
    }
  },
  collapse: (node) => {
    if (node.children && node.children.length > 0) {
      return 'el-icon-folder'; // 有子节点时折叠图标
    } else {
      return 'el-icon-document'; // 没有子节点时折叠图标
    }
  },
};

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
// 处理节点点击事件的方法
const handleNodeClick = (data, node) => {
  console.log(data, node)
}
const renderContent = (h, { node, data, store })  => {
  let icon = 'el-icon-folder'; // 默认图标
  if (node.leaf) {
    icon = 'el-icon-document'; // 叶子节点图标
  } else if (node.expanded) {
    icon = 'el-icon-folder-opened'; // 展开节点图标
  }

  // 如果需要根据节点是否选中来设置图标，可以添加以下逻辑
  if (node.checked) {
    icon = 'el-icon-check'; // 选中节点图标
  }

  return h('span', [
    h('i', { class: icon }),
    node.label
  ]);
}
</script>


<style scoped lang="less">
.node-list{
  width: 500px;
  height: 1000px;
  padding: 10px;
  box-sizing: border-box;
  backdrop-filter: blur(130px);
  background-color: rgba(58, 55, 56, 0.3);

}
</style>
