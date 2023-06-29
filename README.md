# cart-service
## Introducing the Application
The application that we will build is a simple shopping cart. It’s a simple application but it will help us demonstrate how Behavior-driven Development(BDD) can help develop.
Let’s start by listing our requirements for this shopping cart:

* It should allow the user to add items,
* It should allow the user to remove items,
* It should allow the user to retrieve the number of unique items at any point,
* It should allow the user to retrieve the total number of units (individual pieces) of items at any point, and
* It should allow the user to retrieve the total amount at any point.

## Building a Specification Based on Requirements
In this section, we’ll start by writing out some specifications based on the above requirements. 
For example, the requirement that a shopping cart should initially be empty can be written as follows:

```
Given a shopping cart
  initially
    it has 0 items
    it has 0 units
    the total amount is 0.00
```
Grouping of specifications can be indicated using indentation. This particular specification can be written using Ginkgo as follows:

```go
Describe("Shopping cart", func() {
	Context("initially", func() {
		It("has 0 items", func() {})
		It("has 0 units", func() {})
		Specify("the total amount is 0.00", func() {})
	})
})
```
In this case, we can see that there will be three related assertions in the same ‘context’. You can group other related assertions and/or contexts together in a similar manner. Specifications can be mapped quite easily to tests using the constructs provided by Ginkgo.

Note that Specify is just an alias for It, and has only been used because it reads more naturally in this case. This means that the particular block could be replaced with

```go
It("the total amount is 0.00", func() {})
```
without changing how the tests run.
## Mapping Requirements to Specifications
Now that we know how to write the specifications and how they will be mapped to test code in Go, we can write the entire specification as follows:
```
Given a shopping cart
  initially
    it has 0 items
    it has 0 units
    the total amount is 0.00

  when a new item is added
    the shopping cart has 1 more unique item than it had earlier
    the shopping cart has 1 more unit than it had earlier
    the total amount increases by item price

  when an existing item is added
    the shopping cart has the same number of unique items as earlier
    the shopping cart has 1 more unit than it had earlier
    the total amount increases by item price

  that has 0 unit of item A
    removing item A
      should not change the number of items
      should not change the number of units
      should not change the amount

  that has 1 unit of item A
    removing 1 unit item A
      should reduce the number of items by 1
      should reduce the number of units by 1
      should reduce the amount by the item price

  that has 2 units of item A
    removing 1 unit of item A
      should not reduce the number of items
      should reduce the number of units by 1
      should reduce the amount by the item price

    removing 2 units of item A
      should reduce the number of items by 1
      should reduce the number of units by 2
      should reduce the amount by twice the item price

```

Block Diagram of ginkGo-
```sql
              +-------------------------------+
              |           Describe            |
              |_______________________________|
              |                               |
              |       +-----------------+     |
              |       |    Context      |     |
              |       |_________________|     |
              |       |                 |     |
              |       |      It         |     |
              |       |_________________|     |
              |       |                 |     |
              |       |      It         |     |
              |       |_________________|     |
              |                               |
              |       +-----------------+     |
              |       |    Context      |     |
              |       |_________________|     |
              |       |                 |     |
              |       |      It         |     |
              |       |_________________|     |
              |                               |
              |_______________________________|

```
