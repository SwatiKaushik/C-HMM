# C-HMM
Program to identify remote homologues from protein sequence database

C-HMM: Introduction
=========================

C-HMM is a software to detect remote/distant homologues from protein sequence databases. It is based on HMMs(Hidden Markov Models) for identifying the deep evolutionary relationships of protein sequences. The aim of developing C-HMM is to provide a platform for identifying distant protein relationships in less computational time against any user defined protein sequence database. C-HMM is divided into three modules:

1. Cascade-HMM: This is main module of C-HMM which allows sequence searches for many generations. Each generation consists of multiple Jackhmmer searches against a database. 

2. Custom-HMM: In this module, filtered hits (first generation) obtained by Cascade-HMM are clustered and clustered hits are used to generate HMM profiles. These HMM profiles are further used for initiating next generations. 

3. Cluster-HMM: This module allows clustering of the hits obtained by Cascade-HMM after every generation. This helps in reducing the search timings. It can be combined with Cascade-HMM.

Requirements
=========

For initiating C-HMM searches user must have following files: 

1. Input sequence: It can be any protein sequence in FASTA format (see example sample file)

2. Sequence Database: User can provide any protein sequence database in FASTA format (see example database)

3. HMMER3: C-HMM uses different utilities of HMMER3 package. Download HMMER package from  http://hmmer.janelia.org/

4. CD-HIT: For clustering criterion C-HMM implements CD-hit which removes redundant sequences  at a particular threshold. CD-hit can be downloaded from http://weizhongli-lab.org/cd-hit/download.php

Before commencing sequence searches change the path of above files and binaries in cascade.properties/cascade.properties-cust files.


Usage
=============

C-HMM is precompiled with Java7(CascadeCUST.jar). If you are using lower version of Java, recompile the source code (CascadeCUST.tar) using the following commands: 

1. Download and extract apache ant
2. export ANT_HOME="path to ANT directory"
3. export JAVA_HOME="path to Oracle JAVA 7"
4. extract CascadeCust.zip
5. cd CascadeCust
6. /path/apache-ant-1.5.2/bin/ant

Run C-HMM
==========

C-HMM can be called using following commands:

For running Cascade-HMM use:
java -jar CascadeCUST.jar cascade.properties 

For running Custom-HMM use:
java -jar CascadeCUST.jar cascade.properties-cust 


Computational Requirements
=============================

C-HMM can be run on linux/mac OS. C-HMM memory requirement depends on the size of sequence database. We recommend to use high memory machines/clusters. It is a multithreaded program implemented in Java. Multithreading options (# of threads, # of cpu per thread, maximum # of threads) can be declared in cascade.properties/cascade.properties-cust files.


Results Analysis
==========================

After completion of a sequence searches, C-HMM provides separate directories for each generation. Each generation directory would contain 3 results files:

1. gen_#_result.out: This file has information about the hits captured in each generation.
2. gen_#_connection.conn: Connection file stores the information about hits and query sequences with the E-value at which it was captured. This helps in backtracing and identifying the intermediate sequences in each generation.
3. commulative_result_seq_name.out: This file stores the commulative unique hits from each generation.

If user has opted for clustering of hits, each generation directory would also have gen_#_result_nr.out.clstr file. This file has information about clustering of hits.


Cascade Property File 
===============================
C-HMM provides many user defined options which can be declared in property file. All the options provided in the property files are explained below:

1. cascade.maxGeneration: This parameter defines the maximum number of generations for which user wants to initiate C-HMM. 

2. cascade.evalueCutoff: This parameter that describes the number of hits one can expect to see by chance while searching a database. It's default value is 10-3 (refer BLAST manual for more details).

3. cascade.subjectFilters: In this row user has to define h-value and length filter criterion. h-value is a inclusion threshold for the profile generation. hits below the h-value are not considered for profile generation. It's default value is 10-3.
Length filter defines the alignment length between the query and hit sequence. Each hit sequence length should be higher than the threshold. It's default value is 75%.

3. cascade.perGenerationIteration: This parameter describes the number of iterations of Jackhmmer to be performed in each generation. It's default value is 4.

4. cascade.clusteringCommandPrototype/cascade.custJack.custJackClust: These commands are used to define the clustering parameters of CD-hit. User can define a sequence identity threshold for the protein sequence clustering (refer CDhit for other options).

5. cascade.maxQueries: User can define the maximum number of hits to be used for initiating next generation of C-HMM in this parameter. To include all the collected hits leave this option blank. By default C-HMM uses 1000 random hits.

6. cascade.continuation: User can reinitiate cascade searches from the results of previous generation using this option. By default this option is set to "no". To continue sequence searches turn it to "yes".

7. cascade.continuedGeneration: User has to define the name of generation from which to reinitiate sequence searches. For eg. if you want to start from third generation than cascade.continuedGeneration=3.

8. cascade.continuationPrevGenOutput: In this parameter user has to describe the path of previous generation output files. For eg. if you want to restart third generation, give path of the output of second generation.

9. cascade.continuationExistingHitFile: Provide path to "commulative_result_seq_name.out" file of the previous generation.

10. cascade.inputFileExtension: User cand define the extension of input file. By default its filename.query.

11. cascade.outputFileExtension: User cand define the extension of input file. By default its filename.out.

12. cascade.connectionFileExtension: Connection file stores the information about corresponding hit and query sequence.

13. cascade.inputDirectory: Provide path to input directory.

14. cascade.outputDirectory: Provide path to output directory.

15. cascade.seqDatabase: Provide path to protein sequence database. Database should be in FASTA format. Please see sample database for reference.

16. cascade.binaryPath.- : Provide path to different binaries used by C-HMM.


--------------------------------------X Happy Cascading X------------------------------------------------------
