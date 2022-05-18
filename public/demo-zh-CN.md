## 😁 基本使用示例

目前一直在迭代开发，所以尽量安装最新版本。发布日志请前往：[releases](https://github.com/imzbf/md-editor-v3/releases)

### 🤖 安装

```shell
yarn add md-editor-v3
```

目前 vue3 已经能很友好的使用 jsx 来开发了，对于一些爱好者（比如作者本身），需要考虑兼容一下。

两种方式开发上区别在于**vue 模板**能很好的支持`vue`特性，比如指令，内置的双向绑定等；而**jsx 语法**更偏向于`react`的理念，开发环境来讲 jsx 如果在支持 ts 的环境下，会更友好一些。

### 🤓 全局引用

通过直接链接生产版本来使用，下面是一个小例子：

```html
<!DOCTYPE html>
<html lang="zh-CN">
  <head>
    <meta charset="UTF-8" />
    <title>全局引用</title>
    <link
      href="https://cdn.jsdelivr.net/npm/md-editor-v3@${EDITOR_VERSION}/lib/style.css"
      rel="stylesheet"
    />
  </head>
  <body>
    <div id="md-editor-v3">
      <md-editor-v3 v-model="text" />
    </div>
    <script src="https://cdn.jsdelivr.net/npm/vue@3.1.5/dist/vue.global.prod.min.js"></script>
    <script src="https://cdn.jsdelivr.net/npm/md-editor-v3@${EDITOR_VERSION}/lib/md-editor-v3.umd.js"></script>
    <script>
      const App = {
        data() {
          return {
            text: 'Hello Editor!!'
          };
        }
      };
      Vue.createApp(App).use(MdEditorV3).mount('#md-editor-v3');
    </script>
  </body>
</html>
```

### 🥱 Setup 模板

```vue
<template>
  <md-editor v-model="text" />
</template>

<script setup>
import { ref } from 'vue';
import MdEditor from 'md-editor-v3';
import 'md-editor-v3/lib/style.css';

const text = ref('Hello Editor!');
</script>
```

### 🤗 Jsx 模板

```js
import { defineComponent, ref } from 'vue';
import MdEditor from 'md-editor-v3';
import 'md-editor-v3/lib/style.css';

export default defineComponent({
  name: 'MdEditor',
  setup() {
    const text = ref('');
    return () => (
      <MdEditor modelValue={text.value} onChange={(v: string) => (text.value = v)} />
    );
  }
});
```

## 🥂 扩展功能

这里包含了一些编辑器`api`的使用示范

### 🍦 主题切换

主题分为了编辑器主题（`theme`，称为全局主题）、预览内容主题（`previewTheme`）和块级代码主题（`codeTheme`），他们都支持响应式更新，而非只能预设。

#### 🍧 编辑器主题

支持默认和暗夜模式两种

```vue
<template>
  <md-editor v-model="state.text" :theme="state.theme" />
</template>

<script setup>
import { reactive } from 'vue';
import MdEditor from 'md-editor-v3';
import 'md-editor-v3/lib/style.css';

const state = reactive({
  text: '',
  theme: 'dark'
});
</script>
```

#### 🍡 预览主题

内置了`default`、`github`、`vuepress`、`mk-cute`、`smart-blue`、`cyanosis`6 种主题，在一些直接预览文档内容时使用。并且支持在线切换（修改`previewTheme`即可）和自行扩展。

- 切换内置

  ```vue
  <template>
    <md-editor v-model="state.text" :preview-theme="state.theme" />
  </template>

  <script setup>
  import { defineComponent } from 'vue';
  import MdEditor from 'md-editor-v3';
  import 'md-editor-v3/lib/style.css';

  const state = reactive({
    text: '',
    theme: 'cyanosis'
  });
  </script>
  ```

- 自定义

  1. 先以`xxx-theme`为类名，定义你的主题`css`，xxx 是主题名称，具体的内容参考[markdown-theme](https://github.com/imzbf/markdown-theme)

  _xxx.css_

  ```css
  .xxx-theme code {
    color: red;
  }
  ```

  2. 全局引入

  ```js
  import 'xxx.css';
  ```

  3. 设置`previewTheme`为 xxx

  ```html
  <md-editor preview-theme="xxx" />
  ```

#### 🎄 代码主题

内置了`atom`、`a11y`、`github`、`gradient`、`kimbie`、`paraiso`、`qtcreator`、`stackoverflow`代码主题，均来至[highlight.js](https://highlightjs.org/)

- 切换内置

  ```vue
  <template>
    <md-editor v-model="state.text" :code-theme="state.theme" />
  </template>

  <script setup>
  import { reactive } from 'vue';
  import MdEditor from 'md-editor-v3';
  import 'md-editor-v3/lib/style.css';

  const state = reactive({
    text: '',
    theme: 'atom'
  });
  </script>
  ```

- 自定义

  1. 找到你喜欢的代码主题，最好支持暗夜模式

  ```js
  import MdEditor from 'md-editor-v3';

  MdEditor.config({
    editorExtensions: {
      highlight: {
        css: {
          xxxxx: {
            light: 'https://unpkg.com/highlight.js@11.2.0/styles/xxxxx-light.css',
            dark: 'https://unpkg.com/highlight.js@11.2.0/styles/xxxxx-dark.css'
          },
          yyyyy: {
            light: 'https://unpkg.com/highlight.js@11.2.0/styles/xxxxx-light.css',
            dark: 'https://unpkg.com/highlight.js@11.2.0/styles/xxxxx-dark.css'
          }
        }
      }
    }
  });
  ```

  你可以通过将`css`的`key`设置为内置名称来覆盖内置的链接。

  2. 设置`codeTheme`

  ```html
  <md-editor code-theme="xxxxx" />
  ```

### 🛠 扩展库替换

highlight、prettier、cropper、screenfull 均使用外链引入，在无外网的时候，部分可将项目中已安装的依赖传入，也可以使用下载好的引用。

演示替换`screenfull`

```vue
<template>
  <md-editor v-model="text" />
</template>

<script setup>
import { ref } from 'vue';
// 引用screenfull
import screenfull from 'screenfull';
import MdEditor from 'md-editor-v3';
import 'md-editor-v3/lib/style.css';

MdEditor.config({
  editorExtensions: {
    screenfull: {
      instance: screenfull
    }
  }
});

const text = ref('');
</script>
```

#### 📡 内网链接

对应的 js 文件可以去[unpkg.com](https://unpkg.com)，直接找到对应的文件下载即可。

演示替换`screenfull`

```vue
<template>
  <md-editor v-model="text" />
</template>

<script setup>
import { ref } from 'vue';
import MdEditor from 'md-editor-v3';
import 'md-editor-v3/lib/style.css';

MdEditor.config({
  editorExtensions: {
    screenfull: {
      js: 'https://localhost:8090/screenfull@6.0.1/index.js'
    }
  }
});

const text = ref('');
</script>
```

### 📷 图片上传

默认可以选择多张图片，支持截图粘贴板上传图片，支持复制网页图片粘贴上传。

> 注意：粘贴板上传时，如果是网页上的 gif 图，无法正确上传为 gif 格式！请保存本地后再手动上传。

```vue
<template>
  <md-editor v-model="text" @onUploadImg="onUploadImg" />
</template>

<script setup>
import { ref } from 'vue';
import MdEditor from 'md-editor-v3';
import 'md-editor-v3/lib/style.css';

const text = ref('# Hello Editor');

const onUploadImg = async (files, callback) => {
  const res = await Promise.all(
    files.map((file) => {
      return new Promise((rev, rej) => {
        const form = new FormData();
        form.append('file', file);

        axios
          .post('/api/img/upload', form, {
            headers: {
              'Content-Type': 'multipart/form-data'
            }
          })
          .then((res) => rev(res))
          .catch((error) => rej(error));
      });
    })
  );

  callback(res.map((item) => item.data.url));
};
</script>
```

### 🏳️‍🌈 语言扩展与替换

```vue
<template>
  <md-editor v-model="state.text" :language="state.language" />
</template>

<script setup>
import { reactive } from 'vue';
import MdEditor from 'md-editor-v3';
import 'md-editor-v3/lib/style.css';

MdEditor.config({
  markedOptions: {
    languageUserDefined: {
      'my-lang': {
        toolbarTips: {
          bold: '加粗',
          underline: '下划线',
          italic: '斜体',
          strikeThrough: '删除线',
          title: '标题',
          sub: '下标',
          sup: '上标',
          quote: '引用',
          unorderedList: '无序列表',
          orderedList: '有序列表',
          codeRow: '行内代码',
          code: '块级代码',
          link: '链接',
          image: '图片',
          table: '表格',
          mermaid: 'mermaid图',
          katex: '公式',
          revoke: '后退',
          next: '前进',
          save: '保存',
          prettier: '美化',
          pageFullscreen: '浏览器全屏',
          fullscreen: '屏幕全屏',
          preview: '预览',
          htmlPreview: 'html代码预览',
          catalog: '目录',
          github: '源码地址'
        },
        titleItem: {
          h1: '一级标题',
          h2: '二级标题',
          h3: '三级标题',
          h4: '四级标题',
          h5: '五级标题',
          h6: '六级标题'
        },
        imgTitleItem: {
          link: '添加链接',
          upload: '上传图片',
          clip2upload: '裁剪上传'
        },
        linkModalTips: {
          title: '添加',
          descLable: '链接描述：',
          descLablePlaceHolder: '请输入描述...',
          urlLable: '链接地址：',
          UrlLablePlaceHolder: '请输入链接...',
          buttonOK: '确定'
        },
        clipModalTips: {
          title: '裁剪图片上传',
          buttonUpload: '上传'
        },
        copyCode: {
          text: '复制代码',
          successTips: '已复制！',
          failTips: '复制失败！'
        },
        mermaid: {
          flow: '流程图',
          sequence: '时序图',
          gantt: '甘特图',
          class: '类图',
          state: '状态图',
          pie: '饼图',
          relationship: '关系图',
          journey: '旅程图'
        },
        katex: {
          inline: '行内公式',
          block: '块级公式'
        }
      }
    }
  }
});

const state = reactive({
  text: '',
  // 定义语言名称
  language: 'my-lang'
});
</script>
```

### 🛬 自定义目录结构

需求：在标题中存在外链时，点击打开新窗口。

实现：

```vue
<template>
  <md-editor v-model="text" :marked-heading-id="getId" />
</template>

<script setup>
import { ref } from 'vue';
import MdEditor from 'md-editor-v3';
import 'md-editor-v3/lib/style.css';

const text = ref('');

const getId = (text, level, raw) => {
  return `${level}-text`;
};

MdEditor.config({
  markedRenderer(renderer) {
    renderer.heading = (text, level, raw) => {
      // 你不能直接调用默认的markedHeadingId，但是它很简单
      // 如果你的id与raw不相同，请一定记得将你的生成方法通过markedHeadingId告诉编辑器
      // 否则编辑器默认的目录定位功能无法正确使用
      const id = getId(text, level, raw);

      if (/<a.*>.*<\/a>/.test(text)) {
        return `<h${level} id="${id}">${text.replace(
          /(?<=\<a.*)>(?=.*<\/a>)/,
          ' target="_blank">'
        )}</h${level}>`;
      } else if (text !== raw) {
        return `<h${level} id="${id}">${text}</h${level}>`;
      } else {
        return `<h${level} id="${id}"><a href="#${id}">${raw}</a></h${level}>`;
      }
    };

    return renderer;
  }
});
</script>
```

### 📄 目录获取与展示

- 获取

  ```vue
  <template>
    <md-editor v-model="text" @onGetCatalog="onGetCatalog" />
  </template>

  <script setup>
  import { reactive } from 'vue';
  import MdEditor from 'md-editor-v3';
  import 'md-editor-v3/lib/style.css';

  const state = reactive({
    text: '',
    catalogList: []
  });

  const onGetCatalog = (list) => {
    state.catalogList = list;
  };
  </script>
  ```

- 展示

  使用内置`MdCatalog`组件

  ```vue
  <template>
    <md-editor
      v-model="state.text"
      :editorId="state.id"
      :theme="state.theme"
      preview-only
    />
    <md-atalog :editorId="state.id" :scrollElement="scrollElement" :theme="state.theme" />
  </template>

  <script setup>
  import { reactive } from 'vue';
  import MdEditor from 'md-editor-v3';
  import 'md-editor-v3/lib/style.css';

  const state = reactive({
    theme: 'dark',
    text: '标题',
    id: 'my-editor'
  });

  const scrollElement = document.documentElement;
  </script>
  ```

### 🪚 调整工具栏

从`v1.6.0`开始，支持调整工具栏内容顺序和分割符了。

```vue
<template>
  <md-editor v-model="text" :toolbars="toolbars" />
</template>

<script setup>
import MdEditor from 'md-editor-v3';
import 'md-editor-v3/lib/style.css';

const toolbars = ['italic', 'underline', '-', 'bold', '=', 'github'];
</script>
```

### 💪 自定义工具栏

这里包含了`mark`标记扩展普通工具栏和`emoji`扩展下拉工具栏的类型

可运行源码参考本文档[docs](https://github.com/imzbf/md-editor-v3/blob/docs/src/pages/Preview/index.vue)。

![标记及Emoji预览](https://imzbf.github.io/md-editor-v3/imgs/mark_emoji.gif)

> 更多 emoji，[https://getemoji.com/](https://getemoji.com/)。

## 🔒 xss 防范

在`1.8.0`之后，通过`sanitize`事件，自行处理不安全的 html 内容。例如：使用`sanitize-html`处理

```shell
yarn add sanitize-html
```

```vue
<template>
  <md-editor :sanitize="sanitize" />
</template>

<script setup>
import sanitizeHtml from 'sanitize-html';
import MdEditor from 'md-editor-v3';
import 'md-editor-v3/lib/style.css';

const sanitize = (html) => {
  return sanitizeHtml(html);
};
</script>
```

更详细的实现可以参考本文档的源码！

## 🧻 编辑此页面

[demo-zh-CN](https://github.com/imzbf/md-editor-v3/blob/dev-docs/public/demo-zh-CN.md)