---
title: How to create a simple Random number generator in Java 
layout: post
categories: 
  - java
  - android
comments: true
date:   2019-6-8  
author: "Ted Hagos"
image: /images/book_las3.jpg
---

If you need to generate Random numbers in your Android app (assuming you're using Java), you can simply use the `java.util.Random` class. 

````java
Random random = new Random();        // (1)
mrandomnumber = random.nextInt(100); // (2)
````


**(1)** Creates a new Random object

**(2)** Creates a new random integers not greater than 100

Java's Random class generates not only integers, it can also handle double, float, boolean etc., so,  make sure you check out the docs at  <a href='https://bit.ly/javautilrandom'>java.util.Random docs page</a> 

Here's a small sample on how to use the Random class in Java. The **RandomNumber** class has two methods named `getNumber()` and `createRandomNumber()`, it's supposed to behave like a _Singleton_ so, it's designed to create a random number only once. 

```java
import android.util.Log;
import java.util.Random;

public class RandomNumber  {

  private String TAG = getClass().getSimpleName();

  int mrandomnumber;
  boolean minitialized = false;

  String getNumber() {
    if(!minitialized) {
      createRandomNumber();
    }
    Log.i(TAG, "RETURN Random number");
    return mrandomnumber + "";
  }

   void createRandomNumber() {
    Log.i(TAG, "CREATE NEW Random number");
    Random random = new Random();
    mrandomnumber = random.nextInt(100);
    minitialized = true;
  }
}

```

Then, you can use it like this from MainActivity

```java
public class MainActivity extends AppCompatActivity {

  @Override
  protected void onCreate(Bundle savedInstanceState) {
    super.onCreate(savedInstanceState);
    setContentView(R.layout.activity_main);

    RandomNumber data = new RandomNumber();

    ((TextView) findViewById(R.id.txtrandom)).setText(data.getNumber());
  }
}
```

{% include book_las3.html %}