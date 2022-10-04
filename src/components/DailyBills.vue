<template>
  <div>
    <q-list :dense="true">
      <q-item v-for="bill in bills" :key="bill.id">
        <single-bill
          :id="bill.id"
          :content="bill.content"
          :amount="bill.amount"
          :mode="bill.mode"
          @change="(msg) => emit('update', msg)"
          @delete="(id) => emit('deleteId', id)"
        ></single-bill>
      </q-item>
    </q-list>
  </div>
</template>

<script setup lang="ts">
import SingleBill, { SingleBillProps } from './SingleBill.vue';
export interface DailyBillsProps {
  date: string;
  bills?: SingleBillProps[];
}

const emit = defineEmits<{
  (e: 'update', value: SingleBillProps): void;
  (e: 'deleteId', id: string): void;
}>();

withDefaults(defineProps<DailyBillsProps>(), {
  bills: () => [],
});
</script>
