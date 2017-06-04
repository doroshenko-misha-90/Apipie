# Vue-apify

This is a tool for transforming the declaration of api call to js object.
Inspired by VueRouter, Vue render function declaration, koa2.

## Installation
```bash
# Using yarn:
yarn install vue-apify
# Using npm:
npm install vue-apify
```

Using CDN:
```html
<script src="https://unpkg.com/vue-apify"></script>
```

```js
import Vue from 'vue'
import VueApify from 'vue-apify'
import axios from 'axios'

const apiDecl = [
  { 
    name: 'user', // Further you'll use it as `api.user()` for sending request
    options: {    // All of the options you'll find https://github.com/mzabriskie/axios#request-config
      url: '/user/:id',
      method: 'get'
    }
  },
  {
    name: 'settings', // You can not call apiMap.settings(), but apiMap.settings.get() will be available
    url: '/settings',
    method: 'GET'
    children: [
      { 
        name: 'changePassword', // api.settings.changePass(payload)
        options: {
          url: '/change_pass',
          method: 'post'
        }
      },
      {
        name: 'changeAvatar',  // api.settings.changeAvatar(payload)
        url: '/change_avatar', // Simple decralation if yoou need only url and method from axios options
        method: 'post'
      }
    ]
  }
]

const api = VueApify.create(apiDecl, { axiosInstance: axios.create() })

Vue.use(apify)

new Vue({
  // ...
  api,
  // ...
  mounted () {
    api.user({ url_params: { id: 1 } }) // GET: /user/1
      .then(ctx => {
        console.log(ctx.response) // Responce schema as here:
                                  // https://github.com/mzabriskie/axios#response-schema
      })
    const change_pass = {
      cur_pass: '123',
      new_pass: '321'
    }
    api.settings.changePassword({ params: change_pass }) // GET: /change_pass?cur_pass='123'&new_pass='321'
      .then(ctx => { console.log(ctx.response) })
      
    const avatar = // ...
    api.settings.changeAvatar({ data: { avatar } })
  }
})
```
### Documentations
[See here](/docs)
