<template>
  <div class="container">
    <ul :class="['dropdown', { active: dropDownShow }]">
      <li v-for="a in list" :key="a.name" @click="selectItem(a)">
        <img :src="a.flag" class="dropdown-img" />
        <span>{{ a.alpha3Code }}</span>
        <span> {{ a.callingCodes[0] }}</span>
      </li>
    </ul>

    <div class="selectbox">
      <div @click="dropDownShow = !dropDownShow" class="imgwrapper">
        <img :src="selected ? selected.flag : ''" class="imgselected" />
        <span class="icon"> &#9660; </span>
      </div>
      <div :class="['inputwrapper', { border: focus }]">
        <label
          :for="name"
          :class="['label', { focus: focus }]"
          @click="handlelblclick"
          >{{ label }}</label
        >
        <input
          ref="txtinput"
          type="text"
          v-model="number"
          :name="name"
          @focus="focus = true"
          @blur="handleblur"
        />
      </div>
    </div>
  </div>
</template>

<script>
export default {
  data() {
    return {
      number: this.value,
      list: null,
      selected: null,
      dropDownShow: false,
      test: null,
      focus: false,
    }
  },

  methods: {
    get_data() {
      this.$axios
        .get(
          'https://restcountries.eu/rest/v2/all?fields=alpha3Code;flag;callingCodes'
        )
        .then((res) => {
          this.list = res.data
          if (this.value && this.list) {
            this.selected = this.search()
            this.find_country()
          }
        })
    },
    search() {
      let v = this.number.replace('+', '').replace(' ', '')
      let e = v.length > 4 ? 4 : v.length

      let m = this.list

      for (let i = 0; i < e; i++) {
        let s = v.substr(0, e - i)

        let g = m.filter((item) => item.callingCodes[0] == s)

        if (g[0]) {
          this.test = g[0]
          return g[0]
        }
      }
      return null
    },
    selectItem(a) {
      this.dropDownShow = !this.dropDownShow
      if (this.selected && this.selected.callingCodes) {
        let m = this.number.substr(this.selected.callingCodes[0].length + 1)

        let b = '+'.concat(a.callingCodes[0]).concat(m)
        this.number = b
      } else {
        this.number = '+'.concat(a.callingCodes[0])
        this.selected = a
      }
    },
    handleblur() {
      if (!this.number) {
        this.focus = false
      }
    },
    handlelblclick() {
      this.$refs.txtinput.focus()
    },
    find_country(){
  this.selected = this.search()
      this.$emit('input', this.number)
      if (this.number) {
        this.$refs.txtinput.focus()
      }
    }
  },
  mounted() {
    this.get_data()
  },
  watch: {
    number: function (newv, old) {
    this.find_country()
    },
  
  },

  props: ['value', 'name', 'label'],
}
</script>
<style scoped>
.container {
  position: relative;
  margin: 10px;
}
.dropdown {
  background-color: #fff;
  list-style: none;
  position: absolute;
  display: none;
  width: 300px;
  height: 400px;
  overflow-y: auto;
  cursor: pointer;
  z-index: 10;
  box-shadow: 0 -1px 10px 1px rgba(0, 0, 0, 0.1);
}
.imgselected {
  border-radius: 3px;
  width: 25px;
  height: auto;
  margin: 0 5px 0 0;
}
.dropdown .dropdown-img {
  width: 20px;
  height: auto;
}
.active {
  display: block;
}
.selectbox {
  cursor: pointer;
  display: flex;
  /* justify-content: space-between; */
  align-items: center;
}
.icon {
  margin: 0 5px 0 0;
}
.inputwrapper {
  border: 1px solid grey;
  padding: 5px;
  border-radius: 5px;
  position: relative;
}
.inputwrapper:hover {
  border: 1px solid blue;
}
.inputwrapper input {
  border: none;
  outline: none;
}
.inputwrapper input:focus ~ * {
  top: -20px;
  background-color: blue;
}
.inputwrapper label {
  position: absolute;

  background-color: #fff;
  padding: 0 5px;
}
.focus {
  top: -13px;
  color: blue;
}
.imgwrapper {
  display: flex;
  align-items: center;
}
.border {
  border: 1px solid blue;
}
</style>
