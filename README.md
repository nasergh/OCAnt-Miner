# Ant-Miners
In here i try to collect all version of Ant-Miners. it's included mainly myra project.
collection of Ant Colony Optimization (ACO) algorithms for the data mining classification and regression tasks. It includes popular rule induction and decision tree induction algorithms. The algorithms are ready to be used from the command line or can be easily called from your own Java code. They are implemented using a modular architecture, so they can be easily extended to incorporate different procedures and/or use different parameter values.

This repository contains a complete rewrite of the code (by the same author) from the MYRA project hosted at [sourceforge](http://sourceforge.net/projects/myra/). The computational time has been significantly improved &mdash; tasks that used to take minutes, now are done in seconds &mdash; although it was not possible to maintain backward compatibility. You will find that the overall architecture is very similar, but most of the data structures have changed.

While this repository is a fresh start, the versioning is maintained &mdash; the initial version of the refactored is `4.x`. Most algorithms from version `3.x` have been included in the refactored code, although the GUI is currently not available on version `5.x`.

###### Latest Release

* [JAR file (myra-5.0.jar)](https://github.com/febo/myra/releases/download/v5.0/myra-5.0.jar)

* Maven dependency:

```
<dependency>
   <groupId>com.github.febo</groupId>
   <artifactId>myra</artifactId>
   <version>5.0</version>
</dependency>
```

### Algorithms

The following algorithms are implemented:

##### Ant-Miner
```
Main class: myra.algorithm.AntMiner
```

The first rule induction ACO classification algorithm. Ant-Miner uses a sequentical covering strategy combined with an ACO search to create a list of rules. Ant-Miner only supports categorical attributes, continuous attributes need to be discretised in a pre-processing step.

##### Continuous Ant-Miner (*c*Ant-Miner)
```
Main class: myra.algorithm.ContinuousAntMiner
```

An extension of Ant-Miner to cope with continuous attributes. It works essentially the same as Ant-Miner, but continuous attributes undergo a dynamic discretisation process when selected to be included in a rule.

##### Pittsburgh Continuous Ant-Miner (*c*Ant-Miner<sub>PB</sub>)
```
Main class: myra.algorithm.PittsburghContinuousAntMiner
```

*c*Ant-Miner<sub>PB</sub> incorporates  a new strategy to discover a list of classification rules, which guides the search performed by the ACO algorithm using the quality of a candidate list of rules, instead of a single rule. The main motivation is to avoid the problem of rule interaction derived from the order in which the rules are discovered &mdash; i.e., the outcome of a rule affects the rules that can be discovered subsequently since the search space is modified due to the removal of examples covered by previous rules.

##### Unordered Pittsburgh Continuous Ant-Miner (Unordered *c*Ant-Miner<sub>PB</sub>)
```
Main class: myra.algorithm.UnorderedPittsburghContinuousAntMiner
```

An extension to the *c*Ant-Miner<sub>PB</sub> in order to discover unordered rules to improve the interpretation of individual rules. In an ordered list of rules, the effect (meaning) of a rule depends on all previous rules in the list, since a rule is only used if all previous rules do not cover the example. On the other hand, in an unordered set of rules, an example is shown to all rules and, depending on the conflict resolution strategy, a single rule is used to make a prediction.

##### Ant-Tree-Miner
```
Main class: myra.algorithm.AntTreeMiner
```

A decision tree induction algorithm that uses an ACO procedure to creates decision trees. Trees are created in a top-down fashion, similar to C4.5 strategy, but instead of using a greedy procedure based on the information gain, it selects decision nodes using an ACO procedure.

##### Ant-Miner-Reg
```
Main class: myra.algorithm.AntMinerReg
```

The first rule induction ACO regression algorithm. Ant-Miner-Reg uses a sequentical covering strategy combined with an ACO search to create a list of regression rules.

##### Hierarchical Multi-Label Ant-Miner (*hm*Ant-Miner)
```
Main class: myra.algorithm.HierarchicalMultiLabelAntMiner
```

*hm*Ant-Miner is the first Ant-Miner variation for hierarchical multi-label classification problems. *hm*Ant-Miner uses a sequentical covering strategy combined with an ACO search to create a list of hierarchical classification rules that predict multiple class labels from a hierarchy.

### Running the algorithms

All algorihtms can be used in the command line:

```
java -cp myra-<version>.jar <main class> -f <arff training file>
```

where `<version>` is MYRA version number (e.g., `5.0`), `<main class>` is the main class name of the algorithm and `<aff training file>` is the path to the ARFF file to be used as training data. The minimum requirement to run an algorihtm is a training file. If no training file is specified, a list of options is printed:

```
[febo@uok myra]$ java -cp myra-4.5.jar myra.algorithm.ContinuousAntMiner

Usage: ContinuousAntMiner -f <arff_training_file> [-t <arff_test_file>] [options]

The minimum required parameter is a training file to build the model from. If a 
test file is specified, the model will be tested at the end of training. 

The following options are available:

  -c <size>             specify the size of the colony (default 60) 

  -d <method>           specify the discretisation method [c45 | mdl] (default 
                        mdl) 

  -g                    enables the dynamic heuristic computation 

  -h <method>           specify the heuristic method [gain | none] (default 
                        gain) 

  -i <number>           set the maximum number of iterations (default 1500) 

  -m <number>           set the minimum number of covered examples per rule 
                        (default 10) 

  -p <method>           specify the rule pruner method [backtrack | greedy] 
                        (default backtrack) 

  -r <function>         specify the rule quality function [accuracy | laplace | 
                        sen_spe] (default sen_spe) 

  -s <seed>             Random seed value (default current time) 

  -u <number>           set the allowed number of uncovered examples (default 
                        10) 

  -x <iterations>       set the number of iterations for convergence test 
                        (default 10) 

  --export <file>       Path of the file to save the model 

  --parallel <cores>    enable parallel execution in multiple cores; if no cores 
                        are specified, use all available cores 
```
Usinng command-line options you can tweak the parameters of an algorithm. Note that when running the algorithm in parallel (`--parallel` option), there is no guarantee that it will have the same behaviour even if the same seed value is used (`-s` option), since the thread allocation is not controlled by the code.

### Citation Policy

If you publish material based on algorithms from MYRA, please include a reference to the paper describing the algorithm. All papers are available [online](http://cs.kent.ac.uk/~febo).

If you also would like to make a reference to the MYRA repository, please include a citation to:
```
@INPROCEEDINGS{otero:myra,
    author    = "F.E.B. Otero",
    year      = "2017",
    title     = "{MYRA}: {A} {J}ava {A}nt {C}olony {O}ptimization {F}ramework for {C}lassification {A}lgorithms",
    booktitle = {Proceedings of the Genetic and Evolutionary Computation Conference Companion (GECCO '17 Companion)},
    publisher = {ACM Press},
    pages     = {1247--1254},
    year      = {2017}
}
```

##### Ant-Miner

* R.S. Parpinelli, H.S. Lopes and A.A. Freitas. Data Mining with an Ant Colony Optimization Algorithm. In: IEEE Transactions on Evolutionary Computation, Volume 6, Issue 4, pp. 321&ndash;332. IEEE, 2002.

```
@ARTICLE{Parpinelli02datamining,
    author  = {R.S. Parpinelli and H.S. Lopes and A.A. Freitas},
    title   = {Data Mining with an Ant Colony Optimization Algorithm},
    journal = {IEEE Transactions on Evolutionary Computation},
    year    = {2002},
    volume  = {6},
    number  = {4},
    pages   = {321--332}
}
```
##### Continuous Ant-Miner (*c*Ant-Miner)

* F.E.B. Otero, A.A. Freitas and C.G. Johnson. cAnt-Miner: an ant colony classification algorithm to cope with continuous attributes. In: Ant Colony Optimization and Swarm Intelligence (Proc. ANTS 2008), Lecture Notes in Computer Science 5217, pp. 48&ndash;59. Springer, 2008.
```
@INPROCEEDINGS{Otero08datamining,
    author    = {F.E.B. Otero and A.A. Freitas and C.G. Johnson},
    title     = {\emph{c}{A}nt-{M}iner: an ant colony classification algorithm to cope with continuous attributes},
    booktitle = {Proceedings of the 6th International Conference on Swarm Intelligence (ANTS 2008), Lecture Notes in Computer Science 5217},
    editor    = {M. Dorigo and M. Birattari and C. Blum and M. Clerc and T. St{\" u}tzle and A.F.T. Winfield},
    publisher = {Springer-Verlag},
    pages     = {48--59},
    year      = {2008}
}
```

* F.E.B. Otero, A.A. Freitas and C.G. Johnson. Handling continuous attributes in ant colony classification algorithms. In: Proceedings of the 2009 IEEE Symposium on Computational Intelligence in Data Mining (CIDM 2009), pp. 225&ndash;231. IEEE, 2009.
```
@INPROCEEDINGS{Otero09datamining,
    author    = {F.E.B. Otero and A.A. Freitas and C.G. Johnson},
    title     = {Handling continuous attributes in ant colony classification algorithms},
    booktitle = {Proceedings of the 2009 IEEE Symposium on Computational Intelligence in Data Mining (CIDM 2009)},
    publisher = {IEEE},
    pages     = {225--231},
    year      = {2009}
}
```

##### Pittsburgh Continuous Ant-Miner (*c*Ant-Miner<sub>PB</sub>)

* F.E.B. Otero, A.A. Freitas and C.G. Johnson. A New Sequential Covering Strategy for Inducing Classification Rules with Ant Colony Algorithms. In: IEEE Transactions on Evolutionary Computation, Volume 17, Issue 1, pp. 64&ndash;74. IEEE, 2013.
```
@ARTICLE{Otero13covering,
    author  = {F.E.B. Otero and A.A. Freitas and C.G. Johnson},
    title   = {A New Sequential Covering Strategy for Inducing Classification Rules with Ant Colony Algorithms},
    journal = {IEEE Transactions on Evolutionary Computation},
    year    = {2013},
    volume  = {17},
    number  = {1},
    pages   = {64--74}
}
```

* M. Medland, F.E.B. Otero and A.A. Freitas. Improving the cAnt-MinerPB Classification Algorithm. In: Swarm Intelligence (Proc. ANTS 2012), Lecture Notes in Computer Science 7461, pp. 73&ndash;84. Springer, 2012.
```
@INPROCEEDINGS{Medland12datamining,
    author    = {M. Medland and F.E.B. Otero and A.A. Freitas},
    title     = {Improving the $c$Ant-Miner$_{\mathrm{PB}}$ Classification Algorithm},
    booktitle = {Swarm Intelligence, Lecture Notes in Computer Science 7461},
    editor    = {M. Dorigo and M. Birattari and C. Blum and A.L. Christensen and A.P. Engelbrecht and R. Gro{\ss} and T. St{\"u}tzle},
    publisher = {Springer-Verlag},
    pages     = {73–-84},
    year      = {2012}
}
```

##### Unordered Pittsburgh Continuous Ant-Miner (Unordered *c*Ant-Miner<sub>PB</sub>)

* F.E.B. Otero and A.A. Freitas. Improving the Interpretability of Classification Rules Discovered by an Ant Colony Algorithm: Extended Results. Evolutionary Computation, Volume 24, Issue 3, pp. 385&ndash;409. MIT Press, 2015.
```
@ARTICLE{Otero16evco,
    author  = {F.E.B. Otero and A.A. Freitas},
    title   = {Improving the Interpretability of Classification Rules Discovered by an Ant Colony Algorithm: Extended Results},
    journal = {Evolutionary Computation},
    year    = {2016},
    volume  = {24},
    number  = {3},
    pages   = {385--409}
}
```

* F.E.B. Otero and A.A. Freitas. Improving the Interpretability of Classification Rules Discovered by an Ant Colony Algorithm. In: Proceedings of the Genetic and Evolutionary Computation Conference (GECCO '13), pp. 73&ndash;80, 2013.
```
@INPROCEEDINGS{Otero13unordered,
    author    = {F.E.B. Otero and A.A. Freitas},
    title     = {Improving the Interpretability of Classification Rules Discovered by an Ant Colony Algorithm},
    booktitle = {Proceedings of the Genetic and Evolutionary Computation Conference (GECCO'13)},
    publisher = {ACM Press},
    pages     = {73--80},
    year      = {2013}
}
```

##### Ant-Tree-Miner

* F.E.B. Otero, A.A. Freitas and C.G. Johnson. Inducing Decision Trees with An Ant Colony Optimization Algorithm. Applied Soft Computing, Volume 12, Issue 11, pp. 3615&ndash;3626, 2012.
```
@ARTICLE{Otero12tree,
    author  = {F.E.B. Otero, A.A. Freitas and C.G. Johnson},
    title   = {Inducing Decision Trees with An Ant Colony Optimization Algorithm},
    journal = {Applied Soft Computing},
    year    = {2012},
    volume  = {12},
    number  = {11},
    pages   = {3615--3626}
}
```

##### Ant-Miner-Reg

* J. Brookhouse and F.E.B. Otero. Discovering Regression Rules with Ant Colony Optimization. In: Proceedings of the Genetic and Evolutionary Computation Conference Companion (GECCO '15 Companion), pp. 1005&ndash;1012, 2015.
```
@INPROCEEDINGS{Brookhouse15regression,
    author    = {J. Brookhouse and F.E.B. Otero},
    title     = {Discovering Regression Rules with Ant Colony Optimization},
    booktitle = {Proceedings of the Genetic and Evolutionary Computation Conference Companion (GECCO '15 Companion)},
    publisher = {ACM Press},
    pages     = {1005--1012},
    year      = {2015}
}
```

##### Hierarchical Multi-Label Ant-Miner (*hm*Ant-Miner)

* F.E.B. Otero, A.A. Freitas and C.G. Johnson. Inducing Decision Trees with An Ant Colony Optimization Algorithm. Applied Soft Computing, Volume 12, Issue 11, pp. 3615&ndash;3626, 2012.
```
@ARTICLE{Otero10hierarchical,
    author  = {F.E.B. Otero, A.A. Freitas and C.G. Johnson},
    title   = {A Hierarchical Multi-Label Classification Ant Colony Algorithm for Protein Function Prediction},
    journal = {Memetic Computing},
    year    = {2010},
    volume  = {2},
    number  = {3},
    pages   = {165–-181}
}
```
