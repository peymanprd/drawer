<script setup lang="ts">
import { ref } from 'vue'
import { toRefs, useDraggable } from '@vueuse/core'
import { useDrauu } from '@vueuse/integrations/useDrauu'

const colors = ref(['black', '#f9363a', '#496aed', '#03aa75', '#8752f2'])
const target = ref()
const tools = ref()
const showColors = ref(false)
const { undo, redo, canUndo, canRedo, brush, clear } = useDrauu(target)
const { mode, color, size } = toRefs(brush)

const { style } = useDraggable(tools, {
  initialValue: { x: target.value?.innerWidth / 2, y: 22 },
})

function setColor(_color: any) {
  color.value = _color
  showColors.value = false
}
</script>

<template>
  <div id="drawer_board" class="w-full min-h-screen relative">
    <div
      ref="tools"
      :style="style"
      @mouseleave="showColors = false"
      class="flex gap-2 absolute top-4 left-1/2 -translate-x-1/2 z-10 bg-white rounded-md drop-shadow-xl"
    >
      <div
        v-show="showColors"
        class="flex items-center absolute top-10 left-1/2 -translate-x-1/2 z-10 bg-transparent rounded-md bg-white drop-shadow-lg"
      >
        <template v-for="_color in colors" :key="_color">
          <button
            :class="{ active: _color === color }"
            @click="setColor(_color)"
            :style="{ background: _color }"
            class="w-16 h-14 hover:opacity-80 transition"
          ></button>
        </template>
      </div>
      <button class="py-2 px-2.5 text-zinc-500 border-r cursor-move">
        <svg width="24" height="24" viewBox="0 0 24 24">
          <path
            fill="currentColor"
            d="M7 19v-2h2v2H7m4 0v-2h2v2h-2m4 0v-2h2v2h-2m-8-4v-2h2v2H7m4 0v-2h2v2h-2m4 0v-2h2v2h-2m-8-4V9h2v2H7m4 0V9h2v2h-2m4 0V9h2v2h-2M7 7V5h2v2H7m4 0V5h2v2h-2m4 0V5h2v2h-2Z"
          />
        </svg>
      </button>
      <div class="flex items-center gap-2 p-1.5">
        <button
          :class="{ active: brush.mode === 'draw' }"
          class="p-2 hover:bg-zinc-100 rounded-full transition text-zinc-800 hover:text-blue-500 hover:rotate-12"
          @mouseover="showColors = true"
        >
          <svg width="24" height="24" viewBox="0 0 24 24">
            <path
              fill="currentColor"
              d="M20 13.998c-.092.065-2 2.083-2 3.5c0 1.494.949 2.448 2 2.5c.906.044 2-.891 2-2.5c0-1.5-1.908-3.435-2-3.5zm-16.586-1c0 .534.208 1.036.586 1.414l5.586 5.586c.378.378.88.586 1.414.586s1.036-.208 1.414-.586l7-7l-.707-.707L11 4.584L8.707 2.291L7.293 3.705l2.293 2.293L4 11.584c-.378.378-.586.88-.586 1.414zM11 7.412l5.586 5.586L11 18.584h.001l-.001 1v-1l-5.586-5.586L11 7.412z"
            />
          </svg>
        </button>
        <button
          :class="{ active: brush.mode === 'draw' }"
          class="p-2 hover:bg-zinc-100 rounded-full transition text-zinc-800"
          @click="mode = 'draw'"
        >
          <svg width="24" height="24" viewBox="0 0 24 24">
            <path
              fill="none"
              stroke="currentColor"
              stroke-linecap="round"
              stroke-linejoin="round"
              stroke-width="2"
              d="M4 20h4L18.5 9.5a1.5 1.5 0 0 0-4-4L4 16v4m9.5-13.5l4 4"
            />
          </svg>
        </button>
        <button
          :class="{ active: brush.mode === 'line' && !brush.arrowEnd }"
          class="p-2 hover:bg-zinc-100 rounded-full transition text-zinc-800"
          @click="mode = 'line'"
        >
          <svg width="24" height="24" viewBox="0 0 24 24">
            <path
              fill="currentColor"
              d="M19 21q-.975 0-1.75-.562q-.775-.563-1.075-1.438H11q-1.65 0-2.825-1.175Q7 16.65 7 15q0-1.65 1.175-2.825Q9.35 11 11 11h2q.825 0 1.413-.588Q15 9.825 15 9t-.587-1.413Q13.825 7 13 7H7.825q-.325.875-1.087 1.438Q5.975 9 5 9q-1.25 0-2.125-.875T2 6q0-1.25.875-2.125T5 3q.975 0 1.738.562Q7.5 4.125 7.825 5H13q1.65 0 2.825 1.175Q17 7.35 17 9q0 1.65-1.175 2.825Q14.65 13 13 13h-2q-.825 0-1.412.587Q9 14.175 9 15q0 .825.588 1.413Q10.175 17 11 17h5.175q.325-.875 1.088-1.438Q18.025 15 19 15q1.25 0 2.125.875T22 18q0 1.25-.875 2.125T19 21ZM5 7q.425 0 .713-.287Q6 6.425 6 6t-.287-.713Q5.425 5 5 5t-.713.287Q4 5.575 4 6t.287.713Q4.575 7 5 7Z"
            />
          </svg>
        </button>
        <button
          :class="{ active: brush.mode === 'rectangle' }"
          class="p-2 hover:bg-zinc-100 rounded-full transition text-zinc-800"
          @click="mode = 'rectangle'"
        >
          <svg width="24" height="24" viewBox="0 0 24 24">
            <g fill="none" fill-rule="evenodd">
              <path
                d="M24 0v24H0V0h24ZM12.593 23.258l-.011.002l-.071.035l-.02.004l-.014-.004l-.071-.035c-.01-.004-.019-.001-.024.005l-.004.01l-.017.428l.005.02l.01.013l.104.074l.015.004l.012-.004l.104-.074l.012-.016l.004-.017l-.017-.427c-.002-.01-.009-.017-.017-.018Zm.265-.113l-.013.002l-.185.093l-.01.01l-.003.011l.018.43l.005.012l.008.007l.201.093c.012.004.023 0 .029-.008l.004-.014l-.034-.614c-.003-.012-.01-.02-.02-.022Zm-.715.002a.023.023 0 0 0-.027.006l-.006.014l-.034.614c0 .012.007.02.017.024l.015-.002l.201-.093l.01-.008l.004-.011l.017-.43l-.003-.012l-.01-.01l-.184-.092Z"
              />
              <path
                fill="currentColor"
                d="M3 5a2 2 0 0 1 2-2h14a2 2 0 0 1 2 2v14a2 2 0 0 1-2 2H5a2 2 0 0 1-2-2V5Zm16 0H5v14h14V5Z"
              />
            </g>
          </svg>
        </button>
        <button
          :class="{ active: brush.mode === 'ellipse' }"
          class="p-2 hover:bg-zinc-100 rounded-full transition text-zinc-800"
          @click="mode = 'ellipse'"
        >
          <svg width="24" height="24" viewBox="0 0 24 24">
            <path
              fill="currentColor"
              d="M12 22q-2.075 0-3.9-.788q-1.825-.787-3.175-2.137q-1.35-1.35-2.137-3.175Q2 14.075 2 12t.788-3.9q.787-1.825 2.137-3.175q1.35-1.35 3.175-2.138Q9.925 2 12 2t3.9.787q1.825.788 3.175 2.138q1.35 1.35 2.137 3.175Q22 9.925 22 12t-.788 3.9q-.787 1.825-2.137 3.175q-1.35 1.35-3.175 2.137Q14.075 22 12 22Zm0-2q3.35 0 5.675-2.325Q20 15.35 20 12q0-3.35-2.325-5.675Q15.35 4 12 4Q8.65 4 6.325 6.325Q4 8.65 4 12q0 3.35 2.325 5.675Q8.65 20 12 20Zm0-8Z"
            />
          </svg>
        </button>
      </div>
      <button
        class="py-2 px-3.5 transition text-zinc-800 hover:text-blue-500 border-l"
        @click="clear"
      >
        <svg width="24" height="24" viewBox="0 0 24 24">
          <path
            fill="none"
            stroke="currentColor"
            stroke-linecap="round"
            stroke-linejoin="round"
            stroke-width="2"
            d="M19 20H8.5l-4.21-4.3a1 1 0 0 1 0-1.41l10-10a1 1 0 0 1 1.41 0l5 5a1 1 0 0 1 0 1.41L11.5 20m6.5-6.7L11.7 7"
          />
        </svg>
      </button>
    </div>
    <svg
      ref="target"
      class="w-full h-screen absolute top-0 left-0 z-[5] bg-zinc-100 grid col-span-12 border graph-paper"
    ></svg>
  </div>
</template>

<style scoped>
.graph-paper {
  background-color: #f4f4f5;
  background-image: url("data:image/svg+xml,%3Csvg xmlns='http://www.w3.org/2000/svg' width='100' height='100' viewBox='0 0 100 100'%3E%3Cg fill-rule='evenodd'%3E%3Cg fill='%23d4d4d8' fill-opacity='0.25'%3E%3Cpath opacity='.5' d='M96 95h4v1h-4v4h-1v-4h-9v4h-1v-4h-9v4h-1v-4h-9v4h-1v-4h-9v4h-1v-4h-9v4h-1v-4h-9v4h-1v-4h-9v4h-1v-4h-9v4h-1v-4H0v-1h15v-9H0v-1h15v-9H0v-1h15v-9H0v-1h15v-9H0v-1h15v-9H0v-1h15v-9H0v-1h15v-9H0v-1h15v-9H0v-1h15V0h1v15h9V0h1v15h9V0h1v15h9V0h1v15h9V0h1v15h9V0h1v15h9V0h1v15h9V0h1v15h9V0h1v15h4v1h-4v9h4v1h-4v9h4v1h-4v9h4v1h-4v9h4v1h-4v9h4v1h-4v9h4v1h-4v9h4v1h-4v9zm-1 0v-9h-9v9h9zm-10 0v-9h-9v9h9zm-10 0v-9h-9v9h9zm-10 0v-9h-9v9h9zm-10 0v-9h-9v9h9zm-10 0v-9h-9v9h9zm-10 0v-9h-9v9h9zm-10 0v-9h-9v9h9zm-9-10h9v-9h-9v9zm10 0h9v-9h-9v9zm10 0h9v-9h-9v9zm10 0h9v-9h-9v9zm10 0h9v-9h-9v9zm10 0h9v-9h-9v9zm10 0h9v-9h-9v9zm10 0h9v-9h-9v9zm9-10v-9h-9v9h9zm-10 0v-9h-9v9h9zm-10 0v-9h-9v9h9zm-10 0v-9h-9v9h9zm-10 0v-9h-9v9h9zm-10 0v-9h-9v9h9zm-10 0v-9h-9v9h9zm-10 0v-9h-9v9h9zm-9-10h9v-9h-9v9zm10 0h9v-9h-9v9zm10 0h9v-9h-9v9zm10 0h9v-9h-9v9zm10 0h9v-9h-9v9zm10 0h9v-9h-9v9zm10 0h9v-9h-9v9zm10 0h9v-9h-9v9zm9-10v-9h-9v9h9zm-10 0v-9h-9v9h9zm-10 0v-9h-9v9h9zm-10 0v-9h-9v9h9zm-10 0v-9h-9v9h9zm-10 0v-9h-9v9h9zm-10 0v-9h-9v9h9zm-10 0v-9h-9v9h9zm-9-10h9v-9h-9v9zm10 0h9v-9h-9v9zm10 0h9v-9h-9v9zm10 0h9v-9h-9v9zm10 0h9v-9h-9v9zm10 0h9v-9h-9v9zm10 0h9v-9h-9v9zm10 0h9v-9h-9v9zm9-10v-9h-9v9h9zm-10 0v-9h-9v9h9zm-10 0v-9h-9v9h9zm-10 0v-9h-9v9h9zm-10 0v-9h-9v9h9zm-10 0v-9h-9v9h9zm-10 0v-9h-9v9h9zm-10 0v-9h-9v9h9zm-9-10h9v-9h-9v9zm10 0h9v-9h-9v9zm10 0h9v-9h-9v9zm10 0h9v-9h-9v9zm10 0h9v-9h-9v9zm10 0h9v-9h-9v9zm10 0h9v-9h-9v9zm10 0h9v-9h-9v9z'/%3E%3Cpath d='M6 5V0H5v5H0v1h5v94h1V6h94V5H6z'/%3E%3C/g%3E%3C/g%3E%3C/svg%3E");
}
</style>
