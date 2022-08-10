# Unbuffered channel

**The sender blocks until the receiver has received the value**

```go
// create unbuffered channel
c := make(chan int)

list := [6]int{1, 2, 3, 4, 5, 6}

// let's send data througth the channel
go func() {
  for _, item := range list {
    c <- item
  }
  close(c) // sending in closed channel panic
}()

fmt.Println(<-c) 
fmt.Println(<-c)
```
Open in [go play dev](https://goplay.tools/snippet/u2dRHHr_Ski)
 
`item 1 is sent and received`  
`item 2 is sent and received`  
`item 3 is sent and sender block until it is received`  

> 🚨 goroutines are not garbage collected, this code create a memory leak. we will talk about some techniques to prevent this from happening  
---
# Buffered channel

**The sender blocks only until the value has been copied to the buffer; if the buffer is full, this means waiting until some receiver has retrieved a value.**

```go
list := [6]int{1, 2, 3, 4, 5, 6}

c := make(chan int, len(list))

go func() {
  for _, item := range list {
    c <- item
  }
  close(c) 
}()

time.Sleep(time.Second * 5)
fmt.Println("Nb Goroutine", runtime.NumGoroutine())

fmt.Println(<-c)
fmt.Println(<-c)

// output: 
// Nb Goroutine: 1
// 1
// 2
```
Open in [go play dev](https://goplay.tools/snippet/Hbs_XL3yRHK)

we create a buffered channel with enought space to send all list items, then we close the channel and exit the goroutine.

Number of goroutines is equal to 1 because main function run in a goroutine.


```go
list := [6]int{1, 2, 3, 4, 5, 6}

c := make(chan int, len(list)-2)

go func() {
  for _, item := range list {
    c <- item
  }
  close(c) 
}()

time.Sleep(time.Second * 5)
fmt.Println("Nb Goroutine", runtime.NumGoroutine())

fmt.Println(<-c)
fmt.Println(<-c)

time.Sleep(time.Second * 1)
fmt.Println("Nb Goroutine", runtime.NumGoroutine())

// output: 
// Nb Goroutine: 2
// 1
// 2
// Nb Goroutine: 1
```
Open in [go play dev](https://goplay.tools/snippet/acKSZNZB8JV)

We create a buffered channel with capacity of 4, the list length is 6.  
The sender copy 4 items in it's buffered and block until a value is received which free one space in the buffer.  
The sender copy the next item, it repeat this scenario until all the items are copied in the buffer; it then close the sender channel and exit the goroutine.  