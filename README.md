
![alt text](https://fahmirahman.files.wordpress.com/2011/03/go2.png "Let's Go!")


**Brought to you by:** *Elisha Patterson*

On your mark, Get set... **Go**! Hello there and welcome! Today, we’re going to be exploring some of the intricacies of a new and upcoming language designed by Google called Go. We’ll be examining the syntax along some of the key features it provides. We will follow up by diving into some of the pros and cons identified up to this point in time and leave you, the reader, with a more well-rounded perspective that will allow you to decide if the upsides outweigh the current downsides for yourself and/or your organization. So let’s get into it!

The Go programming language was designed collectively by three gentleman working at Google (and still do to this day): *Robert Griesemer*, *Rob Pike*, and *Ken Thompson*. Go’s mascot is the gopher designed by Rob Pike’s wife, Mrs. Renee French. The language was officially announced in November of 2009 and has caught a lot of fire ever since being utilized in companies such as *DropBox*, *Google (obviously)*, *SoundCloud*, *CloudFare*, *Docker*, and *Cloud Foundry*. Even the UK government’s primary website was built with Go (https://www.gov.uk/). 


The goal of these Go pioneers was to make a language that made it easy to build simple, reliable, and efficient software that would eliminate many issues found in other languages while still maintaining their positive characteristics. For example, they decided that they wanted the language to be statically typed and scalable to large systems like Java and C++ do. They wanted to increase the developer’s productivity and readability of their code without an overdose of mandatory keywords and excessive repetition. Additionally, as we’ll touch on in a moment, they sought to remove the necessity of tooling while still being able to support it just as adequately. The same went for networking and multiprocessing (concurrency). Thus, they wanted to create a hybrid between statically typed and dynamic languages that was comprised of the “best of both worlds”. As you’ll notice, Go does not run on a VM, but runs in native machine code on variety of architectures. These designers went on to express their unified distaste for the immense complexity found in C++ as their key motivation to design the new language. 

Now let’s look at a few examples of Go’s language to give you a feel for how it operates. How about we begin at the starting place for every language and *Go* from there: **_Hello World!_**

###Hello, Go!###
```
package main

import "fmt"

func main() {
	fmt.Println("Hello, 世界!")
}
```

As we can see, we have a main package as all Go code belongs to a package along with the ‘fmt’ import statement which is the String formatting package. Finally in the main function, we just call “Hello Go” from the Println method. 

###Multiple results###
An interesting feature about functions in Go is that they can return more than one result.
```
package main

import "fmt"

func swap(x, y string) (string, string) {
	return y, x
}

func main() {
	a, b := swap("hello", "world")
	fmt.Println(a, b)
}
```
In this example, the swap function returns two strings. 




###Type inference###
As with dynamically typed languages such as Ruby or Python, Go offers the ability to infer the data type of your variables without you needing to declare them explicitly. Notice the := notation with the colon in front of the equals to assign values to variables.
```
package main

import "fmt"

func main() {
	v := 42 // change me!
	fmt.Printf("v is of type %T\n", v)
}
```
###For###
The for loop is Go’s one and only looping mechanism. The for loop has three components separated by semicolons:
The init statement (executed before the first iteration)
The condition statement (evaluated before every iteration)
The post statement (executed at the end of every iteration)
The init statement will generally be a short variable declaration with the variables declared being visible only in the scope of the for loop. 






```
package main

import "fmt"

func main() {
	sum := 0
	for i := 0; i < 10; i++ {
		sum += i
	}
	fmt.Println(sum)
}
```
###Defer###
Defer statements delays (“differs”) the function from executing until the surrounding function returns. The deferred call’s arguments are executed immediately, but the actual function call isn’t executed until the surrounding function returns. 
```
package main

import "fmt"

func main() {
	defer fmt.Println("world")

	fmt.Println("hello")
}
```
###Pointers###
Go features the concept of pointers. Unlike C, Go does not have pointer arithmetic. The type *T is a pointer to a T value. Its zero value is nil. The & operator generates a pointer to its operand and the * operator represents the pointer’s underlying value. This action is known as “dereferencing” or “indirecting”.  





```
package main

import "fmt"

func main() {
	i, j := 42, 2701

	p := &i         // point to i
	fmt.Println(*p) // read i through the pointer
	*p = 21         // set i through the pointer
	fmt.Println(i)  // see the new value of i

	p = &j         // point to j
	*p = *p / 37   // divide j through the pointer
	fmt.Println(j) // see the new value of j
}
```
###Slices###
An array has a fixed size. A slice, on the other hand, is a flexible, dynamically sized view into the elements of said array. On a practical basis, slices are much more commonly used than arrays. The type []T is a slice with elements of type T. This expression below creates a slice of 3 elements of the array primes.
```
package main

import "fmt"

func main() {
	primes := [6]int{2, 3, 5, 7, 11, 13}

	var s []int = primes[1:4]
	fmt.Println(s)
}
```
###Maps###
A map will map keys to values. Simple as that. The zero value of a map is nil. A nil map has no keys and nor can any keys be added. The make function returns a map of the given type, initialized and ready to use. 
```
package main

import "fmt"

type Vertex struct {
	Lat, Long float64
}

var m map[string]Vertex

func main() {
	m = make(map[string]Vertex)
	m["Bell Labs"] = Vertex{
		40.68433, -74.39967,
	}
	fmt.Println(m["Bell Labs"]) 
}
```
###Goroutines###
Goroutines are lightweight threads managed by the Go runtime. Go features some pretty sweet concurrency primitives that make modeling concurrent processes very straightforward. Many goroutines run on just a few operating system threads. To run a function in a new go routine, simply put “go” before the function call like so: 
```
Package main

Import (
          “fmt”
          “Time”
)

Func main() {
     go say(“let’s go!”, 3*time.Second)
     go say(“ho!”, 2*time.Second)
     go say(“Hey!”, 1*time.Second)
     time.Sleep(4 * time.Second)
}

Func say(text string, delay time.Duration) {
     time.Sleep(delay)
     fmt. Println(text)
}
```
###Channels###
Channels are a means (conduit) for synchronization and communications through which you can send and receive values with the channel operator, ‘<-’. You send a message from one end and the other routine will receive it at the other. The data flows in the direction of the arrow. 
```
package main

import "fmt"

func sum(s []int, c chan int) {
	sum := 0
	for _, v := range s {
		sum += v
	}
	c <- sum // send sum to c
}

func main() {
	s := []int{7, 2, 8, -9, 4, 0}

	c := make(chan int)
	go sum(s[:len(s)/2], c)
	go sum(s[len(s)/2:], c)
	x, y := <-c, <-c // receive from c

	fmt.Println(x, y, x+y)
}
```

##Pros and Cons##
Now that we’ve covered a number of the core features offered by Go (at least up to this point- there are many more!), let us take a look at some of the pros and cons of utilizing the language. Please note: a lot of the factors that weigh into determining if Go is the right fit for you depend largely upon your specific needs. For example, are you considering going with Go for personal use? A new startup? Or perhaps a more heavy-duty corporate production environment? So be sure to take these questions into account when considering using Go or offering advice to others who are doing the same.

###The Pros###
* Go (adequately named) is lightning fast! It is fast both in terms of the programs written in by comparison to those written in other languages as well as by means of the compiler. 
* Go can to edit and run programs directly from the web (try it out!).
* Go is a garbage-collected language which puts less pressure on the developer to do memory management as Go has most of this “grunt work” functionality already built in (cool, huh?)
* Built-in concurrency! This makes provision for parallelism in a simpler way than is currently possible in other languages. This is demonstrated in the goroutines and channels we looked at earlier. The goroutines start concurrent work and channels support the communication and synchronization.
* Has documentation already built in as a standard feature making it easier for developers like ourselves to document our code and generate data straight from pseudo code into a comprehensive, human-legible format.
* Possesses a rich standard library. Go is likely the only language to date that can claim to have a fully functional web server built in as a part of the standard library itself. 
* Built-in build system that is quite elegant and simple removing the need to deal with build configurations or makefiles. 
* Corporate backing and financial support from one of the most renowned tech giants today, Google. Nowadays, there are even more contributors from outside Google in the open source community than internally.
* The simplicity makes code maintainability less costly. In a production environment, roughly 10% of the cost goes towards writing the code and the other 90% goes into maintaining it. Go’s simplicity cuts down immensely on this maintainability expense. 

###The Cons###
* Go still has, albeit growing, a very young ecosystem meaning there aren’t many libraries designed for it yet which, in turn, means developers wind up having to write these libraries themselves. Also, not many books and courses are provided for it at this point.
* Go can be simple to a fault. Go’s simplicity is mostly on the surface in the sense that as it has sought to increase its simplicity, it has tossed away decades of valuable programming language progress.
* Go’s tooling is a bit light. On the surface it has some sweet tools to work with, but as you start utilizing them, a good portion of them begin to show their limitations.
* Go is still not an easy language to pick up on-the-fly and can be cumbersome at times to handle errors in it. 

##Where will you Go from here?##
To recap, Go is a relatively new language (at least for the time this was written: 2016) that has not yet even crossed the mark of a decade and already we’re seeing a big surge among those in and out of Google to continue to support it. In the first 3 years since its conception in 2009, over 300 contributors joined the Go project in the open source community and that number of advocates has continued to thrive ever since.

Thank you for taking the time to read (or even skim! :)) through all that. If you’d like to learn more about Go and stay updated on the latest they have to offer, be sure to visit https://golang.org/ along with any of the handy sources listed below.

###Get *Go*ing Today!###




###Credits rolling (with theme music playing):###
https://altabel.wordpress.com/2015/11/10/golang-pros-and-cons/
https://speakerdeck.com/railsberry/go-a-simple-programming-environment-by-andrew-gerrand
http://golang-examples.tumblr.com/
https://vimeo.com/69237265
https://tour.golang.org/list
http://www.infoworld.com/article/2896575/google-go/googles-go-language-pros-and-cons.html
