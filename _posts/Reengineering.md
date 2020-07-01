- [Re-engineering](#re-engineering)
  - [Why do systems need to change?](#why-do-systems-need-to-change)
  - [why not start from scratch](#why-not-start-from-scratch)
- [A Preliminary Assessment](#a-preliminary-assessment)
  - [SWOT Report](#swot-report)
  - [Understanding how the systems work](#understanding-how-the-systems-work)
  - [Bash](#bash)
    - [Jupyter](#jupyter)
      - [cygwin](#cygwin)
      - [kernel](#kernel)
  - [WEKA Java ML Framework](#weka-java-ml-framework)
- [Source Code](#source-code)
  - [Static analysis](#static-analysis)
  - [Analysis Tech](#analysis-tech)
    - [Source code analysis](#source-code-analysis)
    - [ByteCode Analysis](#bytecode-analysis)
    - [Reverse-Engineering a Class Diagram](#reverse-engineering-a-class-diagram)
      - [inheritance relations & association relations](#inheritance-relations--association-relations)
- [Dynamic analysis](#dynamic-analysis)
  - [Loggers](#loggers)
  - [Execution traces](#execution-traces)
    - [Instrumentation Agent in Java](#instrumentation-agent-in-java)
- [Clones code](#clones-code)
  - [Why are clones problematics](#why-are-clones-problematics)
## Re-engineering

### Why do systems need to change?
1. Obsolete hardware, such as disk and cpu.
2. new hardware requires new operating systems.
3. language evolution
4. requirements of new functionalities & addressing security vulnerabilities & change of components & Increase of scale and complexity
5. subject to market forces
   1. user's habits
   2. change in business processes: workflows in organisations
   3. business structure
6. unexpected issues
   1. millennium bug
   2. The year 2038 Problem: cuz a 32-bit integer, the maximum is hit on the 19th Jan. 2038

### why not start from scratch

- Risks
  - difficult to anticipate cost and ensure reliability
  - high stakes for business critical systems

## A Preliminary Assessment

> Background reading: Object oriented Re-engineering patterns **Chapter3**

### SWOT Report

- needs to identify: Strengths, Weaknesses, Opportunities, Threats.

1. install, run, discuss with users/ maintainers/ developers
2. aspects of functionality
3. analysis of source code: skim structure
4. analysis of documentation

### Understanding how the systems work

- Speculate about design
  1. need to produce a manageable, useful design of the system with key entities and relationships between them.
     - UML class diagram
  2. 'top-down'  approach
      - for each high-level functionality, divide ite into key steps,
      - for each step, identify the main corresponding component in the source code.

- identify the exceptional entities
  - too big file?

- follow data
  - which entities in the system store valuable data?

### Bash

```bash
- wc [options] [file]
  - word count, can use -l or -lines  to count number of lines
  - wc -l pom.xml

- find  [directory] [options] [pattern]
  - find files or directions
  - find . -name "*.java"

- grep [options] [pattern] [files]
  - find a pattern in a file.
  - grep main run.java

# find help 
man wc
```

#### Jupyter

- use it to run bash and show the process to peers

##### cygwin

##### kernel

### WEKA Java ML Framework

## Source Code

### Static analysis

1. From **source code**, Parsing from source text into abstract syntax tree.
2. From **compiled code/Bytecode** decompilation, byte-code analysis.
3. Generate Dependence graphs, Call graphs, Def-use chains...
4. Find useful information:
   1. Metrics (to indicate potential problems):
      1. Structural - number of methods, sub-classes, associations.
      2. Centrality - Fan-in(number of incoming calls) or Fan-out.
   2. Necessary data for visualisations:
      1. Class Diagrams, Sequence diagrams etc.
   3. slices, dead code, dependence relations...

### Analysis Tech

#### Source code analysis

1. Intra-procedural
   1. examine control and data-flow within a procedure(method)
   2. Useful for identifying information-flow vulnerabilities, etc.
2. inter-procedural
   1. macro view
   2. analyse function/method calls
   3. class hierarchies and associations

#### ByteCode Analysis

1. Tools
   1. Reflection
   2. ASM
   3. JGraphT
   4. GraphViz

#### Reverse-Engineering a Class Diagram

byte .class files --> UML Class Diagram

##### inheritance relations & association relations

subclass superclass

```bash
# use Graphviz generate a graph from .dot
dot -Tps filename.dot -o outfile.ps

dot -Tpdf filename.dot -o outfile.pdf

# -Tformatsets the format of the output. Allowed values forformatare:
# bmp, 	pdf, png ...
# more specific, see Page.27 of 
https://graphviz.gitlab.io/_pages/pdf/dotguide.pdf
```



## Dynamic analysis

- cpu time usage
- memory usage
- electricity usage

### Loggers

- Frameworks used to facilitate runtime tracking instructions. 

### Execution traces

- How to track Execution traces?
  - Application-level: println();
  - VM-Level: Java instrumentation/ Aspect-oriented logging
  - OS-level: dTrace scripts Wireshark

#### Instrumentation Agent in Java

- Agent code

  ```java
  public static void premain(String agentArguments, Instrumentation instrumenttation){
    instrumentation.addTransformer(new Transformer(agentArguments));
  }
  
  // in Transformer class
  public class Transformer implements ClassFileTransformer {
      private void changeMethod(CtBehavior method, String className) throws ...{
        ...
      }
  }
  ```

- An Agent code is attached in a jar file
  
- using Maven plugin: maven-assembly-plugin
```xml
<plugin>
                <groupId>org.apache.maven.plugins</groupId>
                <artifactId>maven-assembly-plugin</artifactId>
                <version>3.1.1</version>

                <configuration>
                    <descriptorRefs>
                        <descriptorRef>jar-with-dependencies</descriptorRef>
                    </descriptorRefs>
                    <archive>
                        <index>true</index>
                        <manifest>
                            <addDefaultImplementationEntries>true</addDefaultImplementationEntries>
                            <addDefaultSpecificationEntries>true</addDefaultSpecificationEntries>
                        </manifest>
                        <manifestEntries>
                            <Premain-Class>dynamicAnalysis.agent.LogMain</Premain-Class>
                        </manifestEntries>
                    </archive>
                </configuration>

                <executions>
                    <execution>
                        <id>make-assembly</id>
                        <phase>package</phase>
                        <goals>
                            <goal>single</goal>
                        </goals>
                    </execution>
                </executions>

            </plugin>
```



- Selectivity is vital
  - The size of trace file is impossible to predict in general.
  
## Clones code

### Why are clones problematics

1. Normalise files
   1. Remove white space
   2. try to remove comments
   3. eliminate lines with trivial symbols







