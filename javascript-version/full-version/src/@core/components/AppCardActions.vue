<script setup>
const props = defineProps({
  collapsed: {
    type: Boolean,
    required: false,
    default: false,
  },
  noActions: {
    type: Boolean,
    required: false,
    default: false,
  },
  actionCollapsed: {
    type: Boolean,
    required: false,
    default: false,
  },
  actionRefresh: {
    type: Boolean,
    required: false,
    default: false,
  },
  actionRemove: {
    type: Boolean,
    required: false,
    default: false,
  },
  title: {
    type: String,
    required: false,
    default: undefined,
  },
})

const emit = defineEmits([
  'collapsed',
  'refresh',
  'trash',
])

defineOptions({ inheritAttrs: false })

const isContentCollapsed = ref(props.collapsed)
const isCardRemoved = ref(false)
const isOverlayVisible = ref(false)

// hiding overlay
const hideOverlay = () => {
  isOverlayVisible.value = false
}

// trigger collapse
const triggerCollapse = () => {
  isContentCollapsed.value = !isContentCollapsed.value
  emit('collapsed', isContentCollapsed.value)
}

// trigger refresh
const triggerRefresh = () => {
  isOverlayVisible.value = true
  emit('refresh', hideOverlay)
}

// trigger removal
const triggeredRemove = () => {
  isCardRemoved.value = true
  emit('trash')
}
</script>

<template>
  <VExpandTransition>
    <!-- TODO remove div when transition work with v-card components: https://github.com/vuetifyjs/vuetify/issues/15111 -->
    <div v-if="!isCardRemoved">
      <VCard v-bind="$attrs">
        <VCardItem>
          <VCardTitle v-if="props.title || $slots.title">
            <!-- 👉 Title slot and prop -->
            <slot name="title">
              {{ props.title }}
            </slot>
          </VCardTitle>

          <template #append>
            <!-- 👉 Before actions slot -->
            <div>
              <slot name="before-actions" />

              <!-- SECTION Actions buttons -->

              <!-- 👉 Collapse button -->
              <VBtn
                v-if="(!(actionRemove || actionRefresh) || actionCollapsed) && !noActions"
                icon
                color="default"
                variant="text"
                size="x-small"
                @click="triggerCollapse"
              >
                <VIcon
                  size="20"
                  icon="mdi-chevron-up"
                  :style="{ transform: isContentCollapsed ? 'rotate(-180deg)' : null }"
                  style="transition-duration: 0.28s;"
                />
              </VBtn>

              <!-- 👉 Overlay button -->
              <VBtn
                v-if="(!(actionRemove || actionCollapsed) || actionRefresh) && !noActions"
                icon
                size="x-small"
                variant="text"
                color="default"
                @click="triggerRefresh"
              >
                <VIcon
                  size="20"
                  icon="mdi-refresh"
                />
              </VBtn>

              <!-- 👉 Close button -->
              <VBtn
                v-if="(!(actionRefresh || actionCollapsed) || actionRemove) && !noActions"
                icon
                size="x-small"
                variant="text"
                color="default"
                @click="triggeredRemove"
              >
                <VIcon
                  size="20"
                  icon="mdi-close"
                />
              </VBtn>
            </div>
          <!-- !SECTION -->
          </template>
        </VCardItem>

        <!-- 👉 card content -->
        <VExpandTransition>
          <div
            v-show="!isContentCollapsed"
            class="v-card-content"
          >
            <slot />
          </div>
        </VExpandTransition>

        <!-- 👉 Overlay -->
        <VOverlay
          v-model="isOverlayVisible"
          contained
          persistent
          class="align-center justify-center"
        >
          <VProgressCircular indeterminate />
        </VOverlay>
      </VCard>
    </div>
  </VExpandTransition>
</template>

<style lang="scss">
.v-card-item {
  +.v-card-content {
    .v-card-text:first-child {
      padding-block-start: 0;
    }
  }
}
</style>
