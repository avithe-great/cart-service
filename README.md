## cart-service
This repo will explain how you can incorporate behavior-driven development with Go using Ginkgo, with the help of a sample application. Ginkgo makes use of Go’s existing testing package.
BDD stands for behavior-driven development. To accomplish stated business goals, the developer must define certain behaviors based on the vision communicated by the client or product manager. Afterward, the tester checks if the new feature meets the "why" behind it.

For example, if a client said they had found in their research that they had a lot of older users handling their application, and they needed an accessible solution, you, as the developer following a BDD model, would consider how this behavior changes which features you would add to the application - perhaps larger fonts and easily clickable items.

BDD specifications are usually written in non-tech language to enable non-technical stakeholders to participate in their development. As a result, the development and final product are aligned with stakeholders' expectations.

Go has a fairly capable built-in testing package. However, it does have a few limitations. This article will explain (with some simple examples) how we use Ginkgo and Gomega at Bold to write more expressive and structured unit tests in Go. As BDD is a higher version of test-driven testing (TDD), so let’s see what TDD is.
