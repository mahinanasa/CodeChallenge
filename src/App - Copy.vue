<template>
  <div id="app">
    <div class="container-fluid ">
      <div class="mx-auto">
        <div class="form-inline my-3">
        <div class="form-group mr-2">
          <label>Width</label>
          <input v-model="width" class="form-control" id="w" />
        </div>
        <div class="form-group mr-2">
          <label>Height</label>
          <input v-model="height" class="form-control" id="h" />
        </div>
        <div class="form-group mr-2">
          <label>Padding</label>
          <input v-model="padding" class="form-control" id="o" />
        </div>
        <!-- Call a function while button click -->
        <button @click="main_draw()" class="btn btn-sm btn-success mr-3">Submit</button>
        </div>
         <div>
          <h4>Shape</h4>
          <pre id="shape">
         {{pattern}} 
          </pre>
        </div>
      </div>
    </div>
  </div>
</template>
<script>
export default {
  name: "recursive-shape",
  data() {
    return {
      height: 24 ,
      width: 20,
      padding: 4,
      pattern: "",
      level:0
    };
  },
  mounted(){
    this.main_draw();
  },
  methods: {
    main_draw() {
  this.pattern = ''
   this.draw.shape = []
   this.draw.level = 0
      if (
        this.height < 20 ||
        this.height % 2 != 0 ||
        this.width < 20 ||
        this.width % 2 != 0 ||
        this.padding < 4 ||
        this.padding % 2 != 0
      ) {
        alert("Invalid input");
        return;
      }
      
       const pixelArray = this.draw(
        this.height - 1,
        this.width - 1,
        this.padding / 2 + 1
      );
  
   this.pattern =  "\n"
      for (var i = 0; i < this.height; i++) {
        for (var j = 0; j < this.width; j++) {
          switch (pixelArray[i][j]) {
            case 0:
              this.pattern += " ";
              break;
            case 1:
              this.pattern += "-";
              break;
            case 2:
              this.pattern += "|";
              break;
          }
        }
        this.pattern += "\n";
      }
    },

    draw(height, width, padding) {
    

      //static variables
      //draw.shape
      //draw.level

      if (typeof this.draw.shape == "undefined" || this.draw.shape.length == 0) {
        this.draw.shape = new Array(height + 1);

        //mapping row to height
        for (var i = 0; i < this.draw.shape.length; i++) {
          this.draw.shape[i] = new Array(width + 1);
        }

        for (var k = 0; k <= height; k++) {
          for (var j = 0; j <= width; j++) {
            this.draw.shape[k][j] = 0;
          }
        }
      }

      if (typeof this.draw.level == "undefined") {
        this.draw.level = 0;
      }
      //Logic starts
      // padding increasing
      // this.draw.level * padding - reference from outer pattern
      //
      //const currentPadding = this.draw.level * padding
      if (
        height >= this.draw.level * padding &&
        width >= this.draw.level * padding
      ) {
        //i =0
        //bottom to top this.drawing

        for (var h = height; h >= this.draw.level * padding; h--) {
          this.draw.shape[h][width] = 2;
          this.draw.shape[h][this.draw.level * padding] = 2;
        }

        for (var w = width; w >= this.draw.level * padding; w--) {
          this.draw.shape[height][w] = 1;
          this.draw.shape[this.draw.level * padding][w] = 1;
        }
      }

      //For showing the space btwm shapes
      height -= padding;
      width -= padding;

      if (height < padding || width < padding) {
        return this.draw.shape;
      }

      this.draw.level++;

      return this.draw(height, width, padding);
    }
  }
};
</script>
