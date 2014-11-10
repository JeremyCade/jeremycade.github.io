---
layout: post
title:  "Software Architecture Evolution Over Time."
date:   2011-05-05
categories: software engineering architectural design patterns
---

Works by Parnas and Shaw set the stage for the development of architectural design patterns that enabled later computer scientists to develop both architectural and module level design patterns. This has empowered architects and developers with the ability to build highly cohesive, loosely coupled large-scale systems.

## 1. Introduction
The development of codified software design patterns over the last 30 years has been of significance to the software industry. With each new design pattern a new level of abstraction has been developed to simplify or iron out complexity in the systems in where those patterns have been implemented.

However in order to understand the overall architecture of a system we first need to be able to decompose the system into subsystems or modules, with each subsystem or module adhering to a design pattern in order to maintain some sort of maintainability and testability.

## 2.  Discussion
In his 1972 paper, "On the Criteria To Be Used in Decomposing Systems into Modules", Parans discusses the decomposition of software into modules, producing two separate modularisations.

The first is along the lines of functional/procedural responsibility or steps, referred to as the "flowchart" method [1]. The second is along the lines of "Information Hiding".

Parans says of the second modularisation: "Every module in the second decomposition is characterized by its knowledge of a design decision which it hides from all others. Its interface or definition was chosen to reveal as little as possible about its inner workings." [1]

These two sentences are of great interest, as this was the first time the idea of Information Hiding was introduced to the world of Software Architecture and Design.

This idea, sometimes referred to as the Black Box Principal, is of paramount importance to software design, as it is the founding basis for a number of Object Oriented design principals including Encapsulation and the practice of designing to Interfaces rather than concrete classes.

The Black Box Principal allows for a module or class to built in such a way that only its input and output are known, while hiding its internal workings, implementation, or design decisions from consuming applications, modules or classes.

This allows for the design and creation of loosely coupled software systems, which in turn increase testability and reduce the chance of failure due to cascading errors in the event of changes to the module or classes internal design or implementation.

This principal, is in fact an abstraction of the inner workings given module or class in order to implement it’s output in a larger system.

In her 1989 paper "Larger Scale Systems Require Higher-Level Abstractions" Shaw makes the argument for the need of codified high-level design patterns that can be used to make abstracted design decisions at the system and subsystem levels, in order to better understand and produce large-scale systems [2].

In section "3. Composition of Systems from Subsystems", Shaw makes the observation that large systems are constructed by combining subsystems, and that these subsystems have their own internal structures, which could be designed at the system level, rather than the module or class level [2].

This allows for the implementation of higher-level design patterns for specific subsystems that are best suited to their function or responsibilities

However Shaw makes the conclusion that the at the time of publication (1989) Software Architecture was not yet mature enough to develop or support such high-level patterns [2].

As we can see, both Parans and Shaw’s work address software design from a structural, decomposition standpoint, while this may have been the founding for a lot of today’s current structural architecture level design patterns, it fails to take into account the other areas software design that are important in todays object oriented world, specifically those of object creation, structure and behaviour in complex software systems.

Neither of the papers takes into account the inherent complexity of trying to integrate multiple modules to form complex systems, while maintaining portability, testability and high maintainability.

Thankfully, some of these issues have been addressed through the creation of modern architectural and module / class level design patterns.

## 3. Critical Analysis
Shaw’s paper is a spiritual continuation of Parnas’s earlier "Information Hiding" (Black Box) work, in that Shaw is in fact advocating for the abstraction of lower level modules functions behind a subsystem design pattern, effectively extending the Black Box Principal beyond that of a single module or class to an entire subsystem. This continuation is evident in section 2 of Shaw’s paper where the Software Architecture Level of Design is discussed. Shaw states: "This level of system organization and design is the software architecture level." [2]. Parnas, in his work, did not mention, or perhaps failed to anticipate that large-scale systems may require a number of levels of abstraction in order to effectively produce the system. Shaw’s work rectifies this by explicitly advocating the need for architecture level design.

Architecture level design, is of course a major factor that must be considered when developing systems for modern systems, the choice of architecture level design patterns is of paramount importance when developing large scale systems, as the choice of the wrong design pattern could have catastrophic consequences, for example the Model View Controller pattern, developed at XEROX PRACE in 1978-79 [3] may provide the foundation for a Graphical User Interface (GUI) based system such as a Word Processor, but would be ill suited to a Telephone Exchange System or a Control and Command System.

While Shaw addressed the higher-level concepts of System and subsystem architecture, she failed to address the issues related to lower level modules and classes, thankfully a large number of these issues have been addressed through the acceptance of the patterns and practices published throughout the early 90’s and early 2000’s. Books such as Code Complete [4] and the Design Patterns: Elements of Reusable Object-Oriented Software" [5] introduced a large number of developers and architects to reusable design patterns that allow for the creation and maintenance and testability of modules or classes in a decoupled, highly cohesive manner.

For example, Robert C. Martin in his book "Agile Software Development: Principals, Patterns and Practices" [5] introduces a set of principals known as SOLID. SOLID is an acronym for Single Responsibility, Open-Closed, Liskov Substitution, Interface Segregation and Dependency Inversion. These principals extend on Parnas’s information hiding principals, and Shaw's module abstraction to address the issues related to testability and maintainability in complex systems. For example, the Dependency Inversion principal states that high level modules or classes should not depend on low level modules or classes. This principal is implemented via the Dependency Injection pattern, which extremely similar to the Factory Method [4][5]. Dependency Injection allows for decoupling of modules, which in turn makes it easier to unit test modules.

Well-tested, decoupled modules often have higher levels of cohesion, allowing those modules to be more portable, readable and maintainable in the long term.

There is however still a number of open issues from a software architecture standpoint, as the underlying hardware increases in capacity and complexity the need to continually abstract the inner workings to increase design simplicity of each system is a cause for ambiguity, and misunderstanding. Shaw did mention this in her paper, however, it may well result in a time where those making the design decisions no longer understand the underlying rationale for the design pattern that they have chosen to implement. At a lower level this is evident in the uptake of memory managed programming environments, e.g. Java. It could be argued that Java developers are more ignorant of their memory footprint, compared to C developers. There is a real chance that this could happen at a higher level of design as we continually develop new ways to abstract away from the inner workings of our systems.

## 4. Conclusion
Parans work created the foundations for a large number of design patterns still implemented in today’s moderns software systems. Shaw extended Parans work in a way that allowed for the abstraction of design decisions to higher levels of a software system. This work has since been extended upon by a number of different people, allowing for reusable software design patterns that give us the ability to build highly cohesive, decoupled systems that are easily tested.

However we may face a time in the future where the continued abstraction of the design decisions leads to an ignorance of the base implementation of a system.

## 5. References
[1] David L. Parnas. "On the Criteria to be Used on Decomposing Systems into Modules," Communications of the ACM, 15(12):1053-1058, 1972.

[2] Mary Shaw. "Larger Scale Systems Require Higher-Level Abstractions," Proceedings of the Fifth International Workshop on Software Specifications and Design, published 1989

[3] Trygve Reenskaug. ∑"MVC XEROX PARC 1978-79″, [http://heim.ifi.uio.no/~trygver/themes/mvc/mvc-index.html](http://heim.ifi.uio.no/~trygver/themes/mvc/mvc-index.html)

[4] Steve McConnell. "Code Complete" Microsoft Press. 1994

[5] E. Gamma, R. Helm, R. Johnson & J. Vlissides. "Design Patterns: Elements of Reusable Object-Oriented Software", 1994

[6] Robert C. Martin. "Agile Software Development: Principles, Patterns, and Practices " 2002
