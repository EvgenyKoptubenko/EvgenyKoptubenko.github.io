---
layout: post
title:  "Java Interfaces vs Abstract Classes!"
date:   2022-06-29 00:02:50 +0300
categories: Java interview
---
Let's talk about Java interfaces and abstract classes. It is necessary to understand the difference between them, especially when there are default methods in interfaces now.
The example is taken from official documentation [docs.oracle.com][docs-oracle-com]

{% highlight java %}
package com.mak.interfaces;

public interface BicycleInterface {

    void changeCadence(int newValue);

    void changeGear(int newValue);

    void speedUp(int increment);

    void applyBrakes(int decrement);

}
{% endhighlight %}

All the methods of the interface are public.
The method can be private, but it must contain a body.
In case the method is public, it can have a body only when it has default at the method's signature

{% highlight java %}
public interface BicycleInterface {

    private void changeCadence(int newValue){
        return;
    }

    default void changeGear(int newValue){
        return;
    }

    void speedUp(int increment);

    void applyBrakes(int decrement);

}
{% endhighlight %}

The class can implement as many interfaces as it wants. What happend in case the interfaces have the same methods?

{% highlight java %}
package com.mak.interfaces;

public class BicycleImpl implements BicycleInterface, BicycleInterface2{

    @Override
    public void speedUp(int increment) {

    }

    @Override
    public void applyBrakes(int decrement) {

    }
}

package com.mak.interfaces;

public interface BicycleInterface {

    private void changeCadence(int newValue){
        return;
    }

    default void changeGear(int newValue){
        return;
    }

    void speedUp(int increment);

    void applyBrakes(int decrement);

}

package com.mak.interfaces;

public interface BicycleInterface2 {

    private void changeCadence(int newValue){
        return;
    }

    default void changeGear(int newValue){
        return;
    }

    void speedUp(int increment);

    void applyBrakes(int decrement);

}


{% endhighlight %}

com.mak.interfaces.BicycleImpl inherits unrelated defaults for changeGear(int) from types com.mak.interfaces.BicycleInterface and com.mak.interfaces.BicycleInterface2
So to fix the issue it is necessary to state directly which method to use

{% highlight java %}
package com.mak.interfaces;

public class BicycleImpl implements BicycleInterface, BicycleInterface2{

    @Override
    public void changeGear(int newValue) {
        BicycleInterface.super.changeGear(newValue);
    }

    @Override
    public void speedUp(int increment) {

    }

    @Override
    public void applyBrakes(int decrement) {

    }
}
{% endhighlight %}

The pros of default methods: 1. We can add as many default methods and it won't break the implementing classes.

There is also such a tricky question: what is the difference between abstract and non-abstract interface. The answer is that there is no difference. All interfaces are abstract, the abstract word is redundant.

Static methods in interface.

{% highlight java %}
package com.mak.interfaces;

public interface BicycleInterface {

    private void changeCadence(int newValue){

    }

    default void changeGear(int newValue){

    }

    static void speedUp(int increment) {

    }

    void applyBrakes(int decrement);

}
{% endhighlight %}
The static interface methods cannot be invoked using implementing class, but should be used using interface:

{% highlight java %}
package com.mak.interfaces;

public class Main {
public static void main(String[] args) {
BicycleInterface.speedUp(42);
}
}

{% endhighlight %}

[docs-oracle-com]: https://docs.oracle.com/javase/tutorial/java/concepts/interface.html
