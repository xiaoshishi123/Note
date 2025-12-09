# 实际应用小问题

### 1 .value问题

对于ref对象 本来应该要用.value来取得他的值

但是  对于reactive封装的  reactive会自动帮你解开

```javascript
let obj = reactive({
    a:1,
    b:2,
    c:ref(3)
  })
  let x = ref(9)
  console.log(obj.c) 
  console.log(x.value) 	
```

