<template>
  <div class="container">
    <div class="quill-editor"
         :content="content"
         @change="onEditorChange($event)"
         @blur="onEditorBlur($event)"
         @focus="onEditorFocus($event)"
         @ready="onEditorReady($event)"
         v-quill:myQuillEditor="editorOption">
    </div>
  </div>
</template>
<script>
export default {
  layout: 'wakagi',
  data() {
    return {
      content: '<p>I am Example</p>',
      editorOption: {
        // some quill options
        modules: {
          toolbar: [
            ['bold', 'italic', 'underline', 'strike'],
            ['blockquote', 'code-block'],
            [{ 'header': 1 }, { 'header': 2 }],
          ]
        }
      }
    };
  },
  head: {
    title: "新加文章",
    meta: [
      {
        hid: "description",
        name: "description",
        content:
          "新加文章",
      },
    ],
  },
  mounted() {
  },
  methods: {
    onEditorBlur(editor) {
      console.log('editor blur!', editor)
    },
    onEditorFocus(editor) {
      console.log('editor focus!', editor)
    },
    onEditorReady(editor) {
      console.log('editor ready!', editor)
    },
    onEditorChange({ editor, html, text }) {
      console.log('editor change!', editor, html, text)
      this.content = html
    }
  },
};
</script>

<style scoped>
</style>
