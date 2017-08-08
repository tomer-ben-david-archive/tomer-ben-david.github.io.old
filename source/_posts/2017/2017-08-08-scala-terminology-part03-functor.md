---
title: Scala, Learning FP By Terminology - Functor
---
**Introduction**

`functor`, who named lambda lambda? why did they call it lambda, is it simple or complex?

In [Part 2](https://devatrest.blogspot.com/2017/07/scala-learning-by-understanding.html) we have discussed what lambda and map means, why they are named as so, how they are defined and work.  In this post we are going to move on and discuss more `FP` terminology.  Specifically the weird term `functor` and 



**functor**

Well we are again not surprised that functor is a term coming from mathematics, let's see how wikipedia defines this mathematical term don't worry if you absolutely nothing from this definition we would get back to it, the definition is scary!

[Functor math term by wikipedia definition](https://en.wikipedia.org/wiki/Functor)

> In mathematics, a functor is a type of mapping between categories arising in category theory. Functors can be thought of as homomorphisms between categories

Very unhelpful! we already said our posts is not directed to math oriented people.  But you know what i'm sure someone with both mathematical background could define it better for us and we are going to do that, let's go for it! we are going to discuss much more clearly what `functor` means!

So we have `lambda` we already said what it is: \for our sake is an anonymous function, we have map, which just tells us to go over each item and apply a function, let's get to `functor`.

according to a great [quora answer](https://www.quora.com/Functional-Programming-What-is-a-functor) about what a `functor` is:

> Any type f with a function like this (map) is a functor, with one additional restriction: the map function has to preserve the "structure" of the value it's mapping over

Aha this makes much more sense! so if this answer is correct then any type which has the `map` function is a functor! Let's look a little more on some more resources lets see if they agree:

The great book **[Scala Design Patterns by Ivan Nikolov](https://devatrest.blogspot.co.il/2017/07/scala-design-patterns-book-review.html)** confirms that:

>You can conclude that standard Scala types such as List, Option, and others that define a map method are functors.

The amazin book **[Learn You A Haskell For A Great Good](https://devatrest.blogspot.com.il/2017/08/book-review-learn-you-haskell-for-great.html)** confirms that: 

>the Functor type class, which is for things that can be mapped over

More interestingly the book says that the best way to understand functor is simply to look at it's definition! so it's definition in haskell is:

```haskell
class Functor f where
  fmap :: (a -> b) -> f a -> f b
```

so a Functor is just a class (type class but we didn't talk about it yet), so it's just a class with a single function! `fmap`.  This goes along extremely well with the fact that we said that functor is a type which has the function `map`.  But why `fmap` in our case and not `map` this is because we are referring here to the general case.  In our case here it's the definition of what a functor is, and it is any class which defines a function which takes a higher order function from `(a -> b)` from any a to any b. now the functor value `f a` when applied with the function `(a -> b)` evaluates to the functor value `f b` that's it.  If our `f` is an array it would simply be:

```haskell
map :: (a -> b) -> [a] -> [b]
```

a function `(a -> b)` applied to functor value `[a]` evaluates to functor value `[b]` yes `[b]` is also a functor value because we can also apply map to it!

So functor refers to the types which have the map function and that the output structure is same as the original structure.

Do you have things that you can map over and get to the same structure only with different items? you do this all day, this is what a functor is, it's this thing you can map over and get with the same structure to the same structure only with different items.

so as we see it the functor is the thing that has a map function where we have a restriction that the output structure is the same as the input structure.  For list we want a list.

So let's get back now to the scary wikipedia mathematical definition:

[Functor math term by wikipedia definition](https://en.wikipedia.org/wiki/Functor)

> In mathematics, a functor is a type of mapping between categories arising in category theory. Functors can be thought of as homomorphisms between categories

and [wikipedia definition of homomorphism](https://en.wikipedia.org/wiki/Homomorphism)

>In algebra, a homomorphism is a structure-preserving map between two algebraic structures of the same type (such as two groups, two rings, or two vector spaces). The word homomorphism comes from the ancient Greek language: ὁμός (homos) meaning "same" and μορφή (morphe) meaning "form" or "shape".

if a functor can be thought of as a homomorphism and homomorphism is a structure-preserving map between two agebric structures of the same type this is exactly what our `map` is so functor is the type of mapping which maps between ategories, and the type which is functor is the type which has a map!

Now that we have covered `functor` let's see what functors are in scala:

for example if we have the type `MyOption`

```scala
trait MyOption[A] {
    def map[B](f: A => B): Option[B] = if (isEmpty) None else Some(f(this.get)
}

trait MySome[A] {
  def isEmpty = false
}
```

**FP Impurity**

As a final word I would like to talk about FP and impurity.

I know that sounds like an oxymoron but FP has impurity why? because it's a cruel world, you have to do IO.  FP has no solution for that.  FP will tell you do your pure core as big as you can and wrap that impure stuff with IO, it won't turn it to pure sutff it just wraps it in IO.  FP tells you well if you cannot make this function pure at least tell us in it's signature it's impure.  We like function signatures in FP.

Martin Odersky says [Questioning FP](https://webcache.googleusercontent.com/search?q=cache:Azjq01tGknsJ:https://groups.google.com/d/topic/scala-debate/xYlUlQAnkmE+&cd=2&hl=en&ct=clnk&gl=il)

>The IO monad does not make a function pure. It just makes it obvious
 that it's impure.
 
 So there is no silver bullet, if you start usimg IOMonad it just going to make your explicitly say that they are impure.
 
 
**Conclusion**

I think we have covered the basics terms here, `map`, `functor`, `lambda`, and discussed a little bit of impurity and how it goes along with FP.  Stay tuned for more items on our terminology list.