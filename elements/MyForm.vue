<script setup lang="ts">

  import type {IForm} from "~/types/types";
  import { z } from "zod";
  import type {FormSubmitEvent} from "#ui/types";

  const defaultState = {
    title: '',
    desc: ''
  }

  const emit = defineEmits(['newTodo'])
  const state = reactive<IForm>(defaultState)
  const schema = z.object({
    title: z.string({
      required_error: "Title is required",
      invalid_type_error: "Title must be a string",
    })
        .min(3, 'Min 3 characters').trim(),
    desc: z.string({
      required_error: "Description is required",
      invalid_type_error: "Description must be a string",
    })
        .min(3, 'Min 3 characters'),
  }).required()

  type Schema = z.output<typeof schema>

  const addTodo = async (event: FormSubmitEvent<Schema>) => {

    emit('newTodo', event.data)
    console.log(event)
    state.title = ''
    state.desc = ''
  }

</script>

<template>
  <div class="form">
    <UForm :schema="schema" :state="state" @submit="addTodo">
      <div class="form_field">
        <UFormGroup label="Title" name="title">
          <UInput v-model="state.title" required/>
        </UFormGroup>

        <UFormGroup label="Description" name="desc">
          <UTextarea v-model="state.desc" required/>
        </UFormGroup>
      </div>

      <UButton
          label="ADD"
          color="teal"
          variant="outline"
          @submit.stop.prevent
          type="submit"
      />
    </UForm>
  </div>
</template>

<style scoped>
  .form{
    .form_field {
      @apply grid gap-2 mb-2;
    }
  }
</style>