<template>
  <transition
    appear
    enter-active-class="animated fadeIn"
    leave-active-class="animated fadeOut"
  >
    <q-card class="my-card" name="id">
      <q-card-section horizontal:true>
        <q-input
          clearable
          color="orange"
          :model-value="content"
          @update:model-value="(val) => (tmp.content = val as string)"
          label="记账"
        />
      </q-card-section>
      <q-card-section horizontal:true>
        <q-input
          color="orange"
          type="number"
          :model-value="amount"
          @update:model-value="(val) => (tmp.amount = val  as number)"
          label="金额"
          style="max-width: 200px"
        />
      </q-card-section>
      <q-card-section horizontal:true>
        <q-select
          :model-value="mode"
          @update:model-value="(val) => (tmp.mode = val as string)"
          :options="options"
          label="收入/支出"
          style="width: 250px"
        />
      </q-card-section>
      <q-card-section horizontal:true>
        <q-btn
          color="white"
          text-color="black"
          label="删除"
          @click="emit('delete', id)"
        />
      </q-card-section>
    </q-card>
  </transition>
</template>

<script setup lang="ts">
import { Ref, ref, watch } from 'vue';

export interface SingleBillProps {
  id: string;
  content?: string;
  amount?: number;
  mode?: string;
}

const emit = defineEmits<{
  (e: 'change', value: SingleBillProps): void;
  (e: 'delete', id: string): void;
}>();

let xx = withDefaults(defineProps<SingleBillProps>(), {
  content: '',
  amount: 1,
  mode: '支出',
});

const tmp: Ref<SingleBillProps> = ref({
  id: xx.id,
  content: xx.content,
  amount: xx.amount,
  mode: xx.mode,
});

watch(tmp.value, (newTmp) => {
  emit('change', newTmp as SingleBillProps);
});

const options = ['支出', '收入'];
</script>

<style lang="sass" scoped>
.my-card
  width: 100%
</style>
