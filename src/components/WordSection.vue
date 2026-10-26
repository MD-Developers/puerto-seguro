<script setup>
import { ref, onMounted } from 'vue'
import { supabase } from '@/services/supabaseClient'

const telegramContainer = ref(null)
const widget = ref([
  {
    username:'',
    message: ''
  }
]);

onMounted(() => {
  getTelegramVideo();
})

async function getTelegramVideo() {
  const { data, error: fetchError } = await supabase.from('words').select()
  if (fetchError) {
    return;
  }
  widget.value = data;

  if(widget.value.length > 0){
    const script = document.createElement('script')
    script.async = true
    script.src = 'https://telegram.org/js/telegram-widget.js?24'
    script.setAttribute('data-telegram-post', `${widget.value[0].username}/${widget.value[0].message}`)
    script.setAttribute('data-width', '50%')
    script.setAttribute('data-userpic', 'false')
    script.setAttribute('data-color', '343638')
    script.setAttribute('data-dark', '1')
    script.setAttribute('data-dark-color', 'FFFFFF')
  
    // Insertar el script dentro del contenedor de la plantilla
    if (telegramContainer.value) {
      telegramContainer.value.appendChild(script)
    }
  }
}

function getImg() {
  return `https://whtkhfteodtfnaskugwz.supabase.co/storage/v1/object/public/pdf-images/documentos/alimento.png?t=${Date.now()}`;
}
</script>
<template>
    <section class="py-14 bg-white dark:bg-white/5 min-h-160.25" id="palabras">
        <div class="max-w-7xl mx-auto px-4 sm:px-6 lg:px-8">
            <div class="w-full flex flex-col md:flex-row justify-center items-center md:gap-x-30 gap-y-10">
                <div ref="telegramContainer" class="telegram-embed"></div>
                <div class="flex w-[32%] min-w-[316.15px] overflow-hidden rounded-sm">
                    <img  :src="getImg()" alt="Palabra" srcset="">
                </div>
            </div>
        </div>
    </section>
</template>
<style scoped>
.telegram-embed {
  display: flex;
  justify-content: center;
}
</style>