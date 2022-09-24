<template>
  <q-page class="row items-center justify-evenly">
    <q-date v-model="date" minimal />
    <example-component
      title="示例内容"
      a
      测试
      ive
      :todos="todos"
      :meta="meta"
    ></example-component>
  </q-page>
</template>

<script lang="ts">
import { Todo, Meta } from 'components/models';
import ExampleComponent from 'components/ExampleComponent.vue';
import { defineComponent, ref } from 'vue';

export default defineComponent({
  name: 'IndexPage',
  components: { ExampleComponent },
  setup() {
    const todos = ref<Todo[]>([
      {
        id: 1,
        content: '测试 1',
      },
      {
        id: 2,
        content: '测试 2',
      },
      {
        id: 3,
        content: '测试 3',
      },
      {
        id: 4,
        content: '测试 4',
      },
      {
        id: 5,
        content: '测试 5',
      },
    ]);
    const meta = ref<Meta>({
      totalCount: 1200,
    });
    console.log(new Date().toLocaleDateString());
    return { todos, meta, date: ref(new Date().toLocaleDateString()) };
  },
});
</script>
