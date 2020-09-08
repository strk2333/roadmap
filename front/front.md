# Front





















表单相关

- 输入 emoji （utf-16）

  - `/[\ud800-\udbff][\udc00-\udfff]/g`

- 空格处理 trim

- 特殊字符处理 

  ```js
  {
    pattern: /^[^`~!@#$%^&*_+<>?:"{},./\\\\;'[\]·！#￥——：；“”‘、，|《。》？、【】[\]]+$/gim,
    message: '不可输入特殊字符'
  }
  ```

- loading

  - 加载 loading
  - 提交 loading
  - 上传 loading

- 空白占位

- InputNumber





安全相关

- 





Regex

- 匹配多行任意字符：[\s\S]
- 限制单行个数：需要加^$





复制

- 



CSS

- display: block; width; margin: 0 auto;



需求

- 多行文本省略

  - CSS 

                  display: -webkit-box;
                  -webkit-line-clamp: 2;
                  -webkit-box-orient: vertical;

  - JS substring