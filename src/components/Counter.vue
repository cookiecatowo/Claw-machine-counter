<template>
  <div class="flex flex-col gap-2">
    <div class="flex items-center gap-2 text-lg font-normnal">
      <span class="shrink-0">禮品名稱</span>
      <Input v-model="current.name"/>
    </div>
    <div class="flex items-center gap-2 text-lg font-normnal">
      <span class="shrink-0">禮品價格</span>
      <Input v-model="current.cost" type="number"/>
    </div>
    
    <div class="grid grid-cols-2 gap-2">
      <div class="bg-white border border-gray-300 rounded-lg grid">
        <div class="grid p-4 border-b border-gray-300">
          <span>目前投幣金額</span>
          <span> {{ current.spend }}</span>
        </div>
        <div class="grid p-4">
          <span>目前損益</span>
          <span>{{ current.cost - current.spend }}</span>
        </div>
      </div>
      <div class="flex flex-col gap-2">
        <Button class="text-lg font-normnal bg-slate-500 h-2/3" @click="coin">投幣</Button>
        <div class="grid grid-cols-2 gap-2 h-fit">
          <!-- <Dialog>
            <DialogTrigger class="h-full w-full">
              <Button class="text-lg font-normnal bg-slate-500 w-full h-full" @click="getPirce">出貨</Button>
            </DialogTrigger>
            <DialogContent>
              <DialogHeader>
                <DialogTitle>恭喜出貨!</DialogTitle>
                <DialogDescription>
                  是否要沿用禮品名稱與禮品金額?
                </DialogDescription>
              </DialogHeader>
              <DialogFooter>
                <DialogClose class="w-full flex gap-2">
                  <Button class="font-semibold bg-slate-500 w-1/2">沿用</Button>
                  <Button class="font-semibold bg-white border border-2 border-slate-500 w-1/2 text-late-500 hover:bg-slate-200" @click="currentInit">清除</Button>
                </DialogClose>
              </DialogFooter>
            </DialogContent>
          </Dialog> -->
          <Button class="text-lg font-normnal bg-slate-500 w-full h-full" @click="getPirce">出貨</Button>
          <Button class="text-lg font-normnal bg-slate-500 h-full" @click="giveUp">放棄</Button>
        </div>
        
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
    <div class="bg-white border border-gray-300 rounded-lg py-4 px-8 grid grid-cols-2 gap-3 text-lg font-semibold"
      :class="totalProfit - totalSpend > 0 ? 'text-green-700': 'text-red-700'" >
      <span class="text-right">總利潤</span>
      <span>{{ totalProfit - totalSpend }} 元</span>
    </div>
    <Button class="text-lg font-semibold bg-slate-500 h-16" @click="clear">全部清除</Button>
    <Drawer>
      <DrawerTrigger>
        <Button class="text-lg font-semibold bg-slate-500 h-16 w-full">展開明細</Button>
      </DrawerTrigger>
      <DrawerContent>
        <div class="p-6 text-sm">
          <div class="grid grid-cols-4 font-semibold text-base border-b leading-10">
            <span>禮品名稱</span>
            <span>禮品售價</span>
            <span>投幣金額</span>
            <span>利潤</span>
            <!-- <span>操作</span> -->
          </div>
          <div v-for="item,iter in itemList" class="grid grid-cols-4 leading-10">
            <span>{{item.name || '禮品' + (iter+1)}}</span>
            <span>{{item.cost}}</span>
            <span>{{item.spend}}</span>
            <span>{{ item.cost - item.spend || -item.spend }}</span>
          </div>
          <div v-if="itemList.length < 1" class="h-12 flex items-center justify-center">
            目前暫無項目
          </div>
        </div>
        <DrawerFooter>
          <div class="grid grid-cols-3 gap-2">
            <div class="bg-white border border-gray-300 rounded-lg p-2 flex flex-wrap justify-center items-center gap-2 text-base font-semibold">
              <span class="text-right">總花費</span>
              <span>{{ totalSpend }} 元</span>
            </div>
            <div class="bg-white border border-gray-300 rounded-lg p-2 flex flex-wrap justify-center items-center gap-2 text-base font-semibold">
              <span class="text-right">總收益</span>
              <span>{{ totalProfit }} 元</span>
            </div>
            <div class="bg-white border border-gray-300 rounded-lg p-2 flex flex-wrap justify-center items-center gap-2 text-base font-semibold "
              :class="totalProfit - totalSpend > 0 ? 'text-green-700': 'text-red-700'">
              <span class="text-right">總利潤</span>
              <span>{{ totalProfit - totalSpend }} 元</span>
            </div>
          </div>
        </DrawerFooter>
      </DrawerContent>
    </Drawer>
  </div>
</template>
<script setup>
  import { Input } from './ui/input'
  import { Button } from './ui/button'
  import {
  Drawer,
  DrawerContent,
  DrawerTrigger,
  DrawerFooter,
} from '@/components/ui/drawer'
import {
  Dialog,
  DialogContent,
  DialogDescription,
  DialogFooter,
  DialogHeader,
  DialogTitle,
  DialogTrigger,
  DialogClose
} from '@/components/ui/dialog'
  import { ref } from 'vue'

  const current = ref({})
  current.value = {
    spend: 0,
    cost: 0,
    name: ''
  }
  const totalSpend = ref(0)
  const totalProfit = ref(0)
  const itemList = ref([])
  const coin = () => {
    current.value.spend += 10
    totalSpend.value += 10
  }
  const getPirce = () => {
    totalProfit.value += Number(current.value.cost)
    itemList.value.push({
      spend: current.value.spend,
      cost: current.value.cost,
      name: current.value.name 
    })
    current.value.spend = 0
  }
  const currentInit = () => {
    current.value = {
      spend: 0,
      cost: 0,
      name: ''
    }
  }
  const giveUp = () => {
    itemList.value.push({
      spend: current.value.spend,
      cost: '未夾出',
      name: current.value.name 
    })
    currentInit()
  }
  const clear = () => {
    currentInit()
    totalProfit.value = 0
    totalSpend.value = 0
    itemList.value = []
  }

</script>