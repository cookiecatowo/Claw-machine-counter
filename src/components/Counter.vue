<template>
  <div class="flex flex-col gap-2">
    <div class="flex items-center gap-2 text-lg font-normnal">
      <span class="shrink-0">禮品名稱</span>
      <Input v-model="name"/>
    </div>
    <div class="flex items-center gap-2 text-lg font-normnal">
      <span class="shrink-0">禮品價格</span>
      <Input v-model="cost"/>
    </div>
    
    <div class="grid grid-cols-2 gap-2">
      <div class="bg-white border border-gray-300 rounded-lg grid">
        <div class="grid p-4 border-b border-gray-300">
          <span>目前投幣金額</span>
          <span> {{ spend }}</span>
        </div>
        <div class="grid p-4">
          <span>目前損益</span>
          <span>{{ cost - spend }}</span>
        </div>
      </div>
      <div class="flex flex-col gap-2">
        <Button class="text-lg font-normnal bg-slate-500 h-1/2" @click="coin">投幣</Button>
        <Button class="text-lg font-normnal bg-slate-500 h-1/2" @click="getPirce">出貨</Button>
        <Button class="text-lg font-normnal bg-slate-500 h-1/2" @click="giveUp">放棄</Button>
      </div>
    </div>
    <div class="bg-white border border-gray-300 rounded-lg py-4 px-8 grid grid-cols-2 gap-3 text-lg font-semibold">
      <span class="text-right">總花費</span>
      <span>{{ totalSpend }} 元</span>
    </div>
    <div class="bg-white border border-gray-300 rounded-lg py-4 px-8 grid grid-cols-2 gap-3 text-lg font-semibold">
      <span class="text-right">總收益</span>
      <span>{{ totalProfit }} 元</span>
    </div>
    <div class="bg-white border border-gray-300 rounded-lg py-4 px-8 grid grid-cols-2 gap-3 text-lg font-semibold text-red-700">
      <span class="text-right">總利潤</span>
      <span>{{ totalProfit - totalSpend }} 元</span>
    </div>
    <Button class="text-lg font-semibold bg-slate-500 h-16" @click="clear">全部清除</Button>
  </div>
</template>
<script setup>
  import { Input } from './ui/input'
  import { Button } from './ui/button'
  import { ref } from 'vue'
  const spend = ref(0)
  const cost = ref(0)
  const name = ref('')
  const totalSpend = ref(0)
  const totalProfit = ref(0)

  const coin = () => {
    spend.value += 10
    totalSpend.value += 10
  }
  const getPirce = () => {
    totalProfit.value += Number(cost.value)
    spend.value = 0
  }
  const giveUp = () => {
    spend.value = 0
  }
  const clear = () => {
    giveUp()
    totalProfit.value = 0
    totalSpend.value = 0
    cost.value = 0
    name.value = 0
  }

</script>