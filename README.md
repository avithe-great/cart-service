# How does the Ginkgo golang framework work?

Ginkgo is a BDD-style testing framework for Go that provides a clear structure for organizing and writing tests. It follows a hierarchical structure where tests are organized into nested contexts and can be described using natural language constructs.

## Brief Introduction to BDD:
BDD is familiar with test-driven development (TDD), you can think of BDD as something similar but at a higher level.

While TDD focuses on the technical, or implementation details, BDD focuses on visible, behavioral details. Another way to think about it is that TDD focuses on ensuring that the individual parts of the application do what they should, while BDD focuses on ensuring that these parts work together as expected.

## Here's a high-level overview of how Ginkgo works:

- `Installation:` You can install Ginkgo using the go get command:
`go get github.com/onsi/ginkgo/ginkgo`
- `Test File Structure:` Ginkgo tests are typically written in separate test files. Each test file is paired with a corresponding _test file, which contains the actual test code.

- `Describe-Context-It Hierarchy:` Ginkgo tests are organized using a hierarchical structure. The top-level construct is the Describe block, which represents the test suite. Inside the Describe block, we can have nested Context blocks, which group related tests together. Finally, within the Context block, you define individual test cases using the It block.


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
In this diagram, `Describe` represents the highest-level block in Ginkgo. It is used to define a test suite and contains one or more Context blocks.

`Context` represents a block used to group related test cases within a test suite. It can contain one or more It blocks.

`It` represents an individual test case. It contains the actual test logic and assertion statements.

By using this hierarchical structure, we can organize your tests in a descriptive and meaningful way, making it easier to understand the behavior being tested.



- `Setup and Teardown:` Ginkgo provides BeforeEach, AfterEach, BeforeSuite, and AfterSuite hooks that allow us to set up and tear down common test dependencies. These hooks are defined using the BeforeSuite, AfterSuite, BeforeEach, and AfterEach functions provided by Ginkgo.
`BeforeSuite:` The BeforeSuite hook is executed once before any test suites are run. It is typically used to perform setup tasks that are common to all test suites. For example, initializing a test database or setting up global test variables can be done in this hook.
`AfterSuite:` The AfterSuite hook is executed once after all test suites have completed. It is commonly used to perform cleanup tasks or teardown operations that are required after all tests have finished running. Examples include closing database connections or cleaning up temporary files.
`BeforeEach:` The BeforeEach hook is executed before each individual test case within a test suite. It is used to set up any necessary state or perform common setup tasks specific to each test case. For instance, if you need to initialize certain variables or resources before each test, you can do so in this hook.
`AfterEach:` The AfterEach hook is executed after each individual test case within a test suite. It is typically used for cleanup operations or resetting state after each test. If you need to release resources or revert changes made during a test case, you can do so in this hook.

# cart-service
## Introducing the Application
The application that we will build is a simple shopping cart. It’s a simple application but it will help us demonstrate how Behavior-driven Development(BDD) can help develop,it in a manner that engages all of the stakeholders, and ensures that the final product matches the original expectations.
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

