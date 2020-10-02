# What does Fortran need in 2020 to survive?
#### Intro
I asked this question for myself at some late evening on Sunday during multiple half-successful attempts of converting and installing my collection of useful Fortran code snippets into the visual studio code editor. I have written a small converter code in D language for this purpose. Usually, I take Python for similar problems, but I took D for this task just to test the usability of VScode / JetBrains plugin for the D language. Both work, even though still with many small quirks. I decided to learn one programming language per year and last year the choice has fallen on D, so that was my motivation to prove the fact that I still have the required knowledge. D is a good example of another sad story in the programming language world. Despite multiple attempts to gain the recognition and rather a good ecosystem, the popularity of this language is in extended stagnation. The most important reason for this situation might be a strong concurrence with C++. It became especially elucidated after the introduction of the C++20 standard. But I want to talk about Fortran today. To start with the theme, unfortunately, I am no more the Fortran developer for the last 5 years. But I had been an active Fortran developer since 2001 and until 2015. Starting with FORTRAN77 I have been using FORTRAN90, Fortran 2003, and beyond Fortran 2008 with a long set of compilers (g77, g95, gfortran, ifort, pgfortran, nagfor etc).

My tasks for Fortran were always very plain and specific: large scale computations for theoretical atomic physics. These are fields where Fortran is still in good health: matrix operations, solutions of ODE/PDEs, parallelized math operations with MPI, OpenMP, or using PGI (now NVIDIA) compiler for GPU CUDA accelerated computing.

I produced my codes in Fortran90-2008 routinely without making any efforts until the year 2015 when failed to make a usable program. Bad signs were already visible at the late beginning, during the prototyping phase, but I was a real stubborn donkey and didn&#39;t want to accept this moment of truth. I do not want to come into detail, but the task was to develop a complex multiscale physical model. And for that kind of problem, I needed advanced data structures (tree, hashmap, linked lists), and containers of combined complex data structures. Also, a few design patterns were needed (prototype, fabric, state, strategy) too. Nothing of that existed in Fortran. There is no book describing even a good old red-black tree in Fortran. There is still no book in Fortran for OOP or with examples for design patterns in Fortran. I spent almost 2 weeks on developing&amp;testing a simple hashmap in Fortran and another 2 or 3 for the basic red-black tree. Then I implemented 3 of 5 needed design patterns and started with prototyping of the program. I spent more than 3 months in total just for developing different wheels in Fortran. In the end, a Ph.D. student finished the prototype of program with the same functionality in a week using D language. That was the strongest fall of my vision and trust in Fortran and I have got a complete enlightenment. I recognized that the language is very limited and is fallen in this use case as in the Fall of Carthage. No future for Fortran here. Period. The 2018-standard just confirmed my belief. No changes came in this swamp, just stagnation...

### Problems
So here is my short and the incomplete list of Fortran problems:

##### Problems of the Fortran language itself

* Missing standard library. There is no choice. Only very basic libraries of questionable quality at Github almost without any kind of internal unit testing. Continuous developing of wheels needed for advanced data structures.

 * Missing built-in unit testing. There are some external modules for that purpose but if we still are positioning Fortran as a formula translator, we need unit testing directly built-in in the language.

* Missing exception handling. Program in Fortran cannot survive any kind of serious exception. For critical software this is an unacceptable freedom.

* Missing unsigned integers / bit strings for cryptography and networking. This fact significantly complicates writing of interfaces (another sad story) to existing C-libraries.

* Very limited pointer functionality. Any application of function pointers or simple pointers in the container is so wordy that makes code unreadable.

* Namespaces are limited to modules and submodules and are not in a good interplay with available limited OOP concepts of object method/field visibility.

* Too restricted overloading in OOP.

* Missing handling of built-in basic types as first citizen objects. You cannot do anything with an integer, real, logical, or complex types.

* No templates. Meta-programming/ generic programming cannot be done without external preprocessor and using it (python or c preprocessor) demolishes your code to completely unreadable state.

* OOP is so wordy and full of &quot;select type&quot; constructs that none wants to use it again after any first serious usage.

* Functional programming is rather at a basic level. Yes we have elemental, pure, recursion, and higher-order functions, one level of nested functions, but without closures, lambda-functions, and partial application.

* Interoperability of Fortran with C is another suitcase without a handle. It is again so wordy and clumsy (combined with already mentioned missing unsigned integer and handling of 0-terminated-strings), that since introducing it in 2003-2008 only very few interfaces in Fortran have been created for popular C-ibraries.

##### Problems of compilers

* In the best scenario, all compilers implement a new Fortran standard about one decade later. This means that we will see the Fortran 2018 compiler possibly around 2030. To that time this standard will contain completely outdated ideas and paradigms of computer science. To the date, only 3 compilers (gfortran and ifort and nagfor) have &quot;almost&quot; implemented the Fortran 2008 standard. All others are stopped around 2003-standard.

* All Fortran compilers are generally ABI-incompatible. Moreover, each compiler has also its standard of pre-compiled modules. Additionally, there are even different versions of compiled modules within one compiler. This reduces significantly the packaging options and distribution of precompiled libraries in Fortran.

* Open-source compiler gfortran as part of GCC doesn&#39;t have more than a dozen maintainers and developers. More than 1000 open bugs exist currently in GCC Bugzilla related to the Fortran part. Intel with ifort might look indeed better, but the state of the language without a reference compiler is too dangerous and usually the language cannot survive in our modern computer science world. Almost 2 decades acted gfortran that part unofficially. Current situation with developers means that we almost lost the gfortran compiler because the development has stopped there.

* No build system and no packaging system. And don&#39;t tell me about makefiles or cmake. Try f.e. dub or maven instead.

##### Ecosystem and community problems

* 99% of developers (engineers, scientists, compiler developers) are from the old school, they are preferring self-hosted forums, Usenet, mailing lists or IRC. They are not active in social media and are not able to bring new users and developers, and cannot promote the language. Only very few developers (and m.b. some students) are using git social platforms (Github/Bitbucket/Gitlab) and are trying to change the situation.

* Available source code base (often promoted as a &quot;feature&quot; by many Fortran apologists) comes to 90% from late 60-70-80, is written (in the best case) in some mix of f77-f90 standards with a bunch of partially supported extensions without internal testing, is highly unreadable, with implicit typing, is not thread-safe, and usually cannot be parallelized without significant refactoring. Only very few enthusiasts are currently trying to translate useful packages and modernize popular math/numerical libraries. There are no open automatic translators/refactoring tools. The superior commercial math libraries are too pricy and cannot be used in small/middle-sized projects - i.e. these libraries are taboo for all Fortran enthusiasts.

* Fortran Standards Committee is a representative distribution of such &quot;old school&quot; and of developers without modern vision/critical voice power and is not able to bring more than 1% of needed changes to the language at once per standard.

* Young potential developers see very limited (and too wordy in OOP paradigm) language, without reference compiler, without the standard library, without any ecosystem (no web site, no forum, no or very limited support in popular IDEs, no CASE tools, no build/packaging system, no good GUI library, no networking, no cryptography), with some dozen books where authors are mostly rephrasing standard and have no single idea about modern software design, development, testing, and maintenance.
 
### Summary 

And finally, just few selected facts. Rosetta code site shows in about 50% of tasks only old-fashioned F77 solutions in UPPERCASE from one &quot;old-school&quot; developer. Github shows broken source code highlighting of Fortran for ages due to the &quot;very smart&quot; idea of having &quot;not reserved&quot; keywords in Fortran language that pushed the parsing/highlighting problem of Fortran code from extreme simplicity to the space level. So none in the world wants to invest time to that &quot;space trip&quot; now.

In summary, the words of Prof. Dijkstra are unfortunately still/again actual: _The Fortran language is still &quot;the infantile disorder--, by now nearly 65 (corr., originally 20) years old, is hopelessly inadequate for whatever computer application you have in mind today: it is now too clumsy, too risky, and too expensive to use&quot;._

Sorry guys, but that the hard truth someone need to repeat now loud and clear. To survive (but without any guarantee for prosperity) we need to address all these questions, and we should fix/repair everything listed here very quickly. Otherwise, there is no &quot;future&quot; for Fortran.
