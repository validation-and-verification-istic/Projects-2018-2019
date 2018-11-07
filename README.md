# Projects 2018-2019

## Assignments

Each group of students should choose one of the assignments listed below. All assignments involve a form of **test execution**, 
**dynamic analysis**, **program inspection** and **code instrumentation**.

Partial controls will take place during sessions **3**, **6** and **8**. Deadline to deliver the project will be 
on **February 1st 23:59 PM CET**. See [deadline section] for more details.

The goal of each assignment is to develop a tool that takes as input the path to an existing Maven project. It can be assumed that the project provided as input uses JUnit 4 to implement its test suite.

[Pairs and choices](assignments.md)

### Code coverage

The goal of this assignment is to create a tool capable of computing several coverage statistics from a Maven project.
The tool should be able to perform at least one of the following analysis:

* Count the number of times each line in the source code of the project is executed by the test suite.
* Determine, for each branch in the code, if all parts has been executed. (Branch coverage).
    For example:
    ```Java
    if(a) {
        m();
    }
    else {
        n()
    }
    ```
    if `a` is `false` then the branch that invokes `n` is executed but not the other one.
* Obtain the sequence of method calls performed while running the test suite.
* Record all the values of constant types used as parameter for each method.

### Mutation analysis

[Mutation analysis or mutation testing](https://en.wikipedia.org/wiki/Mutation_testing) is a technique that evaluates the quality of a test suite. A test suite is considered good if it is able to detect bugs. The idea behind mutation testing is to create modified versions of the original program, or **mutants** by inserting artificial bugs and then check if the test suite is able to detect those changes. A **mutation** could be simple, for example, replacing **-** by **+** in the context of an arithmetic expression. Other examples could be to replace the boolean expression in a conditional instruction by `false` or replacing a method call by some value. Each type of transformation is usually called a **mutation operator**. A mutation operator can create one or more mutants given the same code.
The goal of this assignment is to create a tool that performs mutation testing on a given Maven project. The tool should implement at least 3 of the following mutation operators:

* Replace the boolean expression of a conditional instruction by `false` and, in a second moment by `true`.
* Remove all instructions in the body of a `void` method.
* Replace the body of a boolean method by a single `return true` (or `return false`) instruction.
* Perform the following operator substitution in the context of arithmetic expressions:

| Original operator | Replaced by |
|-------------------|-------------|
|        +          |      -      |
|        -          |      +      |
|        *          |      /      |
|        /          |      *      |

* Remove an arbitrary part of a boolean expression. For example: `a && !b` could become `a` or `!b`
* Replace one method call by a predefined value.
```java
int i = fou();
int j = fou();
```
gets transformed into 
```java
int i = 5;
int j = fou();
```

* In the presence of an integer constant `x`, replace it by `x+1`, `x-1`, `2*x` and `x/2`.  
* Replace a comparison operator by another given the following possible substitutions:

| Original operator | Replaced by |
|-------------------|-------------|
|        <          |     <=      |
|        >          |     >=      |
|        <=         |      <      |
|        >=         |      >      |

* Replace the right part of an assignment by a predefined value.

The tool should provide a way to configure the mutation operators to use in the analysis.

### Assertion generation

The goal of this assignment is to create a tool that proposes new assertions to strengthen existing test cases. The tool should take as input the path of an existing Maven project, a target test method and the number of assertions to add. The output should be the code of the test case with the new assertions included.
To generate the assertions the tool should execute the test case and observe the value of public field and public getter methods. Then, the assertions can verify is these members produce the same values.
For example, given a class and a test like the following:
```Java
class Person {
    private String name;
    private int age;

    public Person(String name, int age) {
        this.name = name;
        this.age = age;
    }

    public String getName() {
        return name;
    }

    public int getAge() {
        return age;
    }

    public boolean isAdult() {
        return age >= 18;
    }
}

class PersonTest {

    @Test
    public void testAge() {
        Person p = new Person("Someone", 13);
        assertFalse(p.isAdult());
    }
}
```
If the tool is instructed to provide 2 assertions for `PersonTest.testAge` then it could produce the following output:

```Java
@Test
public void testAge() {
    Person p = new Person("Someone", 13);
    assertFalse(p.isAdult());
    assertEquals(p.getName(), "Someone");
    assertEquals(p.getAge(), 13);
}
```

At least, the implemented tool should create `assertEquals` assertions using primitive typed method and fields. If the tool can generated more assertions than the number given as input, then it should randomly select the assertions to include.

As an additional goal, the tool could insert timeout assertions by observing several executions of the given test case.


## Requirements

Each team is expected to create a git repository that can be accessed, for example, from Github or Gitlab. This repository must contain:
* A Maven project containing the **code for the assignment** and a **strong jUnit test suite**. The strength of the test suite will be evaluated using coverage and mutation testing tools.
* A `README.md` file specifying how to build the code, how to use the compiled binaries and a list of projects on which the team has successfully used the tool.
* A report describing the functionalities of the tool, what problem does it solve, the main challenges faced during the development and how the tool has been validated. Guidelines concerning this report can be found [here](http://people.rennes.inria.fr/Benoit.Baudry/wp-content/uploads/2013/09/Instructions_for_report.pdf).

The repository should reflect the development history of the assignment. All members of the team should contribute to this repository.

The minimal requirements described above will only grant the team an average grade. The teams are expected to achieve additional requirements such as:
* Set up a continuous integration system to test the code of the assignment. For example [Jenkins](https://jenkins.io/) or [Travis](https://travis-ci.org/)
* Monitor the coverage of the test suite, using tools like [Coveralls](https://coveralls.io/).
* Visualize the results.
* Create a graphical user interface for the tool.
* Develop the tool as a Maven plugin.

Any additional effort will be taken into account for the grade.

## Supporting libraries

The use of the following libraries is encouraged to analyze and instrument the code of a given program:
* [Spoon](http://spoon.gforge.inria.fr/): A library for java source code analysis.
* [ASM](https://asm.ow2.io/): A framework for Java bytecode manipulation and analysis.
* [Javassist](http://www.javassist.org/): A library for Java bytecode manipulation.


## Possible targets

Using real projects as target can be troublesome, so we recommend you to start with the following ones. (We encourage you to go beyond this list)

 * [commons-cli](https://github.com/apache/commons-cli)
 * [commons-codec](https://github.com/apache/commons-codec)
 * [commons-collection](https://github.com/apache/commons-collections)
 * [commons-lang](https://github.com/apache/commons-lang)
 * [commons-math](https://github.com/apache/commons-math)


# Deadlines

At the end of **session 1** (19/10/2018) you should have decided on the assignment and set up the git repository containing a skeleton Maven project.

After **session 3** (16/11/2018) the tool should be able to run the test suite (or selected test classes) of a Maven project given as input.

After **session 6** (07/12/2018) the tool should be able to instrument part of the code needed to achieve its goal.

After **session 8** (25/01/2019) most functionalities should be already implemented.

Final deadline will be  **February 1st 23:59 PM CET CET**. 
At that moment all repositories will be cloned and this will be considered as the final version of the project.

