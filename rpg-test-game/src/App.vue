<template>
  <div class="container-fluid ">
    <div class="row mt-2">
      <div class="title col-6 py-3 offset-3 text-center">
        <h1>Monster Slayer</h1>
      </div>
    </div>
    <!-- Selection Screen -->
    <div v-if="createdChar == false">
      <div class="row my-4 text-center">
        <h2>Select your character</h2>
      </div>
      <div class="row d-flex justify-content-center">
        <div class="col-lg-6 d-flex justify-content-between">
          <div
            class="m-1 col p-1 d-flex flex-column align-items-center justify-content-end portraitDiv"
            @click="selectThis('archer', 0)"
            :class="{ highlighted: selectedChar == charList[0].name }"
          >
            <img
              class="portrait"
              :src="charList[0].img"
              :alt="charList[0].name"
            />
            <p class="mt-3">Archer</p>
          </div>
          <div
            class="col m-1 p-1 d-flex flex-column align-items-center justify-content-end portraitDiv"
            @click="selectThis('knight', 1)"
            :class="{ highlighted: selectedChar == charList[1].name }"
          >
            <img
              class="portrait"
              :src="charList[1].img"
              :alt="charList[1].name"
            />
            <p class="mt-3">Knight</p>
          </div>
          <div
            class="col m-1 p-1 d-flex flex-column align-items-center justify-content-end portraitDiv"
            @click="selectThis('mage', 2)"
            :class="{ highlighted: selectedChar == charList[2].name }"
          >
            <img
              class="portrait"
              :src="charList[2].img"
              :alt="charList[2].name"
            />
            <p class="mt-3">Mage</p>
          </div>
        </div>
      </div>

      <div class="row">
        <div class="col-sm-8 offset-sm-2 col-lg-6 offset-lg-3 mt-3">
          <div class="input-group mb-3">
            <input
              type="text"
              class="form-control"
              placeholder="Enter your name"
              aria-label="Enter your name"
              aria-describedby="button-addon2"
              v-model="name"
            />
            <button
              class="btn btn-outline-secondary"
              type="button"
              id="button-addon2"
              @click="createCharacter"
            >
              Create
            </button>
          </div>
        </div>
      </div>
    </div>
  </div>
    <!-- Actual Game after Selection -->
  <div class="container-fluid" v-if="createdChar">
    <div
    class="monsterRow row justify-content-center text-center"
  >
    <div class="col-sm-8 col-lg-6 border gameBox">
      <div class="col">
        <div class="col mb-1">
          <h2 class="enemyName">Nito</h2>
          <div class="lifeBar mb-5 col-8 offset-2">
            <div
              class="col lifeBarInside"
              :style="{ width: bossLife + '%' }"
            ></div>
          </div>
        </div>
      </div>
      <div class="d-flex justify-content-between align-items-end">
        <div class="player col-2 d-flex flex-column align-items-center">
          <img :src="charList[listIndex].img" alt="" />
        </div>

        <div class="monsterImg col-6">
          <img src="../public/imgs/monsters/nito.gif" alt="" />
        </div>
      </div>
    </div>
  </div>
  <div class="row mt-3 p-2 justify-content-center">
    <div class="col-sm-8 col-lg-6 border d-flex">
      <div class="col d-flex flex-column">
        <span>{{ name }}</span>
        
        
        <div class="d-flex align-items-center">
        <span class="me-2"><b>HP: </b></span>
        <div class="playerBar d-flex align-items-center me-2">
          <div class="playerBarInside"
          :style="{ width: playerLife + '%' }"></div>
        </div>
        <span>{{playerCurrhp}}/{{playerMaxhp}}</span>
        </div>
      </div>
    </div>
  </div>

  <div class="row mt-3 p-2 justify-content-center">
    <div class="col-sm-8 col-lg-6 border d-flex">
      <div class="col d-flex">
        <button @click="playerAttack">Attack</button>
      </div>
    </div>
  </div>
  </div>
</template>

<script>
export default {
  name: "App",
  components: {},
  data() {
    return {
      createdChar: false,
      name: "",
      selectedChar: "",
      charList: [
        {
          name: "archer",
          img: require("../public/imgs/player/archer.gif"),
          color: "rgb(197, 232, 243);",
        },
        { name: "knight", img: require("../public/imgs/player/knight.gif") },
        { name: "mage", img: require("../public/imgs/player/mage.gif") },
      ],
      listIndex: "",
      enemyMaxhp: 500,
      enemyCurrhp: 500,
      bossLife: "",
      playerMaxhp: 100,
      playerCurrhp: 100,
      playerLife: ''
    };
  },
  methods: {
    selectThis(character, index) {
      this.selectedChar = character;
      this.listIndex = index;
    },
    createCharacter() {
      if (this.name && this.selectedChar) {
        console.log(
          `${this.name}, the legendary ${this.selectedChar} is born!`
        );
        this.createdChar = true;
      }
    },
    calculateLifeBars() {
      this.bossLife = (100 * this.enemyCurrhp) / this.enemyMaxhp;
      this.playerLife = (100 * this.playerCurrhp) / this.playerMaxhp;
    },
    playerAttack() {
      this.enemyCurrhp -= 100;
      this.playerCurrhp -= 14;
      this.calculateLifeBars();
    },
  },
  mounted() {
    this.calculateLifeBars();
  },
};
</script>

<style>
.portrait {
  width: 80%;
}
.portraitDiv {
  display: flex;
  align-items: flex-end;
}
.portrait:hover {
  transform: scale(110%);
}
.highlighted {
  /* transform: scale(120%); */
  box-shadow: 0px 1px 20px 30px rgb(231, 231, 231);
  background: rgb(231, 231, 231);
  border-radius: 100%;
  padding: 5px;
  transition: 0.35s;
}
.monsterRow {
}
.monsterRow img {
  width: 100%;
}
.playerRow img {
  width: 100%;
}
.mHealth {
  background-color: greenyellow;
}

.playerRow {
}

.lifeBar {
  height: 20px;
  background-color: rgb(39, 39, 39);
  border: 2px solid black;
  border-radius: 15px;
}

.lifeBarInside {
  height: 15px;
  background-color: rgb(180, 8, 8);
  border-radius: 15px;
  :width: "";
}

.playerBar {
  height: 10px;
  background-color: rgb(39, 39, 39);
  border: 2px solid black;
  border-radius: 15px;
  width: 30%;
}

.playerBarInside {
  height: 5px;
  background-color: rgb(139, 230, 139);
  border-radius: 15px;
  :width: '';
}

.gameBox {
  background-image: url("../public/imgs/bg/nito.gif");
  background-size: contain;
  background-repeat: repeat-x;
  border-radius: 10px;
  border: black;
  box-shadow: 0px 3px 10px 1px black;
  padding: 10px;
  /* background-position: bottom; */
}

.enemyName {
  color: rgb(58, 6, 6);
  text-shadow: -1px 2px 1px rgb(192, 16, 16);
}

/* width  = 100currhp / maxhp + '%' */
</style>
