# Practical Session #1: Introduction

1. Find in news sources a general public article reporting the discovery of a software bug. Describe the bug. If possible, say whether the bug is local or global and describe the failure that manifested its presence. Explain the repercussions of the bug for clients/consumers and the company or entity behind the faulty program. Speculate whether, in your opinion, testing the right scenario would have helped to discover the fault.

2. Apache Commons projects are known for the quality of their code and development practices. They use dedicated issue tracking systems to discuss and follow the evolution of bugs and new features. The following link https://issues.apache.org/jira/projects/COLLECTIONS/issues/COLLECTIONS-794?filter=doneissues points to the issues considered as solved for the Apache Commons Collections project. Among those issues find one that corresponds to a bug that has been solved. Classify the bug as local or global. Explain the bug and the solution. Did the contributors of the project add new tests to ensure that the bug is detected if it reappears in the future?

3. Netflix is famous, among other things we love, for the popularization of *Chaos Engineering*, a fault-tolerance verification technique. The company has implemented protocols to test their entire system in production by simulating faults such as a server shutdown. During these experiments they evaluate the system's capabilities of delivering content under different conditions. The technique was described in [a paper](https://arxiv.org/ftp/arxiv/papers/1702/1702.05843.pdf) published in 2016. Read the paper and briefly explain what are the concrete experiments they perform, what are the requirements for these experiments, what are the variables they observe and what are the main results they obtained. Is Netflix the only company performing these experiments? Speculate how these experiments could be carried in other organizations in terms of the kind of experiment that could be performed and the system variables to observe during the experiments.

4. [WebAssembly](https://webassembly.org/) has become the fourth official language supported by web browsers. The language was born from a joint effort of the major players in the Web. Its creators presented their design decisions and the formal specification in [a scientific paper](https://people.mpi-sws.org/~rossberg/papers/Haas,%20Rossberg,%20Schuff,%20Titzer,%20Gohman,%20Wagner,%20Zakai,%20Bastien,%20Holman%20-%20Bringing%20the%20Web%20up%20to%20Speed%20with%20WebAssembly.pdf) published in 2018. The goal of the language is to be a low level, safe and portable compilation target for the Web and other embedding environments. The authors say that it is the first industrial strength language designed with formal semantics from the start. This evidences the feasibility of constructive approaches in this area. Read the paper and explain what are the main advantages of having a formal specification for WebAssembly. In your opinion, does this mean that WebAssembly implementations should not be tested? 

5.  Shortly after the appearance of WebAssembly another paper proposed a mechanized specification of the language using Isabelle. The paper can be consulted here: https://www.cl.cam.ac.uk/~caw77/papers/mechanising-and-verifying-the-webassembly-specification.pdf. This mechanized specification complements the first formalization attempt from the paper. According to the author of this second paper, what are the main advantages of the mechanized specification? Did it help improving the original formal specification of the language? What other artifacts were derived from this mechanized specification? How did the author verify the specification? Does this new specification removes the need for testing?

## Answers

### 1) Knight Capital Bug Overview
The bug in Knight Capital's trading system was caused by a misconfiguration in their Smart Market Access Routing System (SMARS). The old "Power Peg" algorithm, which had been decommissioned, was inadvertently reactivated on some servers due to a misused configuration flag (RLP). This led to the system sending thousands of erroneous "child orders" for each parent order. These trades were executed at incorrect prices, buying high and selling low at an incredibly fast rate, affecting stock prices and causing significant market disruptions.

The algorithm in question was supposed to handle routing orders efficiently, but because the old "Power Peg" code was unintentionally enabled, it did not function properly. Instead of optimizing trades, it flooded the market with incorrect orders.

#### Repercussions
- Knight Capital incurred a **$440 million loss** within 45 minutes due to erroneous trades.
- Knight’s faulty trades affected **154 stocks**, comprising 20%-50% of trading volume in some cases, and disrupted stock prices significantly, by up to 10%.
- The company needed external financial support to avoid collapse, eventually being bought by Getco LLC.

#### Could Testing Have Helped?
Yes, thorough testing could have uncovered the bug. Particularly, if Knight Capital had tested the impact of turning on the RLP flag with simulated trades and ensured old, deprecated code was not left on active servers, the error could have been avoided. The Knight Capital bug occurred on **August 1, 2012**, when an outdated algorithm called "Power Peg" was mistakenly reactivated due to a misconfiguration flag in their SMARS. This caused Knight's trading system to execute erroneous trades at incorrect prices—buying high and selling low—affecting 154 stocks and leading to a **$440 million loss** in just 45 minutes. The bug, though local to Knight's systems, had a global impact on stock markets. Thorough testing, especially of deployment flags and server configurations, could have prevented this catastrophic failure .      

---

### 2) Private Constructor Changes in Apache Commons
The issue involves making constructors private in utility classes like `IterableUtils` and `ArrayUtils` to prevent instantiation. The change to `IterableUtils`'s constructor was controversial as it broke binary compatibility by changing a public constructor to private. To maintain backward compatibility, the change was revised, and Javadocs were improved. The change in `ArrayUtils` did not affect binary compatibility as it was a package-private class. Additional tests were not explicitly mentioned, but maintaining compatibility suggests that care was taken to ensure stability. The bug was local to the implementation. [More information](https://issues.apache.org/jira/projects/COLLECTIONS/issues/COLLECTIONS-772?filter=doneissues).   

---   
### 3) The concrete experiments they perform:
 -**Chaos Monkey**:Random termination of virtual machine instances hosting production services to ensure services can withstand instance failures.This experiment needs requirements which are running during production and normal working hours for rapid engineer response, and the services need to be designed to handle individual instance failures.
 
 -**Chaos Kong**:Simulates the failure of an entire Amazon EC2 region to verify the system’s ability to handle large-scale failures, and to do this Netflix must be capable of rerouting traffic from an EC2 region to another one in real-time,and the system should support failover between regions without disrupting the streaming experience.

 -**Failure Injection Testing**:Simulates request failures between Netflix services to ensure the system degrades gracefully without affecting the user experience.The requirements for this experiment are that the system should include fallback mechanisms to ensure that essential services continue functioning, even when non-essential ones fail,the test environment must be capable of selectively failing requests and monitoring the system’s response in real-time.

The variables observed are Stream Starts Per Second (SPS), which measures the number of video streams initiated each second,request latency,and error rates.   

The main results shows that Netflix has successfully built more resilient systems by simulating failures, leading to improved fault tolerance. The experiments allowed Netflix to identify system weaknesses, improving their ability to maintain availability and performance even under turbulent conditions.
Other companies like Amazon, Google, Microsoft, and Facebook were applying similar techniques to test  the resilience of their own systems.They could adopt Chaos Engineering based on their systems' architecture,they could terminate key service instances, inject artificial latency, fail requests between critical services,the Variables to Observe could be ad impressions per second (for ad-serving platforms), or error rates and response times.




---

### 4) Formal Specification for WebAssembly
A formal specification for WebAssembly ensures accuracy, consistency, and platform security by offering a precise and rigorous explanation of the language's functionality. Strong guarantees for correctness and compatibility are provided by this formal foundation, although testing is still necessary. 

#### Advantages of a Formal Specification for WebAssembly
- **Precision**: Ensures clear and unambiguous definitions.
- **Consistency**: Guarantees uniform behavior across platforms.
- **Security**: Facilitates rigorous safety checks.
- **Interoperability**: Enables smooth integration across environments.

#### Testing Necessity
The formal specification improves reliability, but testing remains crucial. Tests validate performance, check real-world behavior, and catch practical bugs.      

---

### 5) Mechanized Specification of WebAssembly
The mechanized specification of WebAssembly, detailed in the second paper by Conrad Watt, provides numerous advantages and complements the original formal specification. Key advantages of the mechanized specification include:

#### Advantages
- **Formal Proofs of Soundness**: The executable and validation procedures for WebAssembly can be confirmed by formal, machine-verified proofs made possible by the mechanized specification. This indicates that the specification is verifiable mathematically, ensuring that legitimate WebAssembly code won't result in dangerous behavior during execution (Researchr) (POPL 2018).      

- **Exposure of Issues in the Formal Specification**: The mechanization process helped identify several issues in the original WebAssembly specification, which were subsequently corrected. This feedback loop improved the official specification, showing how the mechanized version contributed to refining WebAssembly's design.

- **Creation of Artifacts**: A number of important artifacts were derived from the mechanized specification, including:
  - **Verified Executable Interpreter**: A WebAssembly interpreter that has been officially verified to guarantee accuracy.
  - **Type Checker**: A program to ensure WebAssembly code is type safe.
  - **Validation Algorithms**: Effective approaches for confirming that WebAssembly code can be validated in linear time.

- **Implementation Consistency**: The mechanized specification serves as a guideline for creating consistent implementations across browsers and environments, ensuring that WebAssembly behaves the same way regardless of the platform.    

---

### Verification of the Specification
The authors verified the specification using mechanized proofs within Isabelle/HOL, which mathematically confirmed that WebAssembly's formal semantics are sound. They also implemented WebAssembly in major browsers, providing practical validation of their formal semantics. These real-world implementations, combined with differential fuzzing, helped ensure that WebAssembly's specification holds up in actual usage scenarios.    

---

### Testing Necessity
Despite the mechanized formal specification, testing remains essential. While formal proofs verify correctness in theory, they do not account for all real-world edge cases or performance aspects. Testing is needed to ensure implementation consistency across platforms, optimize performance, and validate how WebAssembly integrates with other web technologies. Therefore, even with a rigorous specification, both formal verification and practical testing are necessary to ensure WebAssembly's robustness.
