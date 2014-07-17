## 20 Helpful jQuery Methods you Should be Using

### after()/before()  insertAfter/insertBefore

### change() 建议替代 blur()

### data()  removeData()存一些关于某元素的数据

###　queue()  dequeue()

###　delay()  $('div').hide().delay(2000).show()

### bind() unbind() live()  die()

### eq()  $('p:eq(1)').addClass('emphasis')

### get()

###  grep()

var nums = '1,2,3,4,5,6,7,8,9,10'.split(',');
 
``` 
nums = $.grep(nums, function(num, index) {
  // num = the current value for the item in the array
  // index = the index of the item in the array
  return num > 5; // returns a boolean
});

console.log(nums) // 6,7,8,9,10
 ```

 ###   Pseudo-Selectors

 ### isArray() isEmptyObject()  isFunction()  isPlainObject()

 ###  makeArray()

 ###  map()

 ### parseJSON()

 ###　proxy()

 ###　replaceAll()  replaceWith()

 ###　serialize()  serializeArray()

 ###  siblings()

###　wrap() wrapAll() wrapInner()



