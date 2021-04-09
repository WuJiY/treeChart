<template>
  <table>
    <tr>
      <td
        :colspan="hasChild ? model.children.length * 2 : 1"
        :class="{ extend: hasChild }"
      >
        <div class="card">
          <div class="title">
            <p
              v-if="!isEdit.name"
              style="cursor: pointer"
              @click="toEdit('NAME')"
              class="text-style">
              {{model.name}}
            </p>
            <el-input
              v-model="model.name"
              size="mini"
              v-if="isEdit.name"
              v-focus="isEdit.name"
              @blur="edit('NAME')"
              @keyup.enter.native="$event.target.blur"/>

          </div>
          <div class="body">
            <div style="flex: 1;border-bottom: 1px solid #ccc;border-left:1px solid #ccc;padding: 10px 10px">
              <p
                v-if="!isEdit.x"
                style="cursor: pointer"
                @click="toEdit('X')"
                class="text-style">
                {{model.x}}
              </p>
              <el-input
                v-model="model.x"
                size="mini"
                v-if="isEdit.x"
                v-focus="isEdit.x"
                @blur="edit('X')"
                @keyup.enter.native="$event.target.blur"/>
            </div>
            <div
              style="flex: 1;background-color:#D0E1A0;border-right: 1px solid #D0E1A0;border-bottom: 1px solid #D0E1A0;padding: 10px 10px">
              <p
                v-if="!isEdit.y"
                style="cursor: pointer"
                @click="toEdit('Y')"
                class="text-style">
                {{model.y}}
              </p>
              <el-input
                v-model="model.y"
                size="mini"
                v-if="isEdit.y"
                v-focus="isEdit.y"
                @blur="edit('Y')"
                @keyup.enter.native="$event.target.blur"/>
            </div>

            <div class="operation-relation">
              <i class="el-icon-circle-plus-outline" style="cursor: pointer" @click="$emit('on-add', model)"></i>
            </div>
            <div class="operation-relation"
                 style="right:0;">
              <i class="el-icon-circle-close" style="cursor: pointer" @click="$emit('on-remove', model)"></i>
            </div>
          </div>
        </div>
      </td>
    </tr>
    <transition name="fade">
      <tr v-if="hasChild">

        <transition-group name="list" tag="p">
          <td
            v-for="item in model.children"
            :key="item.id"
            colspan="2"
            class="child"
          >
            <TreeChart :model="item" v-on="$listeners"/>


          </td>
        </transition-group>

      </tr>
    </transition>


  </table>

</template>
<script>
  /**
   * 中间连接线分别是上层的::after伪类和下层::before和::after(横线)提供
   * 跨层级监听事件 v-on="$listeners"
   */

  function toEdit(flag) {

    switch (flag) {

      case 'NAME':
        this.isEdit.name = true;
        break
      case 'X':
        this.isEdit.x = true;
        break
      case 'Y':
        this.isEdit.y = true;
        break
    }

  }

  function edit(flag) {
    this.$emit('on-update', this.model)

    switch (flag) {

      case 'NAME':
        this.isEdit.name = false;
        break
      case 'X':
        this.isEdit.x = false;
        break
      case 'Y':
        this.isEdit.y = false;
        break
    }


  }

  export default {
    name: 'TreeChart',
    props: ['model'],
    data() {
      return {
        isEdit: {
          x: false,
          y: false,
          name: false
        },
      }
    },
    computed: {
      hasChild() {
        return this.model.children && this.model.children.length
      }
    },
    methods: {
      toEdit,
      edit
    }
  }
</script>
<style lang="scss">
  .card {
    border: 20px solid #ffffff;
    border-radius: 5px;
    border-top: 0;
    margin: 0 auto;
    width: 250px;


    .title {
      padding: 12px 10px;
      font-size: 18px;
      background-color: #6E6A67;
      color: white;
    }

    .body {
      height: 50px;
      background: #ffffff;
      width: auto;
      display: flex;
      position: relative;
    }
  }

  .text-style {
    height: 26px;
    line-height: 26px;
    margin: 0;
    border: 1px dashed transparent;

    &:hover {
      border: 1px dashed #ccc;
    }
  }

  .fade-enter-active, .fade-leave-active {
    transition: opacity ease 0.5s;
  }

  .fade-enter, .fade-leave-to /* .fade-leave-active below version 2.1.8 */
  {
    opacity: 0;
  }


  .list-item {
    display: inline-block;
    margin-right: 10px;
  }

  .list-enter-active, .list-leave-active {
    transition: all ease 0.5s;
  }

  .list-enter, .list-leave-to
    /* .list-leave-active for below version 2.1.8 */
  {
    opacity: 0;
  }

  .operation-relation {
    position: absolute;
    width: 100px;
    height: 25px;
    bottom: -25px;
    line-height: 25px;
    font-size: 20px;
    opacity: 0;
  }

  .card:hover {
    .operation-relation {
      opacity: 1;
    }
  }

  table {
    border-collapse: separate !important;
    border-spacing: 0 !important;

    td {
      position: relative;
      vertical-align: top;
      padding: 0 0 50px 0;
      text-align: center;

      &.extend {
        &::after {
          content: '';
          position: absolute;
          left: 50%;
          bottom: 35px;
          height: 35px;
          border-left: 2px solid rgb(159, 186, 202);

        }
      }

      &.child {
        &::before {
          content: '';
          position: absolute;
          left: 50%;
          top: -34px;
          height: 34px;
          border-left: 2px solid rgb(159, 186, 202);
        }

        &::after {
          content: '';
          position: absolute;
          left: 0;
          right: 0;
          top: -36px;
          border-top: 2px solid rgb(159, 186, 202);
        }

        &:first-child:before,
        &:last-child:before {
          display: none;
        }

        &:first-child:after {
          left: 50%;
          height: 33px;
          border: 2px solid;
          border-color: rgb(159, 186, 202) transparent transparent rgb(159, 186, 202);
          border-radius: 6px 0 0 0;
          transform: translateX(1px);
        }

        &:last-child:after {
          right: 50%;
          height: 33px;
          border: 2px solid;
          border-color: rgb(159, 186, 202) rgb(159, 186, 202) transparent transparent;
          border-radius: 0 6px 0 0;
          transform: translateX(-1px);
        }

        &:first-child.child:last-child::after {
          transform: translateX(0);
          border-radius: 0;
          border-color: transparent transparent transparent rgb(159, 186, 202);
        }
      }
    }
  }
</style>

