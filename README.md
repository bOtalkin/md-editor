# md-editor-v3

Markdown 编辑器，基于 vue3，使用 jsx 语法开发，支持在 tsx 项目使用。（待优化发布）

## 预览图

默认模式下：

![](https://p9-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/65918d53d93b492ca51de2f36e439d83~tplv-k3u1fbpfcp-watermark.image)

暗黑模式下：

![](https://p1-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/012fb26afac745a79f6d5029de3ecd2b~tplv-k3u1fbpfcp-watermark.image)

## apis

### props

| 名称 | 类型 | 默认值 | 说明 |
| --- | --- | --- | --- |
| modelValue | String | '' | md 编辑内容 |
| editorClass | String | '' | 编辑器最外层样式 |
| hljs | Object | null | 项目中使用到了 highlight，可将实例直接传递，生产环境则不会请求 CDN，需要手动导入支持的高亮代码样式 |
| highlightJs | String | [highlight.js](https://cdn.bootcdn.net/ajax/libs/highlight.js/11.0.1/highlight.min.js) | highlightJs CDN |
| highlightCss | String | [atom-one-dark](https://cdn.bootcdn.net/ajax/libs/highlight.js/11.0.1/styles/atom-one-dark.min.css) | 预览高亮代码样式 |
| historyLength | Number | 10 | 最大记录操作数（太大会占用内存） |
| pageFullScreen | Boolean | false | 浏览器内全屏 |
| preview | Boolean | true | 预览模式 |
| html | Boolean | false | html 预览 |

### 事件绑定

| 名称 | 入参 | 说明 |
| --- | --- | --- |
| onChange | v:String | 内容变化事件（当前与`textare`的`oninput`事件绑定，每输入一个单字即会触发） |
| onSave | v:String | 保存事件，快捷键与保存按钮均会触发 |
| onUploadImg | files:FileList, callback:Function | 上传图片事件，弹窗会等待上传结果，务必将上传后的 urls 作为 callback 入参回传 |

### 快捷键

主要以`CTRL`搭配对应功能英文单词首字母，冲突项添加`SHIFT`，再冲突替换为`ALT`。

| 键位             | 功能       | 说明                             | 开发标记 |
| ---------------- | ---------- | -------------------------------- | -------- |
| CTRL + S         | 保存       | 触发编辑器的`onSave`回调         | √        |
| CTRL + B         | 加粗       | `**加粗**`                       | √        |
| CTRL + U         | 下划线     | `<u>下划线</u>`                  | √        |
| CTRL + I         | 斜体       | `*斜体*`                         | √        |
| CTRL + 1-6       | 1-6 级标题 | `# 标题`                         | √        |
| CTRL + ↑         | 上角标     | `<sup>上角标</sup>`              | √        |
| CTRL + ↓         | 下角标     | `<sub>下角标</sub>`              | √        |
| CTRL + Q         | 引用       | `> 引用`                         | √        |
| CTRL + O         | 有序列表   | `1. 有序列表`                    | √        |
| CTRL + L         | 链接       | `[链接](https://imbf.cc)`        | √        |
| CTRL + T         | 表格       | `\|表格\|` 放弃开发（无法实现）  | x        |
| CTRL + Z         | 撤回       | 触发编辑器内内容撤回，与系统无关 | √        |
| CTRL + SHIFT + S | 删除线     | `~删除线~`                       | √        |
| CTRL + SHIFT + U | 无序列表   | `- 无序列表`                     | √        |
| CTRL + SHIFT + C | 块级代码   | 多行代码块                       | √        |
| CTRL + SHIFT + I | 图片链接   | `![图片](https://imbf.cc)`       | √        |
| CTRL + SHIFT + Z | 前进一步   | 触发编辑器内内容前进，与系统无关 | √        |
| CTRL + ALT + C   | 行内代码   | 行内代码块                       | √        |

## 演示

### jsx 语法项目

```js
import { defineComponent, reactive } from 'vue';
import Editor from '名称待定';
import hljs from 'highlight.js';
import 'highlight.js/styles/atom-one-dark.css';

export default defineComponent({
  setup() {
    const md = reactive({
      text: 'default markdown content'
    });
    return () => (
      <Editor hljs={hljs} modelValue={md.text} onChange={(value) => (md.text = value)} />
    );
  }
});
```

### vue 模板项目

```js
<template>
  <editor v-model="text" pageFullScreen></editor>
</template>

<script>
import { defineComponent } from 'vue';
import Editor from '名称待定';

export default defineComponent({
  name: 'VueTemplateDemo',
  components: { Editor },
  data() {
    return {
      text: '默认值'
    };
  }
});
</script>

```
