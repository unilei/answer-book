<script setup>
const {locale, locales, setLocale} = useI18n()

const availableLocales = computed(() => {
  return (locales.value).filter(i => i.code !== locale.value)
})
const switchLocalePath = useSwitchLocalePath()

import answerData from '@/assets/data/answer'

const answerIndexData = ref([])
const answer = ref({})
const showAnswer = ref(false)
const handleGetAnswer = () => {
  showAnswer.value = true;
  answer.value = {}
  const loading = ElLoading.service({
    text: '',
    target: '#my-answer',
    background: 'rgba(255, 255, 255, 0.7)'
  })
  setTimeout(function () {
    if (answerIndexData.value.length === answerData.length) {
      answerIndexData.value = []
    }
    let index = Math.floor(Math.random() * answerData.length)
    while (answerIndexData.value.includes(index)) {
      index = Math.floor(Math.random() * answerData.length)
    }
    answerIndexData.value.push(index)
    answer.value = answerData[index]
    loading.close()
  }, 3000)

}
</script>

<template>
  <div>
    <p class="bg-yellow-400 p-[10px] font-bold text-center text-slate-800">
      🔥 {{ $t('p_3') }}
    </p>
    <h1 class="max-w-[1240px] mx-auto text-[22px] font-bold mt-[40px] flex gap-1 flex-row items-center justify-center ">
      <nuxt-link to="/">{{ $t('websiteName') }}</nuxt-link>
      <nuxt-link class="text-[12px] text-red-500"
                 href="https://github.com/unilei/answer-book.git" target="_blank">
        <Icon name="uil:github" color="black" size="24"/>
      </nuxt-link>
    </h1>

    <div class="mt-[20px] text-center">
      <NuxtLink v-for="locale in availableLocales" :key="locale.code" :to="switchLocalePath(locale.code)">
        🌐 {{ locale.name }}
      </NuxtLink>
    </div>

    <template v-if="showAnswer">
      <div id="my-answer"
           v-show="showAnswer"
           class="max-w-[520px] mx-auto border border-gray-300 p-[20px] text-center mt-[40px] rounded-[8px] bg-yellow-50">
        <p class="font-bold text-[22px]">
          🥰 {{ locale === 'en' ? answer.answer_en : answer.answer }}
        </p>
      </div>
      <div class="text-center">
        <el-button color="#664d00" type="primary" class="mt-[20px]" @click="handleGetAnswer()">
          {{$t('getAnswer')}}
        </el-button>
      </div>
    </template>
    <template v-else>
      <div class="max-w-[520px] mx-auto border border-gray-300 p-[20px] text-center mt-[40px] rounded-[8px] bg-yellow-50">

        <p class="text-[16px] font-bold mb-[10px] text-slate-800">{{ $t('p_1') }}</p>
        <p class="text-[14px] font-bold mb-[10px] text-slate-800">{{ $t('p_2') }}</p>

        <div class="mt-[20px] p-[10px] border border-gray-300 rounded-[12px]">
          <p class="text-[14px] mb-[10px] text-slate-800">{{ $t('p_5_1') }}</p>
          <p class="text-[14px] mb-[10px] text-slate-800">{{ $t('p_5_2') }}</p>
          <p class="text-[14px] mb-[10px] text-slate-800">{{ $t('p_5_3') }}</p>
          <p class="text-[14px] mb-[10px] text-slate-800">{{ $t('p_5_4') }}</p>
          <p class="text-[14px] mb-[10px] text-slate-800">{{ $t('p_5_5') }}</p>
          <p class="text-[14px] mb-[10px] text-slate-800">{{ $t('p_5_6') }}</p>
        </div>

        <div>
          <el-button color="#664d00" type="primary" class="mt-[20px]" @click="handleGetAnswer()">
            {{$t('getAnswer')}}
          </el-button>
        </div>
      </div>
    </template>

  </div>
</template>

<style scoped>

</style>