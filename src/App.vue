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
        <button @click="submit()" class="btn btn-sm btn-success mr-3">Submit</button>
        </div>
         <div>
          <h4>Shape</h4>
          <pre  id="shape">
            <!-- pattern Displaying here -->
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
      height: 24,
      width: 20,
      padding: 4,
      pattern: "",
      level:0,
      pixelArray:[]
    };
  },
  mounted(){
    //Calling the funtion for the first time(onLoad) for getting default value pattern
    this.submit();
  },
  methods: {
    //Button Click Function
    submit() {
      //Reseting the variable for new Shape
      this.pattern = ''
      this.pixelArray = []
      this.level = 0
      //Validation of user input
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
      
      // Calling funtion for returning  the two dimentional array 
      // this.height - 1,  this.width - 1 : Decrement height by 1, for easy calculation in array
      // this.padding / 2 + 1 : To get the next cordinate for start assign the array values
      const pixelArray = this.draw(this.height - 1, this.width - 1, this.padding / 2 + 1);
  
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
    // Function Which Pushing the digits to pixelArr/drawing the garden/pattern
    draw(height, width, padding) {

      //Initializing two dimentional array for height and width 
      if (typeof this.pixelArray == "undefined" || this.pixelArray.length == 0) {
        //Initializing height/Column
        this.pixelArray = new Array(height + 1);

        //Initializing width/Row with respect to height/Column
        for (var i = 0; i < this.pixelArray.length; i++) {
          this.pixelArray[i] = new Array(width + 1);
        }
        //Declaring the two dimentional array with zero for empty space
        for (var k = 0; k <= height; k++) {
          for (var j = 0; j <= width; j++) {
            this.pixelArray[k][j] = 0;
          }
        }
      }

      // level - No. of level for garden draw
      // this.level * padding - For getting the space difference between levels

      if (height >= this.level * padding && width >= this.level * padding) {
        //Assigning value 2 to pixelArray for printing height for the each levels
        //Assigning values from bottom to up
        for (var h = height; h >= this.level * padding; h--) {
          this.pixelArray[h][width] = 2;
          this.pixelArray[h][this.level * padding] = 2;
        }

        //Assigning value 1 to pixelArray for printing width  for the each levels
        for (var w = width; w >= this.level * padding; w--) {
          this.pixelArray[height][w] = 1;
          this.pixelArray[this.level * padding][w] = 1;
        }
      }

      //Decreasing Height and Width for jump to next levels/Inner levels
      height -= padding;
      width -= padding;

      //Return the pixelArray if the space available is less than padding
      if (height < padding || width < padding) {
        return this.pixelArray;
      }

      //Incrementing levels to move to inner levels
      this.level++;
     
      //Recursive call to draw pattern/gardern of all the levels
      return this.draw(height, width, padding);
    }
  }
};
</script>
