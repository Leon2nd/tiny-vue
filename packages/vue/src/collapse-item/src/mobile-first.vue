<template>
  <div
    data-tag="tiny-collapse-item"
    class="border-b-0.5 sm:border-b border-solid border-color-border-separator last:border-0"
  >
    <div
      role="tab"
      data-tag="tiny-collapse-item-tab"
      :aria-expanded="state.isActive"
      :aria-controls="`tiny-collapse-content-${state.id}`"
      :aria-describedby="`tiny-collapse-content-${state.id}`"
    >
      <div
        data-tag="tiny-collapse-item-body"
        class="flex relative items-center h-12 sm:h-10 py-0 sm:pr-0 pl-4 pr-10 text-sm font-bold justify-between whitespace-nowrap"
        :id="`tiny-collapse-head-${state.id}`"
        :tabindex="disabled ? undefined : 0"
        @keyup.space.enter.stop="handleEnterClick"
        :class="
          disabled
            ? 'text-color-text-disabled'
            : 'text-color-text-primary sm:[&:has(.peer:hover)_[role=title]]:text-color-brand'
        "
        @focus="handleFocus"
        @blur="state.focusing = false"
      >
        <div
          data-tag="tiny-collapse-item-title"
          class="whitespace-nowrap overflow-hidden overflow-ellipsis inline-block peer"
          :class="[disabled ? 'cursor-not-allowed' : 'cursor-pointer sm:hover:text-color-brand']"
          role="title"
          @click="handleHeaderClick"
        >
          <slot name="title"> {{ title }}</slot>
        </div>
        <div data-tag="tiny-collapse-item-titleright" class="text-xs font-normal">
          <slot name="title-right">{{ titleRight }}</slot>
        </div>
        <div
          data-tag="tiny-collapse-item-icon"
          class="absolute sm:left-0 right-3.5 text-xs mr-1 w-3 peer"
          :class="[
            disabled
              ? 'fill-color-text-disabled  cursor-not-allowed'
              : 'fill-color-icon-secondary cursor-pointer sm:peer-hover:fill-color-brand sm:hover:fill-color-brand'
          ]"
          @click="handleHeaderClick"
        >
          <slot name="icon" :active="state.isActive" :disabled="disabled">
            <icon-arrow-bottom
              custom-class="w-2.5 h-2.5 sm:w-3 sm:h-3 transition-transform duration-300"
              :class="[state.isActive ? 'sm:rotate-0 rotate-180' : 'sm:-rotate-90 rotate-0']"
            />
          </slot>
        </div>
      </div>
    </div>
    <collapse-transition>
      <div
        data-tag="tiny-collapse-item-active"
        v-show="state.isActive"
        class="will-change-[height] bg-color-bg-1 overflow-hidden box-border"
        role="tabpanel"
        :aria-hidden="!state.isActive"
        :aria-labelledby="`tiny-collapse-head-${state.id}`"
        :id="`tiny-collapse-content-${state.id}`"
      >
        <div class="pb-6 pt-0 px-4 sm:pt-0 sm:pr-0 sm:pl-4 sm:pb-4 text-xs text-color-text-primary leading-relaxed">
          <slot></slot>
        </div>
      </div>
    </collapse-transition>
  </div>
</template>

<script lang="ts">
import { renderless, api } from '@opentiny/vue-renderless/collapse-item/vue'
import { props, setup, defineComponent } from '@opentiny/vue-common'
import CollapseTransition from '@opentiny/vue-collapse-transition'
import { IconArrowBottom } from '@opentiny/vue-icon'

export default defineComponent({
  props: [...props, 'title', 'titleRight', 'name', 'disabled'],
  components: {
    CollapseTransition,
    IconArrowBottom: IconArrowBottom()
  },
  setup(props, context): any {
    return setup({ props, context, renderless, api })
  }
})
</script>
