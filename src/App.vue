<template>
  <div id="app">
    <transition-group name="list" tag="div">
      <drag v-for="n in data" :key="n.title" class="drag" :data="n" @cut="remove(n)">{{ n.title }}</drag>
    </transition-group>

    <b-alert :show="showDismissibleAlert" variant="danger" dismissible>
      {{ dismissibleAlert }}
    </b-alert>

    <div class="d-flex">
      <div class="flex-column">
        <drop class="drop citrus" @drop="onCitrusDrop" mode="cut">
          <span class="drop-item" v-for="(n, index) in citrusDropped" :key="index">{{ n.title }}</span>
        </drop>
      </div>
      <div class="flex-column">
        <drop class="drop stone-fruit" @drop="onStoneFruitDrop" mode="cut">
          <span class="drop-item" v-for="(n, index) in stoneFruitDropped" :key="index">{{ n.title }}</span>
        </drop>
      </div>
      <div class="flex-column">
        <drop class="drop berries" @drop="onBerriesDrop" mode="cut">
          <span class="drop-item" v-for="(n, index) in berriesDropped" :key="index">{{ n.title }}</span>
        </drop>
      </div>
    </div>
  </div>
</template>

<script>
import { Drag, Drop } from "vue-easy-dnd";

export default {
  name: "App",
  components: {
    Drag,
    Drop
  },
  data: function () {
    return {
      data: [
        {
          title: 'oranges',
          category: "citrus",
        },
        {
          title: 'grapefruits',
          category: "citrus",
        },
        {
          title: 'limes',
          category: "citrus",
        },
        {
          title: 'nectarines',
          category: "stone fruit",
        },
        {
          title: 'apricots',
          category: "stone fruit",
        },
        {
          title: 'peaches',
          category: "stone fruit",
        },
        {
          title: 'strawberries',
          category: "berries",
        },
        {
          title: 'raspberries',
          category: "berries",
        },
        {
          title: 'blueberries',
          category: "berries",
        }
      ],
      citrusDropped: [],
      stoneFruitDropped: [],
      berriesDropped: [],
      DropStatus: false,
      showDismissibleAlert: false,
      dismissibleAlert: ''
    };
  },
  methods: {
    onCitrusDrop(event) {
      this.showDismissibleAlert = false;
      if (event.data.category == 'citrus') {
        this.DropStatus = true;
        this.citrusDropped.push(event.data);
      }
      else{
        this.dismissibleAlert = `${event.data.title} are not a citrus type`;
        this.showDismissibleAlert = true; 
      }
    },
    onStoneFruitDrop(event) {
      this.showDismissibleAlert = false;
      if (event.data.category == 'stone fruit') {
        this.DropStatus = true;
        this.stoneFruitDropped.push(event.data);
      }
      else{
        this.dismissibleAlert = `${event.data.title} are not a stone fruit type`
        this.showDismissibleAlert = true; 
      }
    },
    onBerriesDrop(event) {
      this.showDismissibleAlert = false;
      if (event.data.category == 'berries') {
        this.DropStatus = true;
        this.berriesDropped.push(event.data);
      }
      else{
        this.dismissibleAlert = `${event.data.title} are not a berry fruit type`
        this.showDismissibleAlert = true; 
      }
    },
    remove(n) {
      let title = n.title;
      let index = 0;
      this.data.forEach(dataItem => {
        var x = dataItem.title;
        if (x == title) {
          index = this.data.indexOf(dataItem)
        }
      });
      if (this.DropStatus) {
        this.data.splice(index, 1)
      }
      this.DropStatus = false;
    },
    acceptCitrus(event) {
      console.log(event)
    },
    acceptStoneFruit(event) {
      console.log(event)
    },
    acceptBerries(event) {
      console.log(event)
    }
  }
};
</script>

<style>
html,
body {
  height: 100%;
  font-family: "Roboto";
}

.drag {
  padding: 6px 16px;
  background-color: rgb(220, 220, 255);
  display: inline-flex;
  align-items: center;
  justify-content: center;
  margin: 10px 10px 0 10px;
  font-size: 18px;
  transition: all 0.5s;
}

.drop {
  padding: 10px;
  margin: 20px 10px;
  height: 80px;
  border: 1px solid black;
  position: relative;
  font-size: 18px;
  transition: all 0.5s;
}

.drop-item{
  padding: 6px;
  background-color: rgb(220, 220, 255);
  margin-right: 10px;
  display: inline-block;
}

.drop::before {
  font-size: 30px;
  position: absolute;
  top: 50%;
  left: 50%;
  transform: translate(-50%, -50%);
  font-weight: bold;
  opacity: 0.2;
  white-space: nowrap;
}

.drop.berries::before {
  content: "Berries";
}

.drop.stone-fruit::before {
  content: "Stone fruit";
}

.drop.citrus::before {
  content: "Citrus";
}

.list-enter,
.list-leave-to {
  opacity: 0;
}

.list-leave-active {
  position: absolute;
}

.d-flex{
  display: flex;
}

.flex-column{
  flex-grow: 1;
  width: calc(100%/3);
}
.alert-danger {
    color: #721c24;
    background-color: #f8d7da;
    border-color: #f5c6cb;
}
.alert-dismissible {
    padding-right: 4rem;
}
.alert {
    position: relative;
    padding: 0.75rem 1.25rem;
    margin: 1rem 10px 0rem;
    border: 1px solid transparent;
    border-radius: 0.25rem;
}
.alert-dismissible .close{
    position: absolute;
    top: 0;
    right: 0;
    z-index: 2;
    padding: 0.75rem 1.25rem; 
    font-size: 1.5rem;
    font-weight: 700;
    line-height: 1;
    color: #000;
    text-shadow: 0 1px 0 #fff;
    opacity: .5;
    background-color: transparent;
    border: 0;
    cursor: pointer;
}
</style>
