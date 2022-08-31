<template>
  <div class="ProcUnitPaging">
    <a href="javascript:;" class="InnerBtn" v-on:click="handlePageTurn('First', 0)">首页</a>
    <a href="javascript:;" class="InnerBtn" v-bind:key="'PrevPage-' + numItemIndex" v-for="(numPrevPage, numItemIndex) in numArrayPrevPage" 
    v-on:click="handlePageTurn('specifiedIndex2', numPrevPage)">{{ numPrevPage }}</a>
    <span class="InnerBtn">{{ numCurrentPage }}</span>
    <a href="javascript:;" class="InnerBtn" v-bind:key="'NextPage-' + numItemIndex" v-for="(numNextPage, numItemIndex) in numArrayNextPage" 
    v-on:click="handlePageTurn('specifiedIndex2', numNextPage)">{{ numNextPage }}</a>
    <a href="javascript:;" class="InnerBtn" v-on:click="handlePageTurn('Last', 0)">末页</a>
    <a href="javascript:;" class="InnerBtn" v-on:click="handlePageTurn('Prev', 0)">上页</a>
    <a href="javascript:;" class="InnerBtn" v-on:click="handlePageTurn('Next', 0)">下页</a>
    <div class="ElemTntegrTextBox">
      <input type="text" class="InnerMark" placeholder="指定页" maxlength="6" v-model="strSpecifiedPage">
      <a href="javascript:;" class="InnerMark Addimark" v-on:click="handlePageTurn('specifiedIndex', parseInt(strSpecifiedPage))">Go</a>
    </div>
    <p class="InnerShowText">第 {{ numCurrentPage }} 页，总共 {{ numPageQuantity }} 页，{{ numRowQuantity }} 条数据</p>
  </div>
</template>

<script setup>
  import { onMounted, ref } from 'vue';
  let numRowQuantity = ref(0),   // 总数据
      numPageQuantity = ref(0),   // 每页显示的数据
      numCurrentPage = ref(1),   // 显示当前页码
      strSpecifiedPage = ref(1);
  let numArrayPrevPage = ref([]),
      numArrayNextPage = ref([]);

  // 初始化数据
  function setValFuncInit() {
    numRowQuantity.value = 200;
    const pageSize = 10;
    numPageQuantity.value  = Math.ceil(numRowQuantity.value / pageSize);
    handleBtnRender();
  }
  // 渲染页面
  function handleBtnRender() {
    numArrayPrevPage.value = [];
    for (let numPrevLoop = 3; numPrevLoop >= 1; numPrevLoop--) {
      const numPageIndex = numCurrentPage.value - numPrevLoop;
      if (numPageIndex >= 1) {
        numArrayPrevPage.value.push(numPageIndex);
      }
    }
    numArrayNextPage.value = [];
    for(let numNextLoop = 1; numNextLoop <= 3; numNextLoop++) {
      const numPageIndex2 = numCurrentPage.value + numNextLoop;
      if(numPageIndex2 <= numPageQuantity.value) {
        numArrayNextPage.value.push(numPageIndex2);
      }
    }
  }
  function handlePageTurn(actionIdIn, numPageIndexIn) {
    // 当前页码
    const numCurrentPageInner = numCurrentPage.value;
    // 总页数
    const numPageQuantityInner = numPageQuantity.value;
    switch(actionIdIn) {
      case "First":
        numPageIndexIn = 1;
        break;
      case "Prev":
        if(numCurrentPageInner <= 1) {
          return;
        }
        else {
          numPageIndexIn = numCurrentPageInner - 1;
        }
        break;
      case "Next":
        if(numCurrentPageInner >= numPageQuantityInner) {
          alert("没有啦~");
          return;
        }
        else {
          numPageIndexIn = numCurrentPageInner + 1;
        }
        break;
      case "Last":
        if(numCurrentPageInner == numPageQuantityInner)
        return;
        numPageIndexIn = numPageQuantityInner;
        break;
      case "specifiedIndex":
      case "specifiedIndex2":
        if(numPageIndexIn > numPageQuantityInner) {
          alert("无了");
          return;
        }
        break;
      default:
        break;
    }
    if(!isNaN(numPageIndexIn) && numPageIndexIn > 0) {
      numCurrentPage.value = numPageIndexIn;
      console.log(numCurrentPage.value);
      handleBtnRender();
    }
  }
  onMounted(() => {
    setValFuncInit();
  })
</script>

<style scoped>
  * {
    margin: 0;
    padding: 0;
  }
  a {
    text-decoration: none;
  }
  .ProcUnitPaging,.ElemTntegrTextBox {
    display: flex;
    align-items: center;
  }
  .InnerBtn {
    margin: 0 10px;
  }
  a {
    color: black;
  }
  span {
    color: blue;
  }
  a, span {
    border: 1px solid black;
    padding: 5px 10px;
    border-radius: 10px;
    background-color: beige;
  }
  .ElemTntegrTextBox input {
    border-radius: 5px;
    height: 30px;
  }
  .Addimark {
    margin: 0 10px;
  }
</style>
