<template>
  <div>
    <p slot="title">分类</p>
    <div class="scrollArea">
      <ul slot="content" class="tagList">
        <li v-for="(tag,index) in filteredList" :key="index">
          <div class="icon-wrapper"
               :class="{selected:tag.name === selectedTag.name } "
               @click="select(tag)">
              <Icon :name="tag.svg"></Icon>
            {{ tag.name }}
          </div>
        </li>
        <li>
          <div class="icon-wrapper" @click="edit"> 编辑 ></div>
        </li>

      </ul>

    </div>
  </div>
</template>

<script lang="ts">


import {Component, Prop, Watch} from 'vue-property-decorator';
import Vue from 'vue';
import Parts from '@/components/Money/Parts.vue';
import Icon from '@/components/Icon.vue';
import { Tag } from '@/views/custom';

@Component({components: {Icon, Parts}})
export default class Categories extends Vue {
  @Prop() selectedTag!:Tag
  @Prop() type!: string

//better 重复
created(){
  this.$store.commit('fetchTags')
}
  get filteredList() {
    return this.$store.state.tagList.filter((tag: Tag) => tag.type === this.type)
  }

  @Watch('type')
  updateCategory() {
    if (this.type === "+") {
      this.select({name:'工资',type:'income',id:'',svg:''})
    } else{
      this.select({name:'饮食费',type:'expense',id:'',svg:''})
    }
  }
  select(tag:Tag){
    this.$emit('update:selectedTag',tag)
  }
  edit(){
    this.$router.push('/money/edit')
  }


};
</script>

<style lang="scss" scoped>
@import "~@/assets/style/helper.scss";

p {
  padding: 10px 20px;
  font-weight: bold;
}

.scrollArea {
  height: 30vh;
  overflow: auto;

  .tagList {
    padding: 0 10px;
    display: flex;
    flex-wrap: wrap;
    align-items: start;

    > li {
      width: 33.333%;
      height: 20%;
      display: flex;
      align-items: center;
      justify-content: center;

      .icon-wrapper {
        border: 1px solid lightgray;
        margin-bottom: 10px;
        border-radius: 10px;
        width: 80%;
        height: 50px;
        display: flex;
        flex-direction: column;
        align-items: center;
        justify-content: center;

        &.selected {
          box-shadow: 1px 2px 2px lightgray;
          background: #ffdead;

          svg {
            animation: shake 0.3s linear;
          }
        }
      }
      svg {
        margin-top: 2px;
        height: 50px;
        width: 50px;
      }

      @keyframes shake {
        0% {
          transform: rotate(0deg);
        }
        20% {
          transform: rotate(20deg);
        }
        40% {
          transform: rotate(0deg);
        }
        80% {
          transform: rotate(-20deg);
        }
        100% {
          transform: rotate(0deg);
        }
      }
    }
  }
}


</style>