# **2020-21学年第2学期**

## **实 验 报 告**

![](img/zucc.png)

-   课程名称: <u>编程语言原理与编译</u>
-   实验项目: <u>期末大作业</u>
-   专业班级: <u>计算1802</u>
-   学生学号: <u>30801091</u>
-   学生姓名: <u>朱宇欢</u>
-   实验指导教师: <u>郭鸣</u>

---

## 简介

---



---

## 结构

---

### 前端(F#)

* AbstractSyntax.fs 定义抽象语法
  * 定义了基础类型：TypeInt，TypeChar，TypeFloat，TypeVoid，TypeStruct，TypeArray等
  * 定义了基础表达式：访问，赋值，取值的地址，**访问并赋值**，常数int值，常数String值，常数Float值，空值，以及一元，二元，三元的表达式，And，Or的判断表达式，跳转
  * if，while，do-while，Expression，return，block，for，throw，switch-case，try-catch，continue，break，**sleep**等声明的定义
  * 异常，函数，变量，语句块等的定义
* 由MyLex.fsl生成的MyLex词法生成器
  * 将程序中代码，一般为小写的关键字翻译为大写，“char”-> "CHAR"
  * 将程序中操作符翻译，例：“%=”-> "MODEQ"
* 由MyPar.fsy生成的MyPar语法生成器
  * 定义token：1.定义CSTINT，CSTCHAR，CHAR，WHILE等token，token就是把程序(.c)用token翻译
  * 声明，例如将声明ExprNotAccess，并模式匹配到“Access PLUSEQ Expr          { AssignThird("+=",$1,$3)}”，在AbstractSyntax已经定义了AssignThird，第一个参数为String，第二个为Access，为可访问的值，第三个为Expr，为表达式
  * 将程序经过MyLex词法分析翻译后，通过MyPar进行语法分析
* Assembly.fs 定义了中间表示的生成指令
* Combile.fs 将程序的抽象语法树转化为中间表示

### 后端(Java)

* package exception定义了异常，分别为操作符错误(OperatorError.java)和类型错误(TypeError.java)
* package mytype 定义类型，有基础类型(basic)，Array，Char，Float，Int
* Instruction.java 定义指令
* Machine.java生成Machine.class虚拟机和Machinetrace.class堆栈追踪

### 测试集

* 测试集位于test文件夹中

### 库

* FsLexYacc.Runtime.dll

---

## 用法

---

### 前端

* 写完AbstractSyntax.fs

* MyPar.fsy引入AbstractSyntax，通过MyPar.fsy生成MyPar.fs语法分析器

  ```sh
  dotnet "C:\Users\飞天小女警\.nuget\packages\fslexyacc\10.2.0\build\/fsyacc/netcoreapp3.1\fsyacc.dll"  -o "MyPar.fs" --module MyPar MyPar.fsy
  ```

* MyLex.fsl 代码中打开 MyPar，通过MyLex.fsl生成MyLex.fs词法分析器

  ```sh
  dotnet "C:\Users\飞天小女警\.nuget\packages\fslexyacc\10.2.0\build\/fslex/netcoreapp3.1\fslex.dll"  -o "MyLex.fs" --module MyLex --unicode MyLex.fsl
  ```

* 命令行运行程序，引入引用包

  ```sh
  dotnet fsi
  #r "nuget: FsLexYacc";;  //添加包引用
  ```

* 加载运行该编译器

  ```sh
  #load "AbstractSyntax.fs" "MyPar.fs" "MyLex.fs" "Debug.fs" "Parse.fs" "Assembly.fs" "Combile.fs" "ParseAndComp.fs" ;;
  ```

* open parseAndComp，之后可在fsi中进行编译 *.c文件,生成中间表示 *.out

  ```sh
  open ParseAndComp;;
  compileToFile (fromFile "test/ex5string.c") "test/ex5string.out";;  
  ```

* 中间表示(指令集)

  ```sh
  24 19 1 5 25 15 1 13 0 1 1 0 0 12 15 -1 16 41 13 0 1 1 11 22 15 -1 13 0 1 1 13 0 1 1 11 0 1 1 12 15 -1 13 0 1 1 11 13 11 7 18 18 21 1
  ```

### 后端

* javac编译后端虚拟机 *.java 文件

  ```sh
  javac -encoding UTF-8 Instruction.java mytype/*.java exception/*.java Machine.java 
  ```

* java Machine 运行在前端已经产生的中间表示 *.out

* java Machinetrace 追踪堆栈变化

  ```
  java Machine ex2.out 8
  java Machinetrace ex2.out 8
  ```


---

## 功能实现

---

