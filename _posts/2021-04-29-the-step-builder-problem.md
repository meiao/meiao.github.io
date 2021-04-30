---
layout: post
title:  "The Step Builder problem"
category: java
tags: api
---

We designed our API. We created a POJO that we want to be immutable. To make it
easier to instantiate it, we provide a _builder_. But to make it even more user
friendly, we made it a _step builder_. So whoever wants to instantiate my POJO
can easily concatenate the methods, and the IDE will complete with the correct
method time after time.

If you never heard of a step builder, here is an example.

{% highlight java %}
public class Pizza {
  private final Crust crust;
  private final Sauce sauce;
  private final Topping topping;

  private Pizza(Builder builder) {
    this.crust = builder.crust;
    this.sauce = builder.sauce;
    this.topping = builder.topping;
  }

  // getters omitted for brevity

  public static class Builder
      implements NeedsCrust, NeedsSauce, NeedsTopping, Bakeable {
    private Crust crust;
    private Sauce sauce;
    private Topping topping;

    private Builder() {}

    public NeedsSauce crust(Crust crust) {
      this.crust = crust;
      return this;
    }

    public NeedsTopping sauce(Sauce sauce) {
      this.sauce = sauce;
      return this;
    }

    public Bakeable topping(Topping topping) {
      this.topping = topping;
      return this;
    }

    public Pizza bake() { return new Pizza(this); }


    public interface NeedsCrust { NeedsSauce crust(Crust crust); }
    public interface NeedsSauce { NeedsTopping sauce(Sauce sauce); }
    public interface NeedsTopping { Bakeable topping(Topping topping); }
    public interface Bakeable { Pizza bake(); }
  }

  public static NeedsCrust start() { return new Builder(); }
}
{% endhighlight %}

It does not look pretty. But when it is being used it looks just like a regular
builder, but the returned interfaces on each step tell the IDE (and the user)
which is the next method to call. This also prevents a user from calling the
same method multiple times.[^1]

{% highlight java %}

Pizza pepperoni = Pizza.start()
  .crust(Crust.THIN)
  .sauce(Sauce.RED)
  .topping(Topping.PEPPERONI)
  .bake();

{% endhighlight %}

Pretty good. Problem solved.

Wait. I was talking about APIs. What was the point I was trying to make?
All this rambling took so long that I can't even...

Right after so long, that was it. Say after a few months (or the time it took
to get to this point) the Pizzaria decides to make filled crusts. So now you
add a new field to the `Pizza` class, it will receive its value from the
`Builder`. And the `Builder` will add a new step between the crust and the
sauce.

{% highlight java %}

Pizza pizza = Pizza.start()
  .crust(Crust.THIN)
  // set crust filling here
  .sauce(Sauce.RED)
  // ...
{% endhighlight %}

But if the step is mandatory, it will break anyone who had code that did not
have crust filling.

Fear not. Since the crust filling relates to the ... crust, we could add a new
method to the `NeedsCrust` interface.

{% highlight java %}

public interface NeedsCrust {
  NeedsSauce crust(Crust crust); // this could be a default method
  NeedsSauce crust(Crust crust, CrustFilling filling);
}

{% endhighlight %}

Ooph, that was a close one. But what if it was something new that does not
relate to any of the old fields?

Well, then I guess the way to go would be to add a new method to the `Bakeable`
interface that returns a `Bakeable`. While doing this would keep backwards
compatibility, it allows the user to call that method multiple times. Worst
case scenario you'll need to add some defensive coding in the `Builder`.

We solved the problem. Problem solved.

--- 
[^1]: technically the user could always cast the returned type to a `Builder` and access any method.
