###重构之提炼函数

Intellij IDEA带了一些有意思的快捷键，或者说自己之前不在意这些快捷键的存在。重构作为单独的一个菜单，显然也突显了其功能的重要性，说说**提炼函数**，或者说提出方法。

快捷键

Mac:  ``alt``+``command``+``M``

Windows/Linux: ``Ctrl``+``Alt``+``M``

鼠标: Refactor | Extract | Method

####重构之前

以重构一书代码为例，重构之前的代码

```java
public class extract {
    private String _name;

    void printOwing(double amount){
        printBanner();
        
        System.out.println("name:" + _name);
        System.out.println("amount" + amount);
    }

    private void printBanner() {
    }
}
```

####重构

选中

```java
System.out.println("name:" + _name);
System.out.println("amount" + amount);
```

按下上述的快捷键，会弹出下面的对话框

![Extrct Method](img/day1/extract-method.png)

输入

     printDetails

那么重构就完成了。

####重构之后

IDE就可以将方法提出来

```java
public class extract {
    private String _name;

    void printOwing(double amount){
        printBanner();
        printDetails(amount);
    }

    private void printDetails(double amount) {
        System.out.println("name:" + _name);
        System.out.println("amount" + amount);
    }

    private void printBanner() {
    }
}
```

####重构

还有一种就以Intellij IDEA的示例为例，这像是在说其的智能。

```java
public class extract {
    public void method() {
        int one = 1;
        int two = 2;
        int three = one + two;
        int four = one + three;
    }
}
```

只是这次要选中的只有一行，

```
int three = one + two;
``

以便于其的智能，它便很愉快地告诉你它又找到了一个重复

     IDE has detected 1 code fragments in this file that can be replaced with a call to extracted method...

便返回了这样一个结果

```java
public class extract {

    public void method() {
        int one = 1;
        int two = 2;
        int three = add(one, two);
        int four = add(one, three);
    }

    private int add(int one, int two) {
        return one + two;
    }

}
```

然而我们就可以很愉快地继续和它玩耍了。当然这其中还会有一些更复杂的情形，当学会了这一个剩下的也不难了。

###重构之内联函数

继续走这重构一书的复习之路，接着便是内联，除了内联变量，当然还有内联函数。

快捷键

Mac:  ``alt``+``command``+``M``

Windows/Linux: ``Ctrl``+``Alt``+``M``

鼠标: Refactor | Inline

####重构之前

```java
public class extract {

    public void method() {
        int one = 1;
        int two = 2;
        int three = add(one, two);
        int four = add(one, three);
    }

    private int add(int one, int two) {
        return one + two;
    }

}
```

在``add(one,two)``很愉快地按上个快捷键吧，就会弹出

![Inline Method](img/day1/inline.jpg)

再轻轻地回车，Refactor就这么结束了。。

####Intellij Idea 内联临时变量

以书中的代码为例

```java
double basePrice = anOrder.basePrice();
return (basePrice > 1000);
```

同样的，按下``Command``+``alt``+``N``

```java
return (anOrder.basePrice() > 1000);
```

对于python之类的语言也是如此

```python
def inline_method():
    baseprice = anOrder.basePrice()
    return baseprice > 1000
```

###重构之以查询取代临时变量

快捷键

Mac:  木有

Windows/Linux:  木有

或者: ``Shift``+``alt``+``command``+``T`` 再选择  ``Replace Temp with Query``

鼠标: **Refactor** | ``Replace Temp with Query``

####重构之前

过多的临时变量会让我们写出更长的函数，函数不应该太多，以便使功能单一。这也是重构的另外的目的所在，只有函数专注于其功能，才会更容易读懂。

以书中的代码为例

```java
import java.lang.System;

public class replaceTemp {
    public void count() {
        double basePrice = _quantity * _itemPrice;
        if (basePrice > 1000) {
            return basePrice * 0.95;
        } else {
            return basePrice * 0.98;
        }
    }
}
```

####重构

选中``basePrice``很愉快地拿鼠标点上面的重构

![Replace Temp With Query](img/day1/replace.jpg)

便会返回

```java
import java.lang.System;

public class replaceTemp {
    public void count() {
        if (basePrice() > 1000) {
            return basePrice() * 0.95;
        } else {
            return basePrice() * 0.98;
        }
    }

    private double basePrice() {
        return _quantity * _itemPrice;
    }
}
```

而实际上我们也可以

1. 选中

    _quantity * _itemPrice

2. 对其进行``Extrace Method``

3. 选择``basePrice``再``Inline Method``

在Intellij IDEA的文档中对此是这样的例子

```java
public class replaceTemp {

    public void method() {
        String str = "str";
        String aString = returnString().concat(str);
        System.out.println(aString);
    }

}
```

接着我们选中``aString``，再打开重构菜单，或者

``Command``+``Alt``+``Shift``+``T`` 再选中Replace Temp with Query

便会有下面的结果:

```java
import java.lang.String;

public class replaceTemp {

    public void method() {
        String str = "str";
        System.out.println(aString(str));
    }

    private String aString(String str) {
        return returnString().concat(str);
    }

}
```
