<script setup>
const props = defineProps({
  userData: {
    type: Object,
    required: true,
  },
  isDialogVisible: {
    type: Boolean,
    required: true,
  },
})

const emit = defineEmits([
  'update:modelValue',
  'submit',
  'update:isDialogVisible',
])

const userData = ref(structuredClone(toRaw(props.userData)))
const isUseAsBillingAddress = ref(false)

watch(props, () => {
  userData.value = structuredClone(toRaw(props.userData))
})

const onFormSubmit = () => {
  emit('update:modelValue', false)
  emit('submit', userData.value)
}

const onFormReset = () => {
  userData.value = structuredClone(toRaw(props.userData))
  emit('update:isDialogVisible', false)
}

const dialogVisibleUpdate = val => {
  emit('update:isDialogVisible', val)
}
</script>

<template>
  <VDialog
    :width="$vuetify.display.smAndDown ? 'auto' : 650 "
    :model-value="props.isDialogVisible"
    @update:model-value="dialogVisibleUpdate"
  >
    <VCard class="pa-sm-9 pa-5">
      <VCardItem class="text-center">
        <VCardTitle class="text-h5">
          Edit User Information
        </VCardTitle>
        <VCardSubtitle>
          Updating user details will receive a privacy audit.
        </VCardSubtitle>
      </VCardItem>

      <VCardText>
        <!-- 👉 Form -->
        <VForm
          class="mt-6"
          @submit.prevent="onFormSubmit"
        >
          <VRow>
            <!-- 👉 Full Name -->
            <VCol
              cols="12"
              md="6"
            >
              <VTextField
                v-model="userData.fullName"
                label="Full Name"
              />
            </VCol>

            <!-- 👉 Username -->
            <VCol
              cols="12"
              md="6"
            >
              <VTextField
                v-model="userData.username"
                label="Username"
              />
            </VCol>

            <!-- 👉 Billing Email -->
            <VCol
              cols="12"
              md="6"
            >
              <VTextField
                v-model="userData.email"
                label="Billing Email"
              />
            </VCol>

            <!-- 👉 Status -->
            <VCol
              cols="12"
              md="6"
            >
              <VTextField
                v-model="userData.status"
                label="Status"
              />
            </VCol>

            <!-- 👉 Tax Id -->
            <VCol
              cols="12"
              md="6"
            >
              <VTextField
                v-model="userData.taxId"
                label="Tax Id"
              />
            </VCol>

            <!-- 👉 Contact -->
            <VCol
              cols="12"
              md="6"
            >
              <VTextField
                v-model="userData.contact"
                label="Contact"
              />
            </VCol>

            <!-- 👉 Language -->
            <VCol
              cols="12"
              md="6"
            >
              <VSelect
                v-model="userData.language"
                label="Language"
                :items="['English', 'Spanish', 'Portuguese', 'Russian', 'French', 'German']"
              />
            </VCol>

            <!-- 👉 Country -->
            <VCol
              cols="12"
              md="6"
            >
              <VSelect
                v-model="userData.country"
                label="Country"
                :items="['USA', 'UK', 'Spain', 'Russia', 'France', 'Germany']"
              />
            </VCol>

            <!-- 👉 Switch -->
            <VCol cols="12">
              <VSwitch
                v-model="isUseAsBillingAddress"
                density="compact"
                label="Use as a billing address?"
              />
            </VCol>

            <!-- 👉 Submit and Cancel -->
            <VCol
              cols="12"
              class="d-flex flex-wrap justify-center gap-4"
            >
              <VBtn type="submit">
                Submit
              </VBtn>

              <VBtn
                color="secondary"
                variant="tonal"
                @click="onFormReset"
              >
                Cancel
              </VBtn>
            </VCol>
          </VRow>
        </VForm>
      </VCardText>
    </VCard>
  </VDialog>
</template>
