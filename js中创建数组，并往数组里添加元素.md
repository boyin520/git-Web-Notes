# [js中创建数组，并往数组里添加元素](https://www.cnblogs.com/116970u/p/10585725.html)

- 数组的创建  

1. var arrayObj = new Array();　//创建一个数组  
2. var arrayObj = new Array([size]);　//创建一个数组并指定长度，注意不是上限，是长度  
3. var arrayObj = new Array([element0[, element1[, ...[, elementN]]]]);　创建一个数组并赋值  

​    要说明的是，虽然第二种方法创建数组指定了长度，但实际上所有情况下数组都是变长的，也就是说即使指定了长度为5，仍然可以将元素存储在规定长度以外的，注意：这时长度会随之改变。

- 数组元素的添加

1. arrayObj. push([item1 [item2 [. . . [itemN ]]]]);// 将一个或多个新元素添加到数组结尾，并返回数组新长度 
2. arrayObj.unshift([item1 [item2 [. . . [itemN ]]]]);// 将一个或多个新元素添加到数组开始，数组中的元素自动后移，返回数组新长度 
3. arrayObj.splice(insertPos,0,[item1[, item2[, . . . [,itemN]]]]);//将一个或多个新元素插入到数组的指定位置，插入位置的元素自动后移，返回""。

- 数组的元素的访问  

1. var testGetArrValue=arrayObj[1]; //获取数组的元素值  
2. arrayObj[1]= "这是新值"; //给数组元素赋予新的值

- 数组元素的删除

1. arrayObj.pop(); //移除最后一个元素并返回该元素值  
2. arrayObj.shift(); //移除最前一个元素并返回该元素值，数组中元素自动前移  
3. arrayObj.splice(deletePos,deleteCount); //删除从指定位置deletePos开始的指定数量deleteCount的元素，数组形式返回所移除的元素

- 数组的截取和合并

1. arrayObj.slice(start, [end]); //以数组的形式返回数组的一部分，注意不包括 end 对应的元素，如果省略 end 将复制 start 之后的所有元素  
2. arrayObj.concat([item1[, item2[, . . . [,itemN]]]]); //将多个数组（也可以是字符串，或者是数组和字符串的混合）连接为一个数组，返回连接好的新的数组

- 数组的拷贝

1. arrayObj.slice(0); //返回数组的拷贝数组，注意是一个新的数组，不是指向  
2. arrayObj.concat(); //返回数组的拷贝数组，注意是一个新的数组，不是指向

- 数组元素的排序

1. arrayObj.reverse(); //反转元素（最前的排到最后、最后的排到最前），返回数组地址  
2. arrayObj.sort(); //对数组元素排序，返回数组地址 

参考：https://www.cnblogs.com/huyanlon/p/6866065.html