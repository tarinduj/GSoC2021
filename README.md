# Machine Learning Guided Ordering of Compiler Optimization Passes
**_written by_ Tarindu Jayatilaka _for_ Google Summer of Code 2021 _with_ LLVM Compiler Infrastructure**

Ausgust 20, 2021

## Introduction

Developers generally use standard optimization pipelines like -O2 and -O3 to optimize their code.  Manually craftedheuristics are used to determine which optimization passes to select and how to order the execution of those passes. However, this process is not tailored for a particular application, or kind of application, as it is designed to perform“reasonably well” for any input.We want to improve the existing heuristics or replace the heuristics with machine learning-based models so thatthe LLVM compiler can provide a superior order of the passes customized per application.

<img src="https://github.com/tarinduj/GSoC2021/blob/master/imgs/currentmethod.png" width="600">

## Motivation

If the dependencies between the existing Passes and Code Structure can be analyzed using LLVM’s internal statistics,the findings can be used to improve the existing heuristics or come up with new machine learning models to optimizethe  LLVM  compiler.   This  will  allow  the  compiler  to  order  the  transform  passes  in  a  superior  manner  tailored  toindividual applications (or groups of applications) and code structures.  A wide range of applications and applicationdevelopers will benefit from this with improved performance.

## Project Objectives

- Study the LLVM’s internal statistics and derive insights about the dependencies between existing passes.
- Create new standard pass-pipelines that will result in high-performance code depending on the program type.The desired pass-pipeline can be selected by the user in the form of a flag.  For instance:  -O3a, -O3b, . . .
- Improve  existing  LLVM  heuristics  to  select  the  best  ordering  of  optimization  passes  depending  on  the  codestructure.
- Build a machine learning model that can identify patterns in the code structure, and select the best ordering ofoptimization passes based on the recognized pattern

## Methodology

In this year's work, we focused on building a ML model that can decide between O1, O2, and O3 pass pipelines to apply to a given function.

We implemented everything on a forked LLVM Repo. Everything we implemented during the GSoC period can be found in [this diff](https://github.com/tarinduj/llvm-project/compare/a24b1d1b2033b6bb17b7ad1c58b15d34e078fdb8..bddefff9d64a6c86c921640105ff15f0a1cc64e3).

Introduce more code features and identify the most relevant features.

•Extend this to a larger dataset like SPEC. However, there is a problem in encoding a larger dataset.  The hyperpass pipeline gets longer, thus takes more memory and computation time.•Go beyond tabular code features by encoding the CFG or Calling Graph in a graph data structure and use graphneural networks for predictions.•Extending the local predictions for more passes beyond function passes.•Come up with a few different pipelines for beyond the current LLVM optimization pipelines.  One approach tocome  up  with  these  new  pipelines  would  be  to  use  RL  to  search  through  all  combination  pass  pipelines,  likeHuang et al.  [1] have suggested.  Search space could be reduced by identifying dependencies between passes andgrouping them together

## Resources

This was a continuation of last year's GSoC project. Head to [this link](https://github.com/tarinduj/Google-Summer-of-Code-2020/blob/gh-pages/index.md) find out more about last year's work.

We wrote a papar based on our current progress an presented it at [MLArchSys](https://sites.google.com/view/mlarchsys/isca-2021/accepted-papers?authuser=0) at [ISCA 2021](https://www.iscaconf.org/isca2021/), and [LLPP](https://llvm.org/devmtg/2021-08-09/) at [ICPP 2021](https://oaciss.uoregon.edu/icpp21/index.php).
