<template>
  <div class="modal-wrapper">
    <div class="modal">
      <div class="title-wrapper">
        <h1 v-if="!isTitleEditing" class="modal-title" @click="onTitleFocus">{{ title }}</h1>
        <input
          v-else
          class="modal-title"
          placeholder="Title of the task can not be empty"
          type="text"
          v-model="title"
          ref="modalTitleInput"
          @keypress.enter="onTitleBlur"
          @blur="onTitleBlur"
        />
      </div>
      <div class="modal-body">
        <p>Description:</p>
        <textarea class="modal-area" v-model="description"></textarea>
        <span class="area-counter">{{ description.length }}</span>
      </div>
      <div class="modal-buttons">
        <button class="btn btn-accept" @click="onAccept">Accept</button>
        <button class="btn btn-cancel" @click="onCancel">Cancel</button>
      </div>
    </div>
  </div>
</template>

<script lang="ts">
import { UPDATE_TASK } from '@/constants'
import { Task } from '@/types'
import { Component, Prop, Vue } from 'vue-property-decorator'

@Component
export default class extends Vue {
  @Prop({ required: true }) readonly task!: Task
  @Prop({ required: true }) readonly cardId!: string
  private title = this.task.title
  private description = this.task.description || ''
  private isTitleEditing = false

  private onAccept(): void {
    if (this.title !== this.task.title || this.description !== this.task.description) {
      const updatedTask = { ...this.task }
      if (this.title.trim()) {
        updatedTask.title = this.title
      }
      if (this.description !== null) {
        updatedTask.description = this.description
      }
      this.$store.dispatch(UPDATE_TASK, { updatedTask, cardId: this.cardId })
    }
    this.onCancel()
  }

  private onCancel(): void {
    this.$emit('on-cancel')
  }

  private onTitleFocus(): void {
    this.isTitleEditing = true
    const timeoutID = setTimeout(() => {
      // @ts-expect-error
      this.$refs.modalTitleInput.focus()
      clearTimeout(timeoutID)
    }, 0)
  }

  private onTitleBlur(): void {
    this.isTitleEditing = false
    if (!this.title.trim()) {
      this.title = this.task.title
    }
  }
}
</script>

<style scoped>
.modal-wrapper {
  position: absolute;
  display: flex;
  justify-content: center;
  align-items: center;
  top: 0;
  left: 0;
  width: 100%;
  height: 100%;
  min-height: 100vh;
  background-color: rgba(0, 0, 0, 0.98);
  z-index: 1;
}

.modal {
  position: relative;
  padding: 30px;
  width: 500px;
  min-height: 300px;
  max-height: 700px;
  background-color: #eee;
  border-radius: 25px;
}

.modal-title {
  text-align: center;
  font-size: 2.4rem;
}

h1.modal-title:hover {
  background: #ccc;
  border-radius: 5px;
}

.title-wrapper {
  display: flex;
  justify-content: center;
}

.title-wrapper input {
  display: block;
  border: 1px solid #ccc;
  border-radius: 10px;
}

.title-wrapper input::placeholder {
  font-size: 1.4rem;
}

.modal-body p {
  margin: 1rem;
  font-size: 1.8rem;
}

.modal-area {
  min-width: 100%;
  max-width: 100%;
  min-height: 150px;
  max-height: 400px;
  font-size: 1.6rem;
  padding: 10px 20px;
}

.area-counter {
  display: inline-block;
  width: 100%;
  text-align: right;
  font-size: 0.8rem;
  color: darkslategray;
}

.modal-buttons {
  margin-top: 2rem;
  width: 250px;
  display: flex;
  justify-content: space-between;
}

.btn {
  background: transparent;
  border: 3px solid black;
  border-radius: 25px;
  min-width: 120px;
  font-size: 1.6rem;
  font-weight: 700;
  letter-spacing: 1px;
  padding: 0.5rem 1.5rem;
  cursor: pointer;
  user-select: none;
}

.btn-accept {
  border: 3px solid green;
  color: green;
}

.btn-cancel {
  border: 3px solid #aaa;
  background-color: #aaa;
}
</style>
