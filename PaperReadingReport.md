# Paper Reading Report

## Introduction

APISAN is a tool that is aimed to automatically detect API misuses. The key idea is to learn a pattern of how APIs are used and then mark deviations as possible bugs.

<img src="pics/intro_example.png" alt="intro_example" style="zoom:50%;" />

Above is an example that helps to illustrate this. On the right is a collection of similar usages where when handling error it is required to free a pointer before returning. On the left is a bug where one forgets to free the pointer when handling error. After learning this pattern from a large number of examples similar to the right, the tool reports the deviation on the left as a bug.

The workflow consists of three steps.

## Step 1: Building symbolic contexts

The first step is to convert the code into symbolic representation.

There are two important features: one is to ragard all function calls as symbolic symbols and never 'dive into' the implementation of the APIs. The other is unroll the loop only once in order to simplify the execution tree.

<img src="pics/symbolic_example1.png" alt="symbolic_example1" style="zoom:50%;" />

<img src="pics/symbolic_example2.png" alt="symbolic_example2" style="zoom:50%;" />

Above is an example of how the symbolic representations are built. Firstly it is converted to a tree graph that diverge according to the conditional and loop statements in the code. Then each path in the graph is converted to a sequence of 'assume's and 'call's.

## Step 2: Inferring semantic beliefs

The second step is to extract semantic rules out of a large amount of symbolic contexts. The method can 'discover' rules on itself, as long as it is given patterns of rules to look for. In the paper, four patterns are given.

### 