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

We started off the project by extending our code features set from last year, by adding new code features. Next, we modified the `<PassManager>` to be able to switch the pass pipeline, with a logic similar to `<PassInstrumentation::runBeforePass()>`. We also modified the `<PassManager::addPass()>` method and the `<PassBuilder>` to access the optimization levels associated with the pass from within the `<PassManager>`.

We wrote two new passes for LLVM.
1. MLPassPipelinePredictor: This pass can predict optimization levels for functions using machine learning. We also utilize it to dump training data.
2. FunctionAnnotation: This pass is used to annotate functions with the the optimization level predicted by the `<MLPassPipelinePredictor>`. 

We embeded the `<FunctionAnnotationPass>` in the pass pipeline after `<InstCombinePass>`. Then used it to extrat our training dataset. The script we are using to create the dataset can be found [here](https://github.com/tarinduj/llvm-mlpm).

We are currently working on analyzing the dataset and building the prediction model. Jupyter Notebooks we used to analyze the datasets can be found [here](https://github.com/tarinduj/GSoC2021/tree/master/notebooks).

[Take OptimizationLevel Class Out of the Pass Builder](https://reviews.llvm.org/D107025) was the the only patch we could upstream within the GSoC period. Other patchs and the RFC are still a work in progress. 

## Resources

This was a continuation of last year's GSoC project. Head to [this link](https://github.com/tarinduj/Google-Summer-of-Code-2020/blob/gh-pages/index.md) find out more about last year's work.

We wrote a papar based on our current progress an presented it at [MLArchSys](https://sites.google.com/view/mlarchsys/isca-2021/accepted-papers?authuser=0) at [ISCA 2021](https://www.iscaconf.org/isca2021/), and [LLPP](https://llvm.org/devmtg/2021-08-09/) at [ICPP 2021](https://oaciss.uoregon.edu/icpp21/index.php).
