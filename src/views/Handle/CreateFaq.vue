<template>
  <ContentWrap title="生成faq">
    <ElButton type="success" @click="copyData">复制代码</ElButton>
    <ElForm>
      <ElFormItem label="faq标题">
        <ElInput v-model="dynamicValidateForm.title" />
      </ElFormItem>
      <div v-for="(faq, index) in dynamicValidateForm.faqs" :key="index" class="flex gap-10">
        <ElFormItem :label="'faq问题_' + (index + 1)" class="items-start">
          <ElInput v-model="faq.question" />
        </ElFormItem>
        <ElFormItem :label="'faq回答_' + (index + 1)">
          <Editor
            api-key="agu9w7x5nhyyidzgqwlw246uf464zj6alt750mifdn5f4xaj"
            :init="{
              height: 200,
              width: 400,
              menubar: false,
              plugins: [
                'advlist',
                'autolink',
                'lists',
                'link',
                'image',
                'charmap',
                'preview',
                'anchor',
                'searchreplace',
                'visualblocks',
                'code',
                'fullscreen',
                'insertdatetime',
                'media',
                'table',
                'paste',
                'help',
                'wordcount'
              ],
              toolbar: 'undo redo | bold | link | help',
              link_default_target: '_blank',
              link_title: false,
              link_quicklink: true,
            }"
            v-model="faq.answer"
          />
        </ElFormItem>
        <ElButton type="danger" :icon="Delete" round @click="deleteFaq(index)"></ElButton>
      </div>
    </ElForm>
    <ElButton type="primary" @click="addFaq">新增faq</ElButton>
  </ContentWrap>
</template>

<script setup>
import { ref, reactive } from 'vue'
import { ElButton, ElForm, ElFormItem, ElInput, ElMessage } from 'element-plus'
import { Delete, EditPen } from '@element-plus/icons-vue'
import { ContentWrap } from '@/components/ContentWrap'
import Editor from '@tinymce/tinymce-vue'
import { useClipboard } from '@vueuse/core'

const { copy } = useClipboard()

const content = ref('<p>Hello, world!</p>')
const click = () => {
  console.log(dynamicValidateForm)
}
const formRef = ref(null)

const dynamicValidateForm = reactive({
  faqs: [
    {
      question: '',
      answer: ''
    }
  ],
  title: ''
})
const addFaq = () => {
  dynamicValidateForm.faqs.push({
    question: '',
    answer: ''
  })
}
const deleteFaq = (index) => {
  dynamicValidateForm.faqs.splice(index, 1)
}
const copyData = () => {
  copy(JSON.stringify(dynamicValidateForm))
  ElMessage({
    message: '复制成功！',
    type: 'success'
  })
}
</script>
