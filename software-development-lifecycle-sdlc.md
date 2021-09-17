# Build, Test, Deploy

> Conceptual integrity is the most important consideration in system design.   
> -- Fred Brooks, "The Mythical Man-Month"

The basic pattern of "Build, Test and Deploy" lies at the heart of software development and thus establishes the practical framework for this book. Each step contains a seemingly infinite realm of possibilities, like the Cambrian explosion, yet whichever choices are made along the way each step must be fulfilled to ship software-based solutions.

### Build

**What are you building and for whom?** Before getting into the technical system details, it is usually helpful to understand and articulate the _problem_ you intend to solve with the built system. This often involves describing the _context_ \(the operating domain or environment\), the _users_ \(the end users who interact with the system\), and the _functionality_ \(what the system does\). The more you understand about each of these dimensions, the greater chance of success your system will have in achieving your \(and your user's\) goals.

### Test

**Is your system tested thoroughly to withstand real-world conditions?** While software is mostly written to address a specific need in mind, corner \(or edge\) cases often exist which may affect the system's behavior, given conditions unknown or unforeseen to the programmer at the time of development. Testing the software at various levels \(unit, integration and user interface\) with different methodologies \(manual and automated\) can uncover gaps or vulnerabilities. Covering these gaps early in the development cycle translates directly into cost savings \(in time and reputation\) versus fixing bugs found in production \(i.e. $100 versus $10,000\).

### Deploy

**How do you deliver the system to fulfill your user's needs?** If the value of the system is realized by its use, it should be accessible by its intended user base and well-supported by its developers and maintainers. Web applications are inherently _distributed_ systems, which involve a lot of complexity in their architectures and implementations. Managing this complexity throughout the development pipeline offers  numerous benefits around increased availability, adapting to change quickly, reducing risk, protecting developer morale, and higher profits associated with more readily fulfilling urgent customer needs.

