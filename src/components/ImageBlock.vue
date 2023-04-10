<template>
  <div id="editorjs"></div>
</template>

<script>
  import EditorJS from "@editorjs/editorjs";
  import ImageTool from "@editorjs/image";

  export default {
    data() {
      return {
        editor: null,
        imageBlock: {
          type: "image",
          data: {
            url: "https://via.placeholder.com/150"
          }
        },
        selectedBlockIndex: null
      }
    },
    methods: {
      initEditor() {
        this.editor = new EditorJS({
          holder: "editorjs",
          autofocus: true,
          tools: {
            image: {
              class: ImageTool,
              config: {
                uploader: {
                  uploadByFile(file) {
                    return Promise.resolve("https://via.placeholder.com/150");
                  },
                  uploadByUrl(url) {
                    return Promise.resolve("https://via.placeholder.com/150");
                  }
                }
              }
            },
            list: {
              class: List,
              inlineToolbar: true,
              config: {
                defaultStyle: 'unordered'
              }
            },
          },
          data: {
            blocks: [this.imageBlock]
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
          case "image":
            newBlock = this.imageBlock;
            break;
          default:
            return;
        }
        const insertedBlock = this.editor.blocks.insert(newBlock);
        this.selectedBlockIndex = insertedBlock.index;
      },
      moveBlock(blockIndex, direction) {
        const currentBlock = this.editor.blocks.getBlockByIndex(blockIndex);
        const targetIndex = direction === "up" ? blockIndex - 1 : blockIndex + 1;
        const targetBlock = this.editor.blocks.getBlockByIndex(targetIndex);
        this.editor.blocks.move(currentBlock, targetBlock);
        this.selectedBlockIndex = targetBlock.index;
      }
    },
    mounted() {
      this.initEditor();
    }
  };
</script>