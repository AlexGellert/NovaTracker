<style>
@import './../../styles/themes.less';

.menu-icons{
  fill: var(--nav-fill);
  width: 1.5em;
  height: 1.5em;
  margin-right: 10px;
}

.toolbar-title{
  background: var(--layout-body-color);
  color: var(--nav-fill);
  box-shadow: 0 2px 2px 0 rgba(0, 0, 0, 0.14), 0 3px 1px -2px rgba(0, 0, 0, 0.2),
    0 1px 5px 0 rgba(0, 0, 0, 0.12);
}

#menu-content{
  background: var(--layout-body-background);
  color: var(--layout-body-color);
}

.menu-align{
  display: flex;
  padding: 10px 5px;
  border-bottom: 1px solid white;
  margin-left: 15px;
}

.menu-text{
  font-size: 18px;
  margin: auto 0px;
}

ion-scroll>.scroll-inner {
  background: var(--layout-body-background);
}

</style>

<template>
<ion-page>
  <ion-menu side="start" content-id="menu-content">
    <ion-header>
      <ion-toolbar>
        <ion-title>Navigation</ion-title>
      </ion-toolbar>
    </ion-header>
    <ion-content>
    <div id="menu-content">
        <div class="menu-align" v-for="item in items" :key="item.name" @click="changeLocation(item.href)">
          <svg-icon class="menu-icons" :name="item.icon" slot="start"></svg-icon>
          <label class="menu-text">{{ item.name }}</label>
        </div>
    </div>
    </ion-content>
  </ion-menu>
</ion-page>
</template>

<script lang="ts">
import { Component, Vue, Prop } from 'vue-property-decorator';
import { dataBaseAPI } from '@/services/dataBaseAPI';
import SvgIcon from './svg/svg.vue';

@Component({
  components: {
    SvgIcon
  }
})
export default class MenuLeft extends Vue {
    items = [{ name: 'Home', icon: 'nova-icon', href: '/' }, { name: 'Inventory', icon: 'list', href: '/inventory' },
        { name: 'Quick Edit', icon: 'magnify', href: '/quickEdit' }, { name: 'Theme Selector', icon: 'color-picker', href: '/themes'}, { name: 'Log out', icon: 'user', href: 'logout'}];
    
    constructor(){
        super();
    }

    async changeLocation(link){
      if(link === 'logout'){
        dataBaseAPI.toggleLogin();
        if(this.$router.currentRoute.fullPath != '/'){
          this.$router.push('/');
        }
      }else if(this.$router.currentRoute.fullPath != link){
        this.$router.push(link);
        var openMenu = (document.querySelector("ion-menu-controller") as any).close("start");
      }
    }
    
  }
</script>