<template>
  <div class="pagination">
    <button @click="firstPage">首页</button>
    <button @click="prevPage">上一页</button>
    <button v-if="pageButtons[0] !== 1">...</button>
    <button
      v-for="(page, index) in pageButtons"
      :key="index"
      @click="changeButton(page)"
      :class="[{currentPage: page === currentPage}, 'pageButton']"
    >{{page}}</button>
    <button>...</button>
    <button @click="nextPage">下一页</button>
  </div>
</template>

<script>
export default {
  name: "Pagination",
  data() {
    return {
      pageButtons: [1, 2, 3, 4, 5],
      currentPage: 1
    };
  },
  methods: {
    changeButton(page) {
      this.currentPage = page;
      if (page === this.pageButtons[4]) {
        this.pageButtons.shift();
        this.pageButtons.push(this.pageButtons[3] + 1);
      } else if (page === this.pageButtons[0] && page !== 1) {
        this.pageButtons.unshift(this.pageButtons[0] - 1);
        this.pageButtons.pop();
      }
      this.$emit('handleList', this.currentPage)
    },
    nextPage() {
      this.changeButton(this.currentPage + 1);
    },
    prevPage() {
      if (this.currentPage > 1) {
        this.changeButton(this.currentPage - 1);
      }
    },
    firstPage() {
      this.changeButton(1);
      this.pageButtons = [1, 2, 3, 4, 5];
    }
  }
};
</script>

<style scoped>
.pagination {
  margin-top: 5px;
  margin-bottom: 20px;
  background-color: white;
  padding: 6px 20px;
  border-radius: 5px;
  /*box-shadow: 0px 2px 9px #888888;*/
  border: 1px solid #888888;
}

button {
  background-color: #fff;
  border: 1px solid #ddd;
  color: #778087;
  border-radius: 3px;
  outline: none;
  height: 21px;
  cursor: pointer;
  padding: 0 2px;
  width: 55px;
  height: 29px;
}

.pageButton {
  position: relative;
  bottom: 1px;
  width: 40px;
  margin: 0 4px;
}

.currentPage {
  color: white;
  background-color: #1f1b1b;
}
</style>