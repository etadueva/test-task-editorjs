<template>
  <div id="editorjs"></div>
</template>

<script>
  import EditorJS from "@editorjs/editorjs";
  import Paragraph from "@editorjs/paragraph";
  import ImageTool from "@editorjs/image";
  import NestedList from '@editorjs/nested-list';
  import Table from '@editorjs/table';
  const Header = require('@editorjs/header');
  const Marker = require('@editorjs/marker');
  const LinkTool = require('@editorjs/link');
  const Warning = require('@editorjs/warning');
  const Checklist = require('@editorjs/checklist');

  export default {
    data() {
      return {
        editor: null,
        paragraphBlock: {
          type: "paragraph",
          data: {
            text: "Введите текст"
          }
        },
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
            paragraph: {
              class: Paragraph,
              inlineToolbar: true
            },
            image: {
              class: ImageTool,
              config: {
                uploader: {
                  uploadByFile(file) {
                    // upload image file here and return its url
                    return Promise.resolve("https://via.placeholder.com/150");
                  },
                  uploadByUrl(url) {
                    // upload image by url here and return its url
                    return Promise.resolve("https://via.placeholder.com/150");
                  }
                }
              }
            },
            list: {
              class: NestedList,
              inlineToolbar: true,
              config: {
                defaultStyle: 'unordered'
              },
            },
            header: {
              class: Header,
              shortcut: 'CMD+SHIFT+H',
            },
            Marker: {
              class: Marker,
              shortcut: 'CMD+SHIFT+M',
            },
            linkTool: {
              class: LinkTool,
              config: {
                endpoint: 'http://localhost:8008/fetchUrl', // Your backend endpoint for url data fetching,
              }
            },
            table: {
              class: Table,
              inlineToolbar: true,
              config: {
                rows: 2,
                cols: 3,
              },
            },
            warning: {
              class: Warning,
              inlineToolbar: true,
              shortcut: 'CMD+SHIFT+W',
              config: {
                titlePlaceholder: 'Title',
                messagePlaceholder: 'Message',
              },
            },
            checklist: {
              class: Checklist,
              inlineToolbar: true,
            },
          },
          data: {
            blocks: [this.paragraphBlock, this.imageBlock]
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
          case "image":
            newBlock = this.imageBlock;
            break;
          default:
            return;
        }
        const insertedBlock = this.editor.blocks.insert(newBlock);
        this.selectedBlockIndex = insertedBlock.index;
        this.editor.blocks.setMovable(0, false);
        this.editor.blocks.setMovable(1, false);
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