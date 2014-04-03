---
layout: post
title: "Software Testing"
date: 2014-04-01 16:24:56 +0800
comments: true
categories: software
keywords: software, test, testing
description: software testing
---
###1. Overview  
Software testing provide an objective, independent view to allow the business to appreciate and understand the risks of software, product or service implementation. Test techniques include, but are not limited to the process of executing a program or application with the intent of finding software bugs (errors or other defects).<!--more-->  
  
Software testing can be stated as the process of validating and verifying that a computer program/application/product:  
* meets the requirements that guided its design and development,  
* works as expected,  
* can be implemented with the same characteristics,
and satisfies the needs of stakeholders.  
  
Software testing, depending on the testing method employed, can be implemented at any time in the software development process.  
  
Testing can never completely identify all the defects within software. Instead, it furnishes a criticism or comparison that compares the state and behavior of the product against oracles—principles or mechanisms by which someone might recognize a problem. These oracles may include (but are not limited to) specifications, contracts, comparable products, past versions of the same product, inferences about intended or expected purpose, user or customer expectations, relevant standards, applicable laws, or other criteria.  
  
A primary purpose of testing is to detect software failures so that defects may be discovered and corrected. Testing cannot establish that a product functions properly under all conditions but can only establish that it does not function properly under specific conditions. The scope of software testing often includes examination of code as well as execution of that code in various environments and conditions as well as examining the aspects of code: does it do what it is supposed to do and do what it needs to do. A testing organization may be separate from the development team. There are various roles for testing team members. Information derived from software testing may be used to correct the process by which software is developed.  
  
Software testing is the process of attempting to make the assessment that whether the software product will be acceptable to its end users, its target audience, its purchasers and other stakeholders..  
  
####A Defects and failures  
Not all software defects are caused by coding errors. One common source of expensive defects is requirement gaps, e.g., unrecognized requirements which result in errors of omission by the program designer. Requirement gaps can often be non-functional requirements such as testability, scalability, maintainability, usability, performance, and security.  
  
####B Input combinations and preconditions  
A fundamental problem with software testing is that testing under all combinations of inputs and preconditions (initial state) is not feasible. More significantly, non-functional dimensions of quality (how it is supposed to be versus what it is supposed to do)—usability, scalability, performance, compatibility, reliability—can be highly subjective; something that constitutes sufficient value to one person may be intolerable to another.  
  
Software developers can't test everything, but they can use combinatorial test design to identify the minimum number of tests needed to get the coverage they want.   
  
###2. Testing methods  
####A Static vs. dynamic testing  
Reviews, walkthroughs, or inspections are referred to as static testing, whereas actually executing programmed code with a given set of test cases is referred to as dynamic testing.  
  
Static testing involves verification, whereas dynamic testing involves validation.  
  
####B The box approach  
*1) White-box testing*  
White-box testing (also known as clear box testing, glass box testing, transparent box testing and structural testing) tests internal structures or workings of a program, as opposed to the functionality exposed to the end-user. In white-box testing an internal perspective of the system, as well as programming skills, are used to design test cases. The tester chooses inputs to exercise paths through the code and determine the appropriate outputs. This is analogous to testing nodes in a circuit.  
  
While white-box testing can be applied at the unit, integration and system levels of the software testing process, it is usually done at the unit level. It can test paths within a unit, paths between units during integration, and between subsystems during a system–level test. Though this method of test design can uncover many errors or problems, it might not detect unimplemented parts of the specification or missing requirements.  
  
Techniques used in white-box testing include:  
* API testing (application programming interface) – testing of the application using public and private APIs  
* Code coverage – creating tests to satisfy some criteria of code coverage (e.g., the test designer can create tests to cause all statements in the program to be executed at least once)  
* Fault injection methods – intentionally introducing faults to gauge the efficacy of testing strategies  
* Mutation testing methods  
* Static testing methods  
  
Code coverage tools can evaluate the completeness of a test suite that was created with any method, including black-box testing. This allows the software team to examine parts of a system that are rarely tested and ensures that the most important function points have been tested. Code coverage as a software metric can be reported as a percentage for:  
* Function coverage, which reports on functions executed  
* Statement coverage, which reports on the number of lines executed to complete the test  
  
100% statement coverage ensures that all code paths, or branches (in terms of control flow) are executed at least once. This is helpful in ensuring correct functionality, but not sufficient since the same code may process different inputs correctly or incorrectly.  
  
*2) Black-box testing*  
Black-box testing treats the software as a "black box", examining functionality without any knowledge of internal implementation. The testers are only aware of what the software is supposed to do, not how it does it. Black-box testing methods include: equivalence partitioning, boundary value analysis, all-pairs testing, state transition tables, decision table testing, fuzz testing, model-based testing, use case testing, exploratory testing and specification-based testing.  
  
One advantage of the black box technique is that no programming knowledge is required. Whatever biases the programmers may have had, the tester likely has a different set and may emphasize different areas of functionality.
  
This method of test can be applied to all levels of software testing: unit, integration, system and acceptance. It typically comprises most if not all testing at higher levels, but can also dominate unit testing as well.  
  
*3) Grey-box testing*  
Grey-box testing involves having knowledge of internal data structures and algorithms for purposes of designing tests, while executing those tests at the user, or black-box level. The tester is not required to have full access to the software's source code. Manipulating input data and formatting output do not qualify as grey-box, because the input and output are clearly outside of the "black box" that we are calling the system under test. This distinction is particularly important when conducting integration testing between two modules of code written by two different developers, where only the interfaces are exposed for test.  
  
Typically, a grey-box tester will be permitted to set up an isolated testing environment with activities such as seeding a database. The tester can observe the state of the product being tested after performing certain actions such as executing SQL statements against the database and then executing queries to ensure that the expected changes have been reflected. Grey-box testing implements intelligent test scenarios, based on limited information. This will particularly apply to data type handling, exception handling, and so on.  
  
###3. Testing levels  
####A Unit testing  
Unit testing, also known as component testing, refers to tests that verify the functionality of a specific section of code, usually at the function level. In an object-oriented environment, this is usually at the class level, and the minimal unit tests include the constructors and destructors.  
  
Unit testing aims to eliminate construction errors before code is promoted to QA; this strategy is intended to increase the quality of the resulting software as well as the efficiency of the overall development and QA process.  
  
Depending on the organization's expectations for software development, unit testing might include static code analysis, data flow analysis metrics analysis, peer code reviews, code coverage analysis and other software verification practices.  
  
####B Integration testing  
Integration testing is any type of software testing that seeks to verify the interfaces between components against a software design. Software components may be integrated in an iterative way or all together.  
  
Integration testing works to expose defects in the interfaces and interaction between integrated components (modules). Progressively larger groups of tested software components corresponding to elements of the architectural design are integrated and tested until the software works as a system.  
  
####C Component interface testing  
The practice of component interface testing can be used to check the handling of data passed between various units, or subsystem components, beyond full integration testing between those units. The data being passed can be considered as "message packets" and the range or data types can be checked, for data generated from one unit, and tested for validity before being passed into another unit. Tests can include checking the handling of some extreme data values while other interface variables are passed as normal values. Unusual data values in an interface can help explain unexpected performance in the next unit. Component interface testing is a variation of black-box testing, with the focus on the data values beyond just the related actions of a subsystem component.  
  
####D System testing  
System testing, or end-to-end testing, tests a completely integrated system to verify that it meets its requirements. For example, a system test might involve testing a logon interface, then creating and editing an entry, plus sending or printing results, followed by summary processing or deletion (or archiving) of entries, then logoff.  
  
In addition, the software testing should ensure that the program, as well as working as expected, does not also destroy or partially corrupt its operating environment or cause other processes within that environment to become inoperative (this includes not corrupting shared memory, not consuming or locking up excessive resources and leaving any parallel processes unharmed by its presence).  
  
####E Acceptance testing  
At last the system is delivered to the user for Acceptance testing.  
  
###4. Testing types  
####A Installation testing  
An installation test assures that the system is installed correctly and working at actual customer's hardware.  
  
####B Compatibility testing  
A common cause of software failure (real or perceived) is a lack of its compatibility with other application software, operating systems (or operating system versions), or target environments that differ greatly from the original (such as a terminal or GUI application intended to be run on the desktop now being required to become a web application, which must render in a web browser).  This results in the unintended consequence that the latest work may not function on earlier versions of the target environment, or on older hardware that earlier versions of the target environment was capable of using.  
  
####C Smoke and sanity testing  
Sanity testing determines whether it is reasonable to proceed with further testing. Smoke testing consists of minimal attempts to operate the software, designed to determine whether there are any basic problems that will prevent it from working at all. Such tests can be used as build verification test.  
  
####D Regression testing  
Regression testing focuses on finding defects after a major code change has occurred. Specifically, it seeks to uncover software regressions, as degraded or lost features, including old bugs that have come back. Such regressions occur whenever software functionality that was previously working, correctly, stops working as intended. Typically, regressions occur as an unintended consequence of program changes, when the newly developed part of the software collides with the previously existing code.  
  
####E Acceptance testing  
  
####F Alpha testing  
Alpha testing is simulated or actual operational testing by potential users/customers or an independent test team at the developers' site. Alpha testing is often employed for off-the-shelf software as a form of internal acceptance testing, before the software goes to beta testing.  
  
####G Beta testing  
Beta testing comes after alpha testing and can be considered a form of external user acceptance testing. Beta versions, of software are released to a limited audience outside of the programming team.   
  
####H Functional vs non-functional testing  
Functional testing refers to activities that verify a specific action or function of the code. These are usually found in the code requirements documentation, although some development methodologies work from use cases or user stories. 
Non-functional testing refers to aspects of the software that may not be related to a specific function or user action, such as scalability or other performance, behavior under certain constraints, or security. Testing will determine the breaking point, the point at which extremes of scalability or performance leads to unstable execution. Non-functional requirements tend to be those that reflect the quality of the product, particularly in the context of the suitability perspective of its users.  
  
####I Destructive testing  
Destructive testing attempts to cause the software or a sub-system to fail. It verifies that the software functions properly even when it receives invalid or unexpected inputs, thereby establishing the robustness of input validation and error-management routines.  
  
####J Software performance testing####  
Performance testing is generally executed to determine how a system or sub-system performs in terms of responsiveness and stability under a particular workload. It can also serve to investigate, measure, validate or verify other quality attributes of the system, such as scalability, reliability and resource usage.  
  
Load testing is primarily concerned with testing that the system can continue to operate under a specific load, whether that be large quantities of data or a large number of users. This is generally referred to as software scalability. The related load testing activity of when performed as a non-functional activity is often referred to as endurance testing. Volume testing is a way to test software functions even when certain components (for example a file or database) increase radically in size. Stress testing is a way to test reliability under unexpected or rare workloads. Stability testing (often referred to as load or endurance testing) checks to see if the software can continuously function well in or above an acceptable period.  
  
The terms load testing, performance testing, scalability testing, and volume testing, are often used interchangeably. Real-time software systems have strict timing constraints.  
  
####K Usability testing  
Usability testing is needed to check if the user interface is easy to use and understand. It is concerned mainly with the use of the application.  
  
####L Accessibility testing  
  
####M Security testing  
  
####N Other testings  
  
###5. Testing process  
####A Traditional waterfall development model  
A common practice of software testing is that testing is performed by an independent group of testers after the functionality is developed, before it is shipped to the customer. This practice often results in the testing phase being used as a project buffer to compensate for project delays, thereby compromising the time devoted to testing.   
  
Another practice is to start software testing at the same moment the project starts and it is a continuous process until the project finishes.  
  
####B Agile or Extreme development model  
In contrast, some emerging software disciplines such as extreme programming and the agile software development movement, adhere to a "test-driven software development" model. In this process, unit tests are written first, by the software engineers (often with pair programming in the extreme programming methodology). Of course these tests fail initially; as they are expected to. Then as code is written it passes incrementally larger portions of the test suites. The test suites are continuously updated as new failure conditions and corner cases are discovered, and they are integrated with any regression tests that are developed. Unit tests are maintained along with the rest of the software source code and generally integrated into the build process (with inherently interactive tests being relegated to a partially manual build acceptance process). The ultimate goal of this test process is to achieve continuous integration where software updates can be published to the public frequently.  
  
This methodology increases the testing effort done by development, before reaching any formal testing team. In some other development models, most of the test execution occurs after the requirements have been defined and the coding process has been completed.  
  
####C Top-down and bottom-up  
Bottom Up Testing is an approach to integrated testing where the lowest level components (modules, procedures, and functions) are tested first, then integrated and used to facilitate the testing of higher level components. After the integration testing of lower level integrated modules, the next level of modules will be formed and can be used for integration testing. The process is repeated until the components at the top of the hierarchy are tested. This approach is helpful only when all or most of the modules of the same development level are ready. This method also helps to determine the levels of software developed and makes it easier to report testing progress in the form of a percentage.  
  
Top Down Testing is an approach to integrated testing where the top integrated modules are tested and the branch of the module is tested step by step until the end of the related module.  
  
####D A sample testing cycle  
* Requirement analysis  
* Test planning  
* Test development  
* Test execution  
* Test reporting  
* Test result analysis  
* Defect retesting  
* Regression testing  
* Test closure  
  
###6. Automated testing  
####A Testing tools  
* Program monitors, permitting full or partial monitoring of program code including:  
  + Instruction set simulator, permitting complete instruction level monitoring and trace facilities  
  + Program animation, permitting step-by-step execution and conditional breakpoint at source level or in machine code  
  + Code coverage reports  
* Formatted dump or symbolic debugging, tools allowing inspection of program variables on error or at chosen points  
* Automated functional GUI testing tools are used to repeat system-level tests through the GUI  
* Benchmarks, allowing run-time performance comparisons to be made  
* Performance analysis (or profiling tools) that can help to highlight hot spots and resource usage  
  
####B Measurement in software testing  
Usually, quality is constrained to such topics as correctness, completeness, security, but can also include more technical requirements as described under the ISO standard ISO/IEC 9126, such as capability, reliability, efficiency, portability, maintainability, compatibility, and usability.  
  
There are a number of frequently used software metrics, or measures, which are used to assist in determining the state of the software or the adequacy of the testing.  
  
###7. Testing artifacts  
* Test plan  
* Traceability matrix  
* Test case  
* Test script  
* Test suite  
* Test fixture or test data  
* Test harness  
  
###8. Related process  
####A Software verification and validation  
* Verification: Have we built the software right? (i.e., does it implement the requirements).  
* Validation: Have we built the right software? (i.e., do the requirements satisfy the customer).  
  
According to the IEEE Standard Glossary of Software Engineering Terminology:  
* Verification is the process of evaluating a system or component to determine whether the products of a given development phase satisfy the conditions imposed at the start of that phase.  
* Validation is the process of evaluating a system or component during or at the end of the development process to determine whether it satisfies specified requirements.  
  
According to the ISO 9000 standard:  
* Verification is confirmation by examination and through provision of objective evidence that specified requirements have been fulfilled.
* Validation is confirmation by examination and through provision of objective evidence that the requirements for a specific intended use or application have been fulfilled.  
  
####B Software quality assurance  
Software testing is a part of the software quality assurance (SQA) process. In SQA, software process specialists and auditors are concerned for the software development process rather than just the artifacts such as documentation, code and systems. They examine and change the software engineering process itself to reduce the number of faults that end up in the delivered software: the so-called "defect rate". What constitutes an "acceptable defect rate" depends on the nature of the software; A flight simulator video game would have much higher defect tolerance than software for an actual airplane. Although there are close links with SQA, testing departments often exist independently, and there may be no SQA function in some companies.  
  
Software testing is a task intended to detect defects in software by contrasting a computer program's expected results with its actual results for a given set of inputs. By contrast, QA (quality assurance) is the implementation of policies and procedures intended to prevent defects from occurring in the first place.  
  
[Origin](http://en.wikipedia.org/wiki/Software_testing)