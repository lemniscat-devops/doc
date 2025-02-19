## Breaking Down Barriers: My Journey into DevOps, Cloud, and Platform Engineering

![Lemniscat](img/lemniscat.png)

For several years now, I've had the privilege of assisting various clients across different sectors in their digital transformation strategies. With two decades of experience under my belt, I've held numerous positions ranging from support to development, including system and network administration.

### Wall of Confusion and the DevOps Movement

Early on, I noticed the strange gap that separated the world of administrators from that of developers. As a system and network administrator, I often found developers to be brusque and less demanding. How many times have I grumbled because the installation or update manual was poorly written? How many times have I struggled to perform an operation on software because it wasn't documented in the operational manual (if there was one)?

On the flip side, when I worked as a developer, I found system administrators to be excessively procedural. "Did you submit request X by filling out form Y?" And when administrators finally performed the installation or update of the software, they messed up the installation procedure. In short, I experienced the famous "wall of confusion" from both sides.

When the DevOps movement emerged, I quickly became enthusiastic. "Of course, it's obvious! Why didn't we think of this sooner?" Of course, I encountered many people resistant to this change, advancing arguments of varying validity.

Ultimately, I realized that this was my calling. I believed in the importance of advocating for DevOps, even if it meant convincing one person at a time.

### The Cloud as a opportunity to break down the wall

Then, with the advent of the Cloud in my work environment, my enthusiasm was reignited. "This is fantastic! We finally have an opportunity to break down the wall." The cloud allowed us to provision technical infrastructures quickly, reducing constraints related to complexity and deployment time. This was beneficial for developers who could expect to deliver their work more rapidly. Additionally, the platform approach of these environments also greatly simplified tasks for our system and network administrators.

Fast forward several years, and I've specialized in Cloud and DevOps adoption. While clients have relatively smoothly transitioned to the Cloud, implementing DevOps practices within companies has proven to be challenging. Although there are some beautiful success stories, in my opinion, they remain too rare.

### The Platform Engineering Movement

Through years of experience and extensive reading of influential authors on the subject like Jez Humble, Gene Kim, Matthew Skelton, Nicole Forsgren, I've come to understand that at the enterprise level, the approach must be comprehensive. Each software component comprising the company's IT system must be designed to be coherent as a whole.

The mental load that a DevOps approach can bring tends to cause teams to focus unidirectionally on their application's sole objective for which they are responsible. It's as if each team is running a sprint in their own lane, forgetting about other teams. The confusion is no longer just between developers and administrators but now between project teams. So, what can we do?

Taking a step back, I noticed that Cloud providers offer platforms that lighten our responsibility and mental load while still keeping us accountable. For example, when I create a function app on Azure, I don't have to worry about the underlying infrastructure: data centers that need maintenance, network infrastructures that need to be operated, software that needs to be kept up to date. Yet, I remain responsible for my function app. If the code I deploy doesn't work, if my function isn't monitored, it's still my responsibility.

This platform allows me to be more efficient while remaining autonomous and responsible. This concept of a platform promoted by the "Platform Engineering" movement may be the way forward.

### The DevOps-Oriented Product

It's been two years since I've been working with my colleagues to implement this concept. We're establishing a platform for project teams to enable them to be as autonomous and responsible as possible while limiting their mental load. The experience has been extremely promising. We're building DevOps-oriented products that we make available on our platform's marketplace.

A DevOps-oriented product is one designed to address all aspects of the DevOps approach for the consumer of the product.

Let's take the example of the Azure function app. If we're to design a DevOps-oriented product to allow our teams to be autonomous and responsible for this function app, we need to ask the following questions:

- How will my consumer produce the necessary code? With Git?
- How will my consumer build their code? With a GitHub CI pipeline?
- How will my consumer test their code? With SonarQube, Cucumber...?
- How will my consumer release their code? With Azure Artifact?
- How will my consumer deploy their code? With Azure DevOps?
- How will my consumer operate their code? With an Azure Function App?
- How will my consumer monitor their code? With Azure Monitor?
- How will my consumer manage bugs and code evolutions? With Jira?

### The Lemniscat Framework

Implementing this type of product requires a lot of work. Integrating into a platform the multitude of technological solutions that compose a company's information system is no easy task.

Products designed for delivery typically rely on the same delivery solutions as the product itself when instantiated, making our product hyper-aligned with the CI/CD chain. Given the complexity of the product, this makes the product highly specific to its context and difficult to extend to another context (another project or another company).

How can we design a product more simply? How can we reduce the product's adherence to the technological solutions that compose it, and particularly to the tools of the CI/CD chain?

I searched for weeks and months. I saw several solutions like Dagger.IO, but none convinced me. So, at the end of 2023, I decided to create my own framework.

I am honored to present to you the newcomer: **Lemniscat**.

#### Why this name?

The Lemniscate is a mathematical curve that evokes the symbol of infinity, representing an endless continuity or an infinite loop. In parallel, the "Lemni's cat" is a fictitious character with no specific connection to any particular work, whose name creates an intelligent play on words with the Lemniscate. This association between a fictional character and a mathematical shape gives rise to an intriguing concept named **Lemniscat**.

The Lemniscate is a mathematical curve that takes the shape of an elongated loop, closely resembling the infinity symbol (∞). This curve has interesting properties and is often used to represent concepts of infinite continuity or endless cycles.

"Lemni's cat" is a fictitious character imagined to illustrate the play on words between the Lemniscate and a fictional entity. This cat is not associated with any specific work but is simply used as an example to highlight the phonetic resemblance between "Lemni's cat" and "Lemniscate".

The DevOps approach is a software development methodology that emphasizes collaboration between development and operations teams, as well as the automation of processes. The infinity symbol has become the emblem of DevOps, symbolizing the continuous cycle of planning, development, testing, deployment, and monitoring of software.

The Lemniscate, with its shape reminiscent of the infinity symbol, along with the play on words with "Lemni's cat," creatively illustrate the notion of continuity and infinite cycles, in perfect harmony with DevOps principles.

By associating the Lemniscate, Lemni's cat, and the infinity symbol with the DevOps approach, we get an intriguing illustration of the convergence between mathematics, fiction, and software development practices. This intelligent play on words highlights the cyclical and continuous nature of the development process, underscoring the ingenuity of the connection between these different concepts.

#### What is the goal of Lemniscat?

The goal of Lemniscat is to provide a framework that allows the design of DevOps-oriented products that are as independent as possible from the technological solutions that compose them, particularly from the tools of the CI/CD chain.

The Lemniscat framework is designed to provide a set of guidelines and best practices for designing DevOps-oriented products. By following these guidelines, product designers can create products that are more flexible and adaptable to different contexts, making them easier to extend and integrate into various environments.

The Lemniscat framework is intended to help product designers create products that are more aligned with the principles of DevOps, promoting autonomy, responsibility, and accountability for the consumers of the products.

The lemniscat framework is like a meta framework that can operate many other framework (terraform, ansible, powershell, python, ...) to create a DevOps-oriented product.