<template>
  <div class="hello">
    <input placeholder="text" type="text" v-model="textToAdd.text">
    <input placeholder="x" type="number" v-model="textToAdd.x">
    <input placeholder="y" type="number" v-model="textToAdd.y">
    <input placeholder="fontFamily" type="text" v-model="textToAdd.fontFamily">
    <input placeholder="fontSize" type="number" v-model="textToAdd.fontSize">
    <button @click="addText">Add</button>
    <p>hint: is draggable</p>
    <v-stage ref="stage" :config="stageSize">
      <v-layer ref="layer">
        <v-image
          :config="{
            x: 0,
            y: 0,
            draggable: false,
            image: image
          }"
        />
      </v-layer>
      <v-layer ref="layer">
        <v-text
          v-for="(text, index) in textList"
          :key="index"
          @dragstart="handleDragStart"
          @dragend="handleDragEnd"
          :config="{
            text: text.text,
            x: text.x,
            y: text.y,
            draggable: true,
            fontSize: text.fontSize,
            fontFamily: text.fontFamily,
            fill: isDragging ? 'green' : 'red'
          }"
        />
      </v-layer>
    </v-stage>
  </div>
</template>

<script>

export default {
  data() {
    return {
      textToAdd: {
        text: 'Testing 1 2 3',
        fontSize: 18,
        fontFamily: 'Impact',
        x: 25,
        y: 50
      },
      stageSize: {
        width: 800,
        height: 300
      },
      isDragging: false,
      textList: [{text: 'testText'}],
      image: null
    };
  },
  created() {
    const image = new window.Image();
    image.src = 'https://upload.wikimedia.org/wikipedia/en/e/e9/Gandalf600ppx.jpg'
    image.onload = () => {
      // set image only when it is loaded
      this.image = image;
    };
  },
  methods: {
    handleDragStart() {
      this.isDragging = true;
    },
    handleDragEnd() {
      this.isDragging = false;
    },
    addText() {
      this.textList.push(this.textToAdd)
      this.textToAdd = {}
    }
  }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
h3 {
  margin: 40px 0 0;
}
ul {
  list-style-type: none;
  padding: 0;
}
li {
  display: inline-block;
  margin: 0 10px;
}
a {
  color: #42b983;
}

canvas {
  max-width: 100%
}

</style>
