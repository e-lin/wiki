---
layout: post
title:  "Memory Management in Android"
date:   2016-06-14 00:00:00 +0900
categories: jekyll update
type: Android
excerpt_separator: <!--more-->
---
<!--more-->

WeakReference
---
An object being collected contents below rules:

- Garbage Collection is running
- The obeject is not being referenced

Generally in Java, Garbage Collection automatically collects objects that are not referenced. But in special case like using cache, programmers have to manually set objects to `null` to permit Garbage Collection to collect the objects. It is quite not insinctive. Therefore, Java has a `WeakReference` to do it.

An example using WeakReference:

``` java
public class TestWeakReference {


    public static void main(String[] args) {

        Car c = new Car(22000,"silver");
        WeakReference<Car> weakCar = new WeakReference<Car>(c);

        /** we do not need this, as once c is not pointing to the car object,
          * GC will automatically collect the car object */
        // c = null;

        int i = 0;

        while( true ){
            if( null != weakCar.get() ){
                i++;
                System.out.println( "Object is alive for "+i+" loops - " + weakCar );
            }else{
                System.out.println( "Object has been collected." );
                break;
            }
        }
    }

}
```
After a while, you will see the program prints `Object has been collected.`

But if we add one line code like below:

``` java
public class TestWeakReference {


    public static void main(String[] args) {

        Car c = new Car(22000,"silver");
        WeakReference<Car> weakCar = new WeakReference<Car>(c);

        int i = 0;

        while( true ){

            /** make the c obeject being referenced */
            System.out.println( "here is the strong reference 'car' " + c );

            if( null != weakCar.get() ){
                i++;
                System.out.println( "Object is alive for "+i+" loops -" + weakCar );
            }else{
                System.out.println( "Object has been collected." );
                break;
            }
        }
    }

}
```
The car object will not be collected because of a strong referece c still points to it.


Reference
---
[Android WeakReference的理解与使用][R1]<br />


[R1]: http://www.ithtw.com/6791.html
