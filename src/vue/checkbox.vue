<template>
    <fieldset :class="question.cssClasses.root">
        <div v-for="(item, index) in question.visibleChoices" :key="item.value" :class="getItemClass(item)">
            <label :class="question.cssClasses.label">
                <input v-if="item == question.selectAllItem" type="checkbox" :name="question.name" :value="isAllSelected" v-model="isAllSelected" :id="question.inputId + '_' + item.value" :disabled="question.isReadOnly" v-bind:aria-label="question.locTitle.renderedHtml" :class="question.cssClasses.itemControl"/>
                <input v-if="item != question.selectAllItem" type="checkbox" :name="question.name" :value="item.value" v-model="question.value" :id="question.inputId + '_' + item.value" :disabled="question.isReadOnly" v-bind:aria-label="question.locTitle.renderedHtml" :class="question.cssClasses.itemControl"/>
                <span :class="question.cssClasses.materialDecorator"><span class="check"></span></span>
                <span :class="question.cssClasses.controlLabel"><survey-string :locString="item.locText"/></span>
                <survey-other-choice v-show="question.hasOther && question.value && question.isOtherSelected" v-if="index == choicesCount" :question="question" />
            </label>
        </div>
        <legend style="display: none;">{{question.locTitle.renderedHtml}}</legend>
    </fieldset>
</template>

<script lang="ts">
import Vue from "vue";
import { Component, Prop, Watch } from "vue-property-decorator";
import { default as QuestionVue } from "./question";
import { QuestionCheckboxModel } from "../question_checkbox";

@Component
export class Checkbox extends QuestionVue<QuestionCheckboxModel> {
  get colWidth() {
    var colCount = this.question.colCount;
    return colCount > 0 ? 100 / colCount + "%" : "";
  }
  get choicesCount() {
    return this.question.visibleChoices.length - 1;
  }
  getItemClass(item) {
    var itemClass =
      this.question.cssClasses.item +
      (this.question.colCount === 0
        ? " sv_q_checkbox_inline"
        : " sv-q-col-" + this.question.colCount);
    if (this.question.isItemSelected(item)) {
      itemClass += " checked";
    }
    return itemClass;
  }
  get isAllSelected() {
    return this.question.isAllSelected;
  }
  set isAllSelected(val: boolean) {
    this.question.isAllSelected = val;
  }
}
Vue.component("survey-checkbox", Checkbox);
export default Checkbox;
</script>
