<template>
  <div id="app">
    <!-- nav title -->
    <div class="content">
      <div class="logo" @click='gotoIndex'>Leo's Blog</div>
      <!-- nav bar -->
      <ul class="navBar" v-show="isShowPC">
        <li class="navBarItem" v-for="(navBarItem, idx) in navBarItems" @click="selectNav(idx)">{{navBarItem.title}}</li>
      </ul>
      <div class="listBar" v-show="isShowMobile" @click='showList'>
        <ul class="listItems" v-show='isShowList'>
          <li class="listBarItem" v-for="(navBarItem, idx) in navBarItems" @click="selectNav(idx)">{{navBarItem.title}}</li>
        </ul>
      </div>
      </div>
    <router-view/>
  </div>
</template>

<script>
import store from '@/store'

export default {
  name: 'App',
  store,
  data() {
    return {
      navBarItems: [
        {title: 'HOME'},
        {title: 'ABOUT'},
        {title: 'ARCHIVE'},
        {title: 'CONTACT'}
      ],
      isShowPC: true,
      isShowMobile: false,
      isShowList: false,
      clientWidth: document.body.clientWidth
    }
  },
  created() {
    if (this.clientWidth < 768) {
      store.commit('clientChanged', 'mobile')
      this.isShowPC = false
      this.isShowMobile = true
    } else {
      store.commit('clientChanged', 'pc')
      this.isShowPC = true
      this.isShowMobile = false
    }
  },
  mounted(){
    var that = this;
    window.onresize = (() => { // 定义窗口大小变更通知事件
      this.clientWidth = document.body.clientWidth; //窗口宽度
      if (this.clientWidth < 768) {
        store.commit('clientChanged', 'mobile')
        this.isShowPC = false
        this.isShowMobile = true
      } else {
        store.commit('clientChanged', 'pc')
        this.isShowPC = true
        this.isShowMobile = false
      }
    });
  },
  methods: {
    selectNav(idx) {
      console.log(idx)
      let nav = this.navBarItems[idx].title.toLowerCase()
      this.$router.push({
        path: `/${nav}`
      })
    },
    showList() {
      console.log('showList')
      if(this.isShowList === false) {
        this.isShowList = true
      } else {
        this.isShowList = false
      }
    },
    gotoIndex() {
      this.$router.push({
        path:'/home'
      })
    }
  }
}
</script>

<style>
* {
  margin: 0;
  padding: 0;
  list-style: none;
  outline: none;
}

html {
  height: 100%; overflow-x: hidden; overflow-y: auto;
}
body {
  min-height: 100%;
  display: flex;
  flex-direction: column;
}
#app {
  font-family: 'Avenir', Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;

  min-height: 100%;
  display: flex;
  flex-direction: column;
}

.logo, .navBar, .listBar {
  z-index: 2;
}

.logo {
  font-size: 20px;
  position: absolute;
  left: 20px;
  top: 20px;
  font-family: '微软雅黑';
  color: #fff;
}

.logo:hover {
  cursor: pointer;
}

.navBar {
  position: absolute;
  top: 20px;
  right: 20px;
  display: flex;
  justify-content: center;
  padding: 10px 20px 10px;
}

.navBar .navBarItem {
  padding: 0 30px;
  font-size: 14px;
  font-weight: bold;
  color: #fff;
}

.navBar .navBarItem:hover {
  color: red;
  cursor: pointer;
  font-weight: bold;
  font-family: '微软雅黑';
  color: #7e7e7e;
}

/* mobile style */
.listBar {
  width: 40px;
  height: 40px;
  background-image: url(./common/image/list.png);
  position: absolute;
  top: 10px;
  right: 10px;
  cursor: pointer;
}

.listItems {
  width: 130px;
  height: 250px;
  position: absolute;
  top: 39px;
  right: 39px;
  background: #fff;
  z-index: 2;
  box-shadow: 1px 1px 1px 0.5px #888;
  border-radius: 5px;
  display: flex;
  flex-direction: column;
  justify-content: center;
}

.listItems .listBarItem {
  padding: 15px;
  display: flex;
  color: #7e7e7e;
  font-size: 16px;
}


</style>

