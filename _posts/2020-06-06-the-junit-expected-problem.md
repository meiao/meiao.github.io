---
layout: post
title:  "The JUnit exception problem"
category: java
tags: junit testing
---

We wrote a method that is supposed to throw an exception in some cases and we
want to test it. We have a big problem!

## JUnit to the rescue
JUnit provides a way for you to test exceptional cases your class may encounter.

The syntax is really simple, just add expected and the exception class to the
@Test annotation. Like the example below.
{% highlight java %}
@Test(expected=MyException.class)
public void methodToBeTestedTest() {
  testSpecificSetup();
  classToBeTested.methodThatThrowsException();
}
{% endhighlight %}

We solved the problem. Problem solved.

But wait. We needed to check whether there was some side effect, or if some mock
was called (or not). I am totally freaking out!

## Plain old try-catch to the rescue

Not being able to verify mocks and run other asserts is just the first problem
with the @Test(expected). Another problem is that a test will pass, when it
should fail, if the method testSpecificSetup() throws MyException.

If you think this is unlikely and you would never do something like this,
remember, no man is an island. And even the best coder will write something
wrong any now and then, no one is perfect.

So how to test it? You can use the plain old try-catch block to catch the
expected exception and then you can verify everything you want. You can verify
extra attributes in your exception. Just don't forget to fail the test if the
method does not throw the exception.

{% highlight java %}
@Test
public void methodToBeTestedTest() {
  testSpecificSetup();
  try {
    classToBeTested.methodThatThrowsException();
    Assert.fail("Exception was expected in the line above.");
  } catch (MyException exception) {
    // you can verify the exception here
  }
  // and do your other verifications here
}
{% endhighlight %}

We solved the problem. Problem solved.
