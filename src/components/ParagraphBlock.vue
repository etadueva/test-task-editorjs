<template>
  <div id="editorjs"></div>
</template>

<script>
  import EditorJS from "@editorjs/editorjs";
  import Paragraph from "@editorjs/paragraph";

  export default {
    data() {
      return {
        editor: null,
        paragraphBlock: {
          type: "paragraph",
          data: {
            text: "Введите текст"
          }
        }
      }
    },
    methods: {
      initEditor() {
        this.editor = new EditorJS({
          holder: "editorjs",
          autofocus: true,
          tools: {
            paragraph: {
              class: Paragraph,
              inlineToolbar: true
            }
          },
          data: {
            blocks: [this.paragraphBlock]
          },
          onChange: () => {
            this.editor.save().then((outputData) => {
              const jsonData = JSON.stringify(outputData);
              console.log(jsonData);
            });
          },
          onReady: () => {
            console.log("Ready");
          }
        });
      },
      addBlock(type) {
        let newBlock;
        switch (type) {
          case "paragraph":
            newBlock = this.paragraphBlock;
            break;
          default:
            return;
        }
        const insertedBlock = this.editor.blocks.insert(newBlock);
        this.selectedBlockIndex = insertedBlock.index;
      }
    },
    mounted() {
      this.initEditor();
    }
  };
</script>