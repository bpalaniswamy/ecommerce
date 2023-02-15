<template>
  <div id="container">
    <template v-for="data in gridData">
      <div
        :class="{ fullWidth: data.position.startsWith('row') }"
        class="item"
        v-if="data.type === 'html'"
        v-html="data.contents"
      ></div>
      <div v-else class="item">
        {{ data.title }}
      </div>
    </template>
  </div>
</template>
<script>
export default {
  name: "HearstECommerce",
  props: {},
  data() {
    let contents = [
      {
        type: "html",
        contents: "<p>Free shipping on orders over $50</p>",
        position: "row-3",
      },
      {
        type: "html",
        contents: "<p>Example banner HTML text</p>",
        position: "row-1",
      },
      {
        type: "html",
        contents: "<p>Some other content</p>",
        position: "cell-3",
      },
      {
        type: "html",
        contents: "<p>Content in product cell</p>",
        position: "cell-5",
      },
    ];
    let products = [
      {
        product_id: 1,
        url: "/product/1",
        title: "Product A",
        image: "https://picsum.photos/200",
      },
      {
        product_id: 2,
        url: "/product/2",
        title: "Gizmo B",
        image: "https://picsum.photos/200",
      },
      {
        product_id: 3,
        url: "/product/3",
        title: "Widget C",
        image: "https://picsum.photos/200",
      },
      {
        product_id: 5,
        url: "/product/5",
        title: "Subscription E",
        image: "https://picsum.photos/200",
      },
      {
        product_id: 4,
        url: "/product/4",
        title: "Magazine D",
        image: "https://picsum.photos/200",
      },
      {
        product_id: 6,
        url: "/product/6",
        title: "Ticket F",
        image: "https://picsum.photos/200",
      },
      {
        product_id: 7,
        url: "/product/7",
        title: "Video G",
        image: "https://picsum.photos/200",
      },
    ];
    let gridData = [];
    return {
      contents,
      products,
      gridData,
    };
  },
  mounted() {
    this.getGridData();
  },
  methods: {
    getGridData() {
      this.gridData = [...this.products];
      let cellContents = this.contents.filter((content) => {
        return content.position.startsWith("cell");
      });
      let rowContents = this.contents.filter((content) => {
        return content.position.startsWith("row");
      });

      cellContents.forEach((cell) => {
        let index = cell.position.split("-")[1];
        this.gridData.splice(index - 1, 0, cell);
      });
      rowContents = rowContents.sort((a, b) => {
        if (a.position.split("-")[1] > b.position.split("-")[1]) {
          return 1;
        } else {
          return -1;
        }
      });
      console.log("rowc", rowContents);
      let newlyInsertedRows = 0;
      rowContents.forEach((row) => {
        let rowNumber = row.position.split("-")[1];
        let indexToBeInserted = 3 * (rowNumber - 1) - newlyInsertedRows * 2;
        this.gridData.splice(indexToBeInserted, 0, row);
        newlyInsertedRows++;
      });
    },
  },
};
</script>
<style scoped>
@media screen and (min-width: 500px) {
  #container {
    display: grid;
    grid-template-columns: 1fr 1fr 1fr;
    border: 1px solid black;
  }
}

.fullWidth {
  grid-column: 1 / -1;
  text-align: left;
  align-items: start;
  justify-items: self-start;
}
.item {
  border: 1px solid black;
}
</style>