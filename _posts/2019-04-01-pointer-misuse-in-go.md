---
title: Pointer misuse in Go
date: 2019-04-01 00:00:00
featured_image: "/images/go-pointers/featured.png"
excerpt: Looking at some of the ways pointers get misused in Go and how we can improve them.
---

![Photo by Ashley @ https://github.com/ashleymcnamara/gophers](/images/go-pointers/featured.png)

Pointers are a great way of building software in a more efficient and less memory intensive fashion. I find however, there is a great source of confusion and misuse around their application that lead to less intuitive and often bug prone programs.

Let's see a typical usage of pointers:
{% highlight go %}
    package main
    
    import (
    	"fmt"
    )
    
    type Human struct {
    	Name string
    }
    
    func main() {
    	h := &Human{"JeSsie"}
    	transformName(h)
    	fmt.Println(h.Name) // returns 'JESSIE'
    }
    
    // transformName will uppercase the name of a human
    func transformName(h *Human) {
    	b := make([]byte, len(h.Name))
    
    	for i := range b {
    		c := h.Name[i]
    		if 'a' <= c && c <= 'z' {
    			c -= 'a' - 'A'
    		}
    		b[i] = c
    	}
    
    	h.Name = string(b)
    }
{% endhighlight %}

So, what's the issue with the above code anw?

### Mutation is sneaky at best
Mutation is difficult to tackle when things go wrong. A million things could have altered the state of your program. Now, it's not to say mutation is necessarily bad but in a lot of cases we can improve its visibility. For instance, instead of having `transformName` doing the mutation, have it return a `string`. This will force the propagation of the mutation to happen at a more visible place.

{% highlight go %}
    func main() {
    	h := &Human{"JeSsie"}
    	h.Name = transformName(h)
    	fmt.Println(h.Name) // returns 'JESSIE'
    }
{% endhighlight %}

The above though doesn't really feel idiomatic Go. Moreover, there is a better way of achieving a similar effect to the above.

### Use methods when it comes to mutations
Methods are just functions that have a particular receiver. Attaching behaviour that is operating specifically on a type is the perfect usecase. It also makes code much cleaner.

{% highlight go %}
    func main() {
    	h := &Human{"JeSsie"}
    	h.transformName()
    	fmt.Println(h.Name)
    }
    
    // transformName will upper case the name of a human
    func (h *Human) transformName() {
    	b := make([]byte, len(h.Name))
    
    	for i := range b {
    		c := h.Name[i]
    		if 'a' <= c && c <= 'z' {
    			c -= 'a' - 'A'
    		}
    		b[i] = c
    	}
    
    	h.Name = string(b)
    }
{% endhighlight %}

The above has also the positive side-effect that by scanning the methods of a type, you can see where the mutation happens instead of having to pass pointers around in your application.

### The function signature should be succint and to the point
Type driven development is a way of developing software specifically in pure functional languages like Haskell and Elm. When reading the type signature of a function it should be very clear to the reader what this function will do. Let's have a look at the signature of `transformName`

{% highlight go %}
    func transformName(h *Human) {}
{% endhighlight %}

Hm.. So it takes a pointer to a `Human` and returns nothing. There are a lot of questions that are raised based on this signature. Why does it take a whole `Human` to operate just on its name? 🤔 Is there a possibility that it could also operate on its surname? Does it have different rules based on the `Human`'s locale?

Let's look at the solutions we mention above.

In the case of the function that returns just a string, the type signature is simply

{% highlight go %}
    func transformName(str string) string {}
{% endhighlight %}

Nice! It's very clear to the reader that this function receives a string and returns a string. 🆒

What about the method? Well that's an interesting one. On the surface, it's no better than the initial implementation. I beg to disagree though. By hiding the internal implementation and attaching it to the `Human` type, we have now given full control of details to the struct itself.

### Aim to make functions as "pure" as possible
Purity is a term you normally associate with Haskell but that doesn't mean we cannot make use of it in Go as well. Functions without side-effects makes refactoring so much easier. Let's say we are staring at the initial solution fresh. Practically, that function takes a string and uppercases it. So why does it need to be associated with a `Human` and it's `Name`? It sounds like it could live under the `string` package. [Indeed such function does exist!](https://golang.org/pkg/strings/#ToUpper) But let's assume that it didn't exist. What would our code look like.

{% highlight go %}
    package main
    
    import (
    	"fmt"
    )
    
    type Human struct {
    	Name string
    }
    
    func main() {
    	h := &Human{"JeSsie"}
    	h.Name = uppercaseString(h.Name)
    	fmt.Println(h.Name)
    }
    
    // uppercaseString will return a new uppercased string
    func uppercaseString(str string) string {
    	b := make([]byte, len(str))
    
    	for i := range b {
    		c := str[i]
    		if 'a' <= c && c <= 'z' {
    			c -= 'a' - 'A'
    		}
    		b[i] = c
    	}
    
    	return string(b)
    }
{% endhighlight %}

Great. But we can make it more Go-like based on what we have discussed so far.

{% highlight go %}
    package main
    
    import (
    	"fmt"
    )
    
    type Human struct {
    	Name string
    }
    
    func main() {
    	h := &Human{"JeSsie"}
    	h.transformName()
    	fmt.Println(h.Name)
    }
    
    func (h *Human) transformName() {
    	h.Name = uppercaseString(h.Name)
    }
    
    // uppercaseString will return a new uppercased string
    func uppercaseString(str string) string {
    	b := make([]byte, len(str))
    
    	for i := range b {
    		c := str[i]
    		if 'a' <= c && c <= 'z' {
    			c -= 'a' - 'A'
    		}
    		b[i] = c
    	}
    
    	return string(b)
    }
{% endhighlight %}

Whether is worth the extra step or not is up to the programmer and the circumstances.

Up until this point, I mentiond that pointers are mostly an optimization strategy which is slightly misleading. In his book [Go in Action](https://www.manning.com/books/go-in-action), [Bill Kennedy](https://twitter.com/goinggodotnet) goes in great detail about pointers semantics. Pointer semantics are _the_ way we should think about the way we build software in Go. Performance can come later.

Pointer semantics allude to the idea that certain entities in your software will need to propagate any changes to the rest of the program. We see this a lot with configuration and structs that represent table data. Let's say we have a User table and we have a `User` struct type. Does a change in the `User`'s email need to be realised by another part of the program that is operating on that entity? If this answer is yes, mostly likely you need to be using a pointer to represent that entity.

### Pointers semantics are not an excuse for nullable types
Having a function receiving or returning a pointer, means that function can receive or return `nil`. I find this pattern to be code smell. Let's look at an example.

{% highlight go %}
    package main
    
    type User struct {
    	Email string
    }
    
    func main() {
    	u := findUser("email@icloud.com")
    	if u == nil {
    		panic("OMG!!")
    	}
    }
    
    // findUser performs a DB lookup and returns a User
    func findUser(str string) *User {
        // Ooops we couldn't find a user
    	return nil
    }
{% endhighlight %}

At first glance, this makes sense. However, we have used the fact that `findUser` returns a pointer to sneak in a `nil` value. This is bad in my opinion for two reasons.

### Nil sucks 🔥
In languages like Ruby where types are not a thing, `nil` is pretty much the only way to represent the absence of a value. But with Go we can do so much better.
{% highlight go %}
    package main
    
    type User struct {
    	Email string
    }
    
    func main() {
    	u := findUser("email@icloud.com")
    	if u.Email == "" {
    		panic("OMG!!")
    	}
    }
    
    // findUser performs a DB lookup and returns a User
    func findUser(str string) *User {
    	// Ooops we couldn't find a user
    	return &User{}
    }
{% endhighlight %}

Yeah I said it! Just instantiate an empty `User` struct and utilize Go's default values. 

What about an even better way?

{% highlight go %}
    package main
    
    import "errors"
    
    type User struct {
    	Email string
    }
    
    func main() {
    	_, err := findUser("email@icloud.com")
    	if err != nil {
    		panic("OMG!!")
    	}
    }
    
    // findUser performs a DB lookup and returns a User
    func findUser(str string) (*User, error) {
    	// Ooops we couldn't find a user
    	return nil, errors.New("Sorry m8, couldn't find user")
    }
{% endhighlight %}

This approach, builds on the whole idea of Go's multiple returns with the last one being an `error`. Now our code is super clear. Having `findUser` being responsible knowning about errors also helps to customize the type of errors that we could have and have the caller act accordingly.

### IMO my code shouldn't be affected based on pointer semantics
Obviously working with pointers forces you to think and structure your application in a different way. I argue that it shouldn't really matter the way we design our interactions between our functions.
Let's flip it on it's head a bit and see our functions as if it was returning a value instead of a pointer.

{% highlight go %}
    package main
    
    type User struct {
    	Email string
    }
    
    func main() {
    	u := findUser("email@icloud.com")
    	if u.Email == "" {
    		panic("OMG!!")
    	}
    }
    
    // findUser performs a DB lookup and returns a User
    func findUser(str string) User {
        // Ooops we couldn't find a user
    	return User{}
    }
{% endhighlight %}

In this case we cannot just return `nil` which would have forced us to use one of the ways we experimented with earlier. Naturally, and given practical requirements, we would have opted in for returning an `error` as second argument. The convenience of being able to return `nil` however, clouded the way we should have looked at that piece of code in the first place.

To recap: pointer semantics should be the driving force when using pointers. Performance can come later. Avoid passing `nil` when pointers are expected.

