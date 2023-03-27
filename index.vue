<template>
  <nut-category :category="category" @change="change">
    <nut-category-pane :categoryChild="categoryChild" @onChange="onChange">
    </nut-category-pane>
  </nut-category>
  <Tabbar/>
</template>
<script lang="ts">
import { reactive, toRefs, onMounted } from 'vue';

export default {
  setup() {
    const data = reactive({
      categoryInfo: {},
      category: [{}],
      categoryChild: [{}]
    });

    onMounted(() => {
      setTimeout(() => {
        getData();
      }, 500);
    });

    const getData = () => {
      fetch('//storage.360buyimg.com/nutui/3x/categoryData.js')
        .then((response) => response.json())
        .then((res) => {
          const { categoryInfo, categoryChild } = res;
          data.categoryInfo = categoryInfo;
          data.category = categoryInfo.category;
          data.categoryChild = categoryChild;
        })
        .catch((err) => console.log('Oh, error', err)); 
    };

    const change = (index: any) => {
      data.categoryChild = [].concat(data.categoryInfo.category[index + 1].children as any);
    };
    const onChange =()=>{
        console.log("当前分类数据");
    }
    return {
      onChange,
      change,
      ...toRefs(data)
    }
  }
};
</script>
