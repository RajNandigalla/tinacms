# react-tina-editor

The documentation details the architecture of react-tina-editor, it is a wysiwyg editor built using [ReactJS](https://reactjs.org/) and [Prosemirror](https://prosemirror.net/).

## High level architecture

The editor components can be found [here](https://github.com/tinacms/tinacms/tree/master/packages/react-tinacms-editor/src/components). There are mainly 3 editing components:

1. [Wysiwyg editor](https://github.com/tinacms/tinacms/tree/master/packages/react-tinacms-editor/src/components/Wysiwyg): this editor is root component, it maintains information about editing mode (wysiwyg or markdow) and accordingly initialise correct editor. It makes editor mode information available at [editorMode](https://github.com/tinacms/tinacms/blob/master/packages/react-tinacms-editor/src/context/editorMode.tsx) context, thus this information is available to all child components and they can change their behaviour depending on currently selected editing mode.

2. [Prosemirror editor](https://github.com/tinacms/tinacms/tree/master/packages/react-tinacms-editor/src/components/ProsemirrorEditor): this is component that provides rich text editing capabilities. It initialises a prosemirror editor object and makes it available in [editorState](https://github.com/tinacms/tinacms/blob/master/packages/react-tinacms-editor/src/context/editorState.tsx) context for child components to consume.

3. [Markdown editor](https://github.com/tinacms/tinacms/tree/master/packages/react-tinacms-editor/src/components/MarkdownEditor): this component provides markdown editing cpability.

[Translator](https://github.com/tinacms/tinacms/tree/master/packages/react-tinacms-editor/src/translator) package is where lies the code to convert editor content to other formats like Markdown or HTML. This is also used while toggling between wysiwyg and plain text mode of editing.

![](https://i.imgur.com/5Ip2rSu.png)

### Prosemirror Editor

[This](https://github.com/tinacms/tinacms/blob/master/packages/react-tinacms-editor/src/components/ProsemirrorEditor/index.tsx) is most complex part of the package. It includes an editing area which is simply a `div` and a more complex MenuBar. The component builds prosemirror editor passing the `div` as DOM node for mounting the editor. Prosemirror state is than passed to [editorState](https://github.com/tinacms/tinacms/blob/master/packages/react-tinacms-editor/src/context/editorState.tsx) context and is available for use in all child components including menu items and popups.

![](https://i.imgur.com/0HLqZRY.jpg)

##### Editor Plugins

[Here](https://github.com/tinacms/tinacms/tree/master/packages/react-tinacms-editor/src/plugins) are the various plugins used by the editor. Each plugin groups a complete functionality, for instance [image](https://github.com/tinacms/tinacms/tree/master/packages/react-tinacms-editor/src/plugins/Image) plugin has all react component, prosemirror classes, node views, etc required to support image functionality in the editor. Here is the complete list of the plugins:

1. [Block](https://github.com/tinacms/tinacms/tree/master/packages/react-tinacms-editor/src/plugins/Block)
2. [Blockquote](https://github.com/tinacms/tinacms/tree/master/packages/react-tinacms-editor/src/plugins/Blockquote)
3. [CodeBlock](https://github.com/tinacms/tinacms/tree/master/packages/react-tinacms-editor/src/plugins/CodeBlock)
4. [Common](https://github.com/tinacms/tinacms/tree/master/packages/react-tinacms-editor/src/plugins/Common)
5. [History](https://github.com/tinacms/tinacms/tree/master/packages/react-tinacms-editor/src/plugins/History)
6. [Image](https://github.com/tinacms/tinacms/tree/master/packages/react-tinacms-editor/src/plugins/Image)
7. [Inline](https://github.com/tinacms/tinacms/tree/master/packages/react-tinacms-editor/src/plugins/Inline)
8. [Link](https://github.com/tinacms/tinacms/tree/master/packages/react-tinacms-editor/src/plugins/Link)
9. [List](https://github.com/tinacms/tinacms/tree/master/packages/react-tinacms-editor/src/plugins/List)
10. [Table](https://github.com/tinacms/tinacms/tree/master/packages/react-tinacms-editor/src/plugins/Table)

Typical structure of a plugin:

- Menu
  - MarkdownMenu
  - ProsemirrorMenu
- Popups
  - MyPopup,...
- nodeView
- plugin

`Menu` folder has 2 components `MarkdownMenu` and `ProsemirrorMenu`, if present these are added to menubar in Markdown mode and Prosemirror mode respectively.

`Popups` folder has various Popup components. For instance image and link have Popup components used for editing. These components should take care for show / hide themselves conditionally.

`nodeView` is used to create customized vide for prosemirror node, this is used for code block and image nodes.

`plugin` is prosemirror [plugin](https://prosemirror.net/docs/ref/#state.Plugin_System) object. It helps maintain state for plugin for instance for links and images it helps to keep information when user is currently editing link or image. It also enables other functionality like attaching event listeners, creation editor decorations, etc.

### Markdown Editor

[This](https://github.com/tinacms/tinacms/tree/master/packages/react-tinacms-editor/src/components/MarkdownEditor) editor is simple textarea where editor content can be edited a plain text. Currently menubar options are not enabled in this editor.

### Menubar

Markdown editor and Prosemirror editor have their own MenuBar component which in turn uses [BaseMenubar](https://github.com/tinacms/tinacms/tree/master/packages/react-tinacms-editor/src/components/BaseMenubar) component.

## Translator

[Translator](https://github.com/tinacms/tinacms/tree/master/packages/react-tinacms-editor/src/translator) helps in converting content into different formats - JSON, Markdown, HTML.

- [HTML Translator](https://github.com/tinacms/tinacms/tree/master/packages/react-tinacms-editor/src/translator/DOMTranslator): this is built using prosemirror provided api.
- [Markdown Translator](https://github.com/tinacms/tinacms/tree/master/packages/react-tinacms-editor/src/translator/MarkdownTranslator): this is built using [markdown-it](https://github.com/markdown-it/markdown-it) library.
