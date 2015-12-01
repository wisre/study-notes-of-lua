# lua介绍
   lua是一种扩展性的程序设计语言，不仅支持面向过程的程序设计，也对面向对象，面向函数以及数据驱动型的程序设计提供良好的支持，
它是一种功能强大而且轻量级的脚本语言，lua被当做一个api库来使用，这个api库完全使用c语言实现。

   作为一种扩展性的语言，lua是没有main函数的概念的，它是嵌在主程序里面的，主程序能够调用函数去执行lua代码片段，读写lua变量，以及注册能够被lua
调用的c函数，通过调用C函数，lua能够进一步的扩展自己的适用范围。lua套件里包含名为lua的主程序，通过用lua类库，提供了一个完全独立的lua解释器。

# lua基本语法
 这章主要是介绍lua的词法、语法和语义

## lua词法
 lua的标识符有字母、数字和下划线组成，但是不能以数字开头，这和其他的大多数程序设计语言的规则是一样的，标识符用来定义变量和关联数组的域。
lua的关键字如下，这下关键字不能够用来当做标识符。
<table>
    <tr>
      <td>and</td>
      <td>break</td>
      <td>do</td>
      <td>else</td>
      <td>elseif</td>
    </tr>
      <tr>
      <td>end</td>
      <td>false</td>
      <td>for</td>
      <td>function</td>
      <td>if</td>
    </tr>
      <tr>
      <td>in</td>
      <td>local</td>
      <td>nil</td>
      <td>not</td>
      <td>or</td>
    </tr>
      <tr>
      <td>repeat</td>
      <td>return</td>
      <td>then</td>
      <td>true</td>
      <td>util</td>
      <td>while</td>
      
    </tr>

</table>
  lua标识符是大小写敏感的，and是保留字，但是And和AND是有效的标识符，作为一个约定，以下划线开始接着是大写的字母的标识符被留作lua的内部全局变量。
  
  下表是lua的一些运算符：
  <table>
    <tr>
      <td>+</td>
      <td>-</td>
      <td>*</td>
      <td>/</td>
      <td>%</td>
      <td>^</td>
      <td>#</td>
    </tr>
    <tr>
      <td>==</td>
      <td>~=</td>
      <td><=</td>
      <td>>=</td>
      <td>\></td>
      <td>\<</td>
      <td>=</td>
    </tr>
    <tr>
      <td>(</td>
      <td>)</td>
      <td>{</td>
      <td>}</td>
      <td>[</td>
      <td>]</td>
    </tr>
    <tr>
      <td>;</td>
      <td>:</td>
      <td>,</td>
      <td>.</td>
      <td>..</td>
      <td>...</td>
    </tr>

</table>
  字符串通过单引号和双引号修饰，能够包含跟C语言类似的转义字符：'\a'、'\b'、'\f'、'\n'、'\r'、'\t'、'\v'、'\\'、'\"'、'\''，而且跟在反斜杠后面的一行会在字符串里产生一个换行符。字符串里面的字符也能用反斜杠家数字来表示，例如\ddd。
  
  字符串也能利用方括号通过长格式来定义，以一个左方括号开始中间有n个等号，然后再接一个左方括号，遇到了同样格式的右方括号则表示字符串结束。例如以[==[开始和]==]结束来表示一个字符串，在这种格式里面的字符串可以包含任何字符，不会被转义字符转义，也不理会其他的方括号格式。
数字常量能够用十进制部分和指数部分组成，例如3.14e2，也支持16进制的形式，需在常量前面加上0x。
注释已字符--开始，直到本行末尾，假如--后面接的是长格式的开始，则注释直到长格式的结束符为止。例如--[[ ***]]
      
## 值和类型
  lua是动态类型的语言，意味着它只有值，没有类型定义，所有的值带有它的类型，lua里所有的值都是first-class值，都能够被存储在变量里，
通过参数传递给其他函数或者当做返回值。

  在lua中有8中基本类型：Nil（空类型）、数值类型、布尔类型、字符串类型、函数、userdata、线程类型和关联表类型。Nil是值nil的类型，它代表不存在任何有意义的数据，布尔类型是false值和true值的类型，nil值和false值都是条件为否，其他的值条件为真。
Number代表着实数，字符串代表着字符数组，字符串能够包含任何8位的字符，包括空字符'\0'，lua能够调用用lua和c定义的函数。

  userdata类型用于存储任意的c数据，这个类型相当于一块原始内存，而且没有任何预定义操作方法。但是，程序员可以通过metatable为userdata类型的数据定义操作方法，在lua中userdata值
不能被创建和修改，只能够通过C的api来实现，这样就保证了主程序的数据完整性。

  thread类型表示一个独立的可执行的线程，被用来实现协同程序，不要把lua线程和操作系统的线程混淆，lua支持在所有的出操作系统上的协同程序，甚至是那些不支持线程的系统。
  
  table就是关联数组的类型，关联数据不仅能用数值作为索引，也能够以出了nil值以外的所有其他值当做索引，table类型能够包含任何类型的值，出了nil值，table是lua的唯一的数据结构机制，能够被用来定义普通数据，符号表，sets，记录，图等。为了表示records，lua用域值来当做索引，lua支持用a.name来表示a['name']。
  
  tables、userdata、thread和函数是对象，lua变量其实不能包含这些值，只能够引用它们，赋值、参数传递和返回值总是操作这些值的引用，不存在任何的值拷贝。
  
  库函数type可以返回给定值的类型描述，lua提供字符串类型和数值类型的自动转换，为了完全控制数值怎么转换成字符串类型，可以使用string函数来实现。
  
## 变量
  在lua中有三种类型的变量：全局变量，局部变量和table的域变量，单个标识符能够表示全局变量或者局部变量，假如没有特殊修饰，所有的变量都被假定为全局变量，除非在变量的前面有local保留字的修饰，局部变量能够在定义他们的函数里被随时的访问，在为变量赋值前，它的值是nil。
  
  方括号用来获取table类型的值，访问数组值t[i]也可以用gettable_event(t,i)来表示，全局变量作为普通table数组的域，叫做环境table，每个函数都有环境table的引用，以便函数里的全局变量能够指向环境变量表，当一个函数被创建，它继承创建它的函数的上下文信息，为了获取lua函数的环境表，能够调用getfenv函数，可以调用setfenv来设置环境表。可以通过_env.x方式访问全局变量，也可以用gettable_event(_env,"x")来访问。_env是函数的运行环境
  
# 语句
  lua几乎支持所有的在pascal和C里常见的语句，包括：赋值语句、控制语句、函数调用和变量声明。
  
## 语句块
  lua的执行单元叫做chunk，一个chunk块由一系列的语句组成，这个语句都是顺序执行的，每个语句由分号分隔，chunk的样式如下：
   
  <p>chunk ::={stat[';']}</p>
   
  lua中不存在空语句，语句;;是非法的。lua可以操作chunk语句为匿名函数的函数体，这个匿名函数可以有多个参数，同样的，chunk语句也能定义局部变量，接受参数的传递和返回值。
   
  一个chunk块能够存储在文件里，或者存储在主函数的一个字符串变量里。为了执行chunk块，lua首先为虚拟机把它预编译成指令集，然后再利用解释器执行。
   
  chunk块也能够用luac预编译成二进制格式，程序的源码也编译形式能相互转化，lua能够自动的检测文件的类型然后正确的执行。

## block
  block跟chunk一样，是一系列的语句，block能够通过do和end明确的划定界限而产生一个语句，明确范围的block能够限定变量的作用范围，有时也能用来在其他block的中间增加一个return和break语句。
   
  <p>block ::= chunk</p>
  <p>stat ::= do block end</p>
  
 ## 赋值语句
   lua支持多重赋值，因此，赋值语句的语法是在=号左边是一系列的变量，在右边是一系列的表达式，两边的元素列表用逗号分隔。
   
   <p>stat ::= varlist `=´ explist</p>
   <p>varlist ::= var {`,´ var}</p>
   <p>explist ::= exp {`,´ exp}</p>
   
   在赋值前，值得列表的长度要调整为变量列表的长度，假如值得列表的长度更长，多余的值被丢弃，假如变量的列表更长，超出值列表长度的变量被赋值为nil，假如在值列表中有函数调用，则函数的所有返回值在值列表调整前会成为值列表的元素。
   
   赋值语句首先执行所有的表达式，然后才开始赋值，因此下面的代码设置a[3]为20，并不会影响a[4]的值，因为在a[i]在i被赋值为4前，已经确定为3了。
   
   <p> i = 3</[>
   <p>i, a[i] = i+1, 20</p>
   
   同样的，下面的代码是交换x和y的值
   
   <p>x,y=y,x</p>
   
   赋值给全局变量和数组元素的操作能够用操作元表来替换，例如t[i]= val和 settable_event(t,i,val)是相等的，给全局变量赋值x= val和 _env.x=  val以及settable(_env,x,val)是相等的，_env是正在运行的函数的上下文环境。
   
## 控制语句
  控制结构if、while和repeat有着通用的语义和语法，如下所示：
 
  stat ::= while exp do block end
  
  stat ::= repeat block until exp
  
  stat ::= if exp then block {elseif exp then block} [else block] end
  
  控制结构的条件表达式能够接受任何值，false和nil被当做条件否，所有其他的值当做条件成立。在repeat..until结构中，内部的block块不会在until这里结束，而是在条件表达式后才结束，因此，条件表达式能够引用block里面的值。
  
  return语句用来返回函数也chunk的值，由于函数和chunk可以返回多个值，因此return的语法如下：
   
  stat ::= return [explist]
  
  break语句用来终止while循环，repeat循环和for循环的执行。
  
  return和break语句只能放在block的最后，假如确实需要房子啊block的中间，应该使用一个有限定范围的block语句，例如do return end或者do break end
  
## for语句
  for语句有两种形式，一种数字循环型的，一种通用的循环样式
  
  数字型的循环通过一个控制变量等差递增或者等差递减来实现循环，它的语法如下：

  stat ::= for Name `=´ exp `,´ exp [`,´ exp] do block end
  
  name的初始值是第一个表达式的值，第二个表达式是变量的限定值，第三个变量的值是等差系数，下面的for循环语句与下面的代码是等价的
  
  for v = e1, e2, e3 do block end
  
  等价于
  
  do
       local var, limit, step = tonumber(e1), tonumber(e2), tonumber(e3)
       if not (var and limit and step) then error() end
       while (step > 0 and var <= limit) or (step <= 0 and var >= limit) do
         local v = var
         block
         var = var + step
       end
     end

Tips
* 所有的三个控制变量在循环开始前都会计算一次，它们的结果必须是数字
* var limit step是不存在的变量，在这里是为了更好的解释
* 假如第三个表达式是空值，等差系数则为1
* 能够用break退出for循环
* 循环变量v是循环体内的局部变量，在循环结束后就会别销毁，假如你需要这个值，你需要在循环结束前把它复制给其他变量

通用的for循环语句也迭代器函数一起工作，每次迭代，迭代器函数被调用以产生一个新值，当值变为nil时，循环结束。通用for循环的语法如下：

stat ::= for namelist in explist do block end
	namelist ::= Name {`,´ Name}
	
下面的for循环语句：

for var_1, ···, var_n in explist do block end

等价于：

do
       local f, s, var = explist
       while true do
         local var_1, ···, var_n = f(s, var)
         var = var_1
         if var == nil then break end
         block
       end
     end
     
Tips：
* explist仅仅执行一次，它的值是一个迭代函数，一个状态和一个迭代变量初始值

## 函数调用
函数调用的语法如下：

stat ::= functioncall

## 变量本地声明

本地变量能够在block的任何地方声明，声明也能包括初始化赋值：

stat ::= local namelist [`=´ explist]

一个chunk也是一个block，变量能够在限定范围的block外，但是在chunk内声明，在这种情况下，变量的作用范围是chunk结束。
