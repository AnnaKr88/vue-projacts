<script setup lang="ts">
  import type {ITodoList} from "~/types/types";
  import {z} from "zod";
  import type {FormSubmitEvent} from "#ui/types";

  interface Props {
    data: ITodoList,
    index: number,
  }

  const updateTodo = ref(false)
  const props = defineProps<Props>()
  const oldValue = ref({
    id: props.data.id,
    title: props.data.title,
    desc: props.data.desc,
  })
  const newValue = ref(props.data)
  const emit = defineEmits(['deleteEl', 'updateEl', 'changeEl'])

  const schema = z.object({
    title: z.string({
      required_error: "Title is required",
      invalid_type_error: "Title must be a string",
    })
        .min(2, 'Min 2 characters').trim(),
    desc: z.string({
      required_error: "Description is required",
      invalid_type_error: "Description must be a string",
    })
        .min(3, 'Min 3 characters'),
  }).required()


  type Schema = z.output<typeof schema>

  const deleteEl = (e: ITodoList['id']) => {
    emit('deleteEl', e)
  }

  const updateEl = (e: ITodoList) => {
    emit('updateEl', e)
    updateTodo.value = !updateTodo.value
  }

  const changeEl = (e: FormSubmitEvent<Schema>) => {
    emit('changeEl', e)
    console.log(e.data)
    updateTodo.value = !updateTodo.value
  }

  const back = () => {
    console.log(oldValue.value)
    updateTodo.value = !updateTodo.value

  }

</script>

<template>
  <div class="todo_el">
    <template v-if="updateTodo">
        <div class="full">
          <UForm :schema="schema" :state="oldValue" @submit="changeEl" class="form">
            <div class="form_field">
              <UFormGroup label="Title" name="title">
                <UInput
                    :value="oldValue.title"
                />
              </UFormGroup>

              <UFormGroup label="Description" name="desc">
                <UTextarea
                    :value="oldValue.desc"
                />
              </UFormGroup>
            </div>

            <div class="el_buttons">
              <UButton
                  @submit.stop.prevent
                  type="submit"
                  label="Update"
              />
              <UButton
                  @click="deleteEl(oldValue.id)"
                  label="Delete"
              />
              <UButton
                  @click="back"
                  label="Back"
              />
            </div>
          </UForm>
        </div>
    </template>
    <template v-else>
      <div class="el">
        <span><strong class="text-xl">{{ index+1 }}. </strong>ID: {{ data.id }}</span>
        <span>Title: {{ data.title }}</span>
        <span>Description: {{ data.desc }}</span>
      </div>
      <div class="el_buttons">
        <UButton
            @click="updateEl(data)"
            label="Update"
        />
        <UButton
            @click="deleteEl(data.id)"
            label="Delete"
        />
      </div>
    </template>
  </div>


</template>

<style scoped>
.todo_el{
  @apply flex justify-between gap-1 border-2 border-green-900 p-2;

  .full{
    @apply w-full;

    .form{
        @apply flex gap-2 justify-between;

      &_field{
        @apply w-4/5;
      }
      }
  }

  .el {
    @apply grid gap-2;

    &_buttons {
      @apply grid gap-2;

      button {
        @apply justify-center uppercase;
      }

    }
  }

}

</style>