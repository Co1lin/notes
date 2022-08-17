# Fuzzing

Introduction:

- [The Fuzzing Book](https://www.fuzzingbook.org/html/Fuzzer.html)

- [Fuzzing PDF Slides from Columbia University](https://www.cs.columbia.edu/~suman/security_2/fuzzing.pdf)

Learning with Coding:

- [libFuzzer Tutorial](https://github.com/google/fuzzing/blob/master/tutorial/libFuzzerTutorial.md)
- [Tensorflow v2.0 compilation from source code and fuzzing (part2)](https://dongshuaike.github.io/tensorflow/2020/06/26/Tensorflow_v2.0_compilation_and_fuzzing_part_2.html)



Blackbox fuzzing

- feed <u>random</u> inputs to programs
- see whether it exhibits incorrect behavior
- Easy but inefficient

Fuzzing

- generate inputs based on file or network

Problem detection

- Own dynamic checker: valgrind skins ???

Regression testing

- To confirm that a recent program or code change has not adversely affected existing features
- Re-run some test cases to ensure existing functionalities work fine



Mutation-based fuzzing

Generation-based fuzzing

Coverage-guided gray-box fuzzing

- Special type of mutation-based fuzzing

- Run mutated inputs on **instrumented** (插桩) program and <u>measure code coverage</u>

- Search for mutants that result in coverage increase

    1) try random mutations on test corpus
    2) only add mutants to the corpus <u>if coverage increases</u>

- Examples: American Fuzzy Lop (cut off branches) (AFL), libfuzzer

    ![image-20220805153846935](fuzzing.assets/image-20220805153846935.png)

Data-flow-guided fuzzing

- Intercept the data flow, analyze the inputs of <u>comparisons</u> ???
- Modify the test inputs, observe the effect on comparisons
- [datAFLow: Towards a Data-Flow-Guided Fuzzer](https://www.ndss-symposium.org/wp-content/uploads/fuzzing2022_23001_paper.pdf)

Differential testing/fuzzing

- Provide the same input to a series of similar applications,  and observe differences in their execution
- 

How much fuzzing is enough?

- Code coverage: how much code has been executed
    - Line coverage
    - Branch coverage
    - Path coverage
    - NOTE: do not guarantee finding the bug (e.g. input sensitive)
- 



Common Vulnerabilities and Exposures (CVE)

- **flaws in information security systems** that could be used to **harm** an organization or personal computer systems

