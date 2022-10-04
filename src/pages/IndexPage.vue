<template>
  <q-page>
    <div class="q-pa-md">
      <div class="row">
        <div class="col">
          <q-btn icon="event" round color="primary">
            <q-popup-proxy
              cover
              transition-show="scale"
              transition-hide="scale"
            >
              <q-date
                v-model="currentDate"
                :events="events"
                event-color="orange"
              />
            </q-popup-proxy>
          </q-btn>
        </div>
        <div class="col">
          <q-btn
            color="white"
            text-color="black"
            label="新建"
            @click="addBill"
          />
        </div>
      </div>
      <div class="row">
        <div class="text-h4 q-mb-md">{{ currentDate }}</div>
        <daily-bills
          :date="currentDate"
          :bills="bills"
          @update="(msg) => updateBill(msg)"
          @delete-id="(id) => deleteBill(id)"
        ></daily-bills>
      </div>
    </div>

    <!-- <daily-bills :date="currentDate" :bills="bills"></daily-bills> -->
  </q-page>
</template>

<script setup lang="ts">
import { watch, ref } from 'vue';
import { date, LocalStorage, uid } from 'quasar';
import DailyBills from 'src/components/DailyBills.vue';
import { SingleBillProps } from 'src/components/SingleBill.vue';

const currentDate = ref(date.formatDate(new Date(), 'YYYY/MM/DD'));
const proxyDate = ref(date.formatDate(new Date(), 'YYYY/MM/DD'));
const bills = ref(
  LocalStorage.getItem<SingleBillProps[]>(currentDate.value) ?? []
);
const events = ref(LocalStorage.getAllKeys());

watch(currentDate, () => {
  events.value = LocalStorage.getAllKeys();
  bills.value =
    LocalStorage.getItem<SingleBillProps[]>(currentDate.value) ?? [];
});

function updateBill(billProps: SingleBillProps) {
  bills.value?.forEach((el) => {
    if (el.id === billProps.id) {
      el.content = billProps.content;
      el.mode = billProps.mode;
      el.amount = billProps.amount;
    }
  });
  LocalStorage.set(currentDate.value, bills.value);
  console.log('更新 %s 的账单成功', currentDate.value);
}

function deleteBill(id: string) {
  bills.value = bills.value?.filter((el) => el.id !== id);
  if (bills.value?.length !== 0) {
    LocalStorage.set(currentDate.value, bills.value);
  } else {
    LocalStorage.remove(currentDate.value);
    events.value = LocalStorage.getAllKeys();
  }

  console.log('删除 %s 的 %s 号账单成功', currentDate.value, id);
}

function addBill() {
  bills.value?.push({ id: uid() });
  LocalStorage.set(currentDate.value, bills.value);
  events.value = LocalStorage.getAllKeys();
}
</script>
