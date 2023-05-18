# lodash



◼ **字符串（String）**

​		**_.camelCase()** 

​				转换字符串string为驼峰写法。
​		**_.capitalize(string)** 

​				转换字符串string首字母为大写，剩下为小写。
​		**_.endWith(string, target)** 

​				检查字符串string是否以给定的target字符串结尾。
​		**_padStart(str, lenght,char)** 

​				如string字符串长度小于 length 则在左侧填充字符。 如果超出length长度则截断超出的部分。
​		**_.trim(string, chars)** 

​				从string字符串中移除前面和后面的 空格 或 指定的字符。  



◼ **数组（Array）**

​		**_.first(arr, level)** 

​				获取array中的第一个元素。
​		**_.last(arr, [n=1])** 

​				获取array中的最后一个元素。
​		**_.uniq(arr)** 

​				创建一个去重后的array数组副本。返回新的去重后的数组。
​		**_.compact(arr)** 

​				创建一个新数组，包含原数组中所有的非假值元素。返回过滤掉假值的新数组。
​		**_.flatten(arr)** 

​				减少一级array嵌套深度。返回新数组。



◼ **对象**

​		**_.pick(object, [props])** :  从 object中选中的属性来创建一个对象。返回新对象。

​		**_.omit(object, [props])**:  反向版\_.pick ;  删除object对象的属性。返回新对象。

​		**_.clone( value)** - 支持拷贝 arrays、 booleans、 date 、map、 numbers， Object 对象, regexes, sets, strings, symbols等等。 arguments对象的可枚举属性会拷贝为普通对象。（注：也叫浅拷贝） 返回拷贝后的值。

​		**_.cloneDeep(value)** -这个方法类似_.clone，除了它会递归拷贝 value。（注：也叫深拷贝）。返回拷贝后的值。



**◼ 集合（Array | Object）**
		**_.sample()**:  从collection（集合）中获得一个随机元素。返回随机元素。

​		**_.shuffle()**:  创建一个被打乱值的集合。

​		**_.orderBy** :  给数组排序，默认是升序asc。

​		**_.each() / _.forEach()**  -  遍历(集合) 中的每个元素

​		**_.filter( )** - 返回一个新的过滤后的数组。



**◼ 函数**
		**_.curry()** - 返回新的柯里化（curry）函数。

​		**_.debounce()** - 返回新的 debounced（防抖动）函数。

​		**_.throttle()** - 返回节流的函数。



# Day.js

**◼ 获取(Get) + 设置(Set)**
		**.year()、.month、.date()** - -  获取年、月、日
		**.hour()、.minute()、.second()**   -  获取时、分、秒
		**.day()**  - 获取星期几
		**.format()**  - 格式化日期



**◼ 操作日期和时间**
		**.add(numbers , unit)** - 添加时间
		**.subtract(numbers , unit)**  - 减去时间
		**.strartOf(unit)** - 时间的开始(例如：获取今年的第一天零时零分零秒)



**◼ 解析时间**
		**dayjs(毫秒|秒)**  - 时间戳（毫秒 和 秒）
		**dayjs('2022-06-15', 'YYYY-MM-DD')**  -  字符串 + 格式
		**dayjs(new Date())** - 接收日期对象



**◼ Day.js的插件应用**
		**.fromNow()** - 从现在开始的时间 （需要依赖：relativeTime 插件）
		**relativeTime插件**：https://cdn.jsdelivr.net/npm/dayjs@1.11.3/plugin/relativeTime.js