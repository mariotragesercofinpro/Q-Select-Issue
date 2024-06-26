<template>
  <q-layout view="hHh lpR fFf">
    <q-page-container>
      <div>
        <div>Directly using the component MySelect</div>
        <MySelect></MySelect>
        <br>
        <div>Using MySelect as custom element</div>
        <custom-select></custom-select>
      </div>
    </q-page-container>
  </q-layout>
</template>

<script setup>
import MySelect from "@/components/MySelect.vue";
import {createApp, defineCustomElement, getCurrentInstance, h} from "vue";
import {Quasar} from "quasar";

const app = createApp({}).use(Quasar);
const customElement = defineCustomElement({
  setup(props) {
    const instance = getCurrentInstance();
    Object.assign(instance.appContext, app._context);
    Object.assign(instance.provides, app._context.provides);
    return () => (
        h('body',
            h(MySelect, props)
        )
    );
  }
});
customElements.define('custom-select', customElement);
</script>
