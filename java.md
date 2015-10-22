# 5.3.局部内部类
  你可以在这些代码区比如方法体，结构体，初始化模块定义内部类，这些局部内部类不是下列类的成员，它们的代码是不完整的，但
是它们属于这个代码区，，就像内部变量一样。这些类在它们被定义的区域之外是完全不可获得的，完全没有方法访问它。但是，这些类
的实例是可以作为参数传递的普通对象，并从方法中返回，直到她们不再被引用。因为本地的类是无法访问的，它们不能有访问修饰词，
也不能被声明为静态的，因为它们是本地的内部类。所有其他类修饰符，包括注释，可以应用，但只有strictfp和注释都有实际的应用。


  局部内部类可以访问所有的变量，在地方范围内的类是被定义为局部变量，方法参数，实例变量（假设它是一个非静态块），变量和静
态变量。唯一的限制是一个本地变量或方法参数可以被访问，只有它被声明为final。这一限制的原因主要涉及多线程问题（见14章），
并确保所有这些变量有定义好的值时，从内部类访问。鉴于访问局部变量或参数的方法被调用完成后采用局部类定义的局部变量和参数，
因此这些变量的存在价值不再必须冻结，在局部类的对象被创建之前。如果需要，您可以将非最终变量复制到随后由本地的内部类访问的
最后一个变量中。

  考虑在java.util包中定义的标准接口迭代器。这个接口定义了一个方法来遍历一组对象。通常用于在容器对象中提供访问元
素的访问，但可用于任何通用的迭代.
   'package java.util;

public interface Iterator<E> {

    boolean hasNext();
    
    E next() throws NoSuchElementException;
    
    void remove() throws UnsupportedOperationException,
    
                         IllegalStateException;
}
'

  hasNext方法会返回true如果next要返回更多的元素，remove方法会移除掉next方法返回的最后一个元素。但是remove方法是
可选择的也就意味着当remove方法在next方法之前被提出或者在next方法被仅使用一次而remove方法却被所提出多次的情况下，
系统就会抛出UnsupportedOperationException，然后 IllegalStateException也会被抛出如果next方法在没有元素之后还被使
用就会返回NoSuchElementException详情请见571页的“Iteration”（迭代）.

  下面是一个简单的方法，它返回一个遍历一系列对象实例
的迭代器：

package java.util;

public interface Iterator<E> {

    boolean hasNext();
    
    E next() throws NoSuchElementException;
    
    void remove() throws UnsupportedOperationException,
    
                         IllegalStateException;
                         
}


   hasNext方法会返回true如果next要返回更多的元素，remove方法会移除掉next方法返回的最后一个元素。但是remove方法是可选
择的也就意味着当remove方法在next方法之前被提出或者在next方法被仅使用一次而remove方法却被所提出多次的情况下，系统就会
抛出UnsupportedOperationException，然后 IllegalStateException也会被抛出如果next方法在没有元素之后还被使用就会返回
NoSuchElementException详情请见571页的“Iteration”（迭代）


	下面是一个简单的方法，它返回一个遍历一系列对象实例的迭代器：
	
	public static Iterator<Object>
	
    walkThrough(final Object[] objs) {

    class Iter implements Iterator<Object> {
    
        private int pos = 0;
        
        public boolean hasNext() {
        
            return (pos < objs.length);
            
        }
        
        public Object next() throws NoSuchElementException {
        
            if (pos >= objs.length)
            
                throw new NoSuchElementException();
                
            return objs[pos++];
            
        }
        
        public void remove() {
        
            throw new UnsupportedOperationException();
            
        }
        
    }
    
    return new Iter();
    
}

  Iter类是walkThrough方法的本地类，它不是外围类的成员因为Iter是本地方法，它具有访问所有的方法在特定的参数对象的final变
量。它定义了一个pos字段来跟踪对象数组中的它在哪里。（这段代码假定迭代器和NoSuchElementException是从含有walkThrough
的java.util资源中导入的）。

  局部内类的成员可以隐藏他们在声明中的本地变量和参数，正如它们可以隐藏实例字段和方法一样。在所有情况下，讨论了140
页的规则。唯一的区别是，一旦一个局部变量或参数被隐藏，这就是不可能的。

##5.3.1 静态语境中的类
	我们声明一个内部类通常与封闭类的实例关联。可以声明一个局
部内部类，或一个匿名内部类（见下一节），在一个静态的语境：
一个静态方法，静态初始化块，或作为一个静态的初始化部分。
在这些静态的上下文中，没有包含类的相应实例，因此内部类实
例没有外围实例。在这种情况下，任何试图用一个合格的表达式
来引用一个封闭类的实例字段或方法都会导致编译时错误。

    
