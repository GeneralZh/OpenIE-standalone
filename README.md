Open IE 5.0
======
This project contains the principal Open Information Extraction (Open IE)
system from the University of Washington (UW) and Indian Institute of Technology,Delhi (IIT Delhi).  An Open IE system runs over
sentences and creates extractions that represent relations in text.  For
example, consider the following sentence.

    The U.S. president Barack Obama gave his speech on Tuesday to thousands of people.

There are many binary relations in this sentence that can be expressed as a
triple `(A, B, C)` where `A` and `B` are arguments, and `C` is the relation
between those arguments.  Since Open IE is not aligned with an ontology, the
relation is a phrase of text.  Here is a possible list of the binary relations
in the above sentence:

    (Barack Obama, is the president of, the U.S.)
    (Barack Obama, gave, his speech)
    (Barack Obama, gave his speech, on Tuesday)
    (Barack Obama, gave his speech, to thousands of people)

The first extraction in the above list is a "noun-mediated extraction", because
the extraction has a relation phrase is described by the noun "president".  The
other extractions are very similar.  In fact, they can be represented more
informatively as an n-ary extraction.  An n-ary extraction can have 0 or more
secondary arguments.  Here is a possible list of the n-ary relations in the
sentence:

    (Barack Obama, is the president of, the U.S.)
    (Barack Obama, gave, [his speech, on Tuesday, to thousands of people])

Extractions can include more than just the arguments and relation as well.  For
example, we might be interested in whether the extraction is a negative
assertion or a positive assertion, or if it is conditional in some way.
Consider the following sentence:

    Some people say Barack Obama was born in Kenya.

We would not want to extract that `(Barack Obama, was born, in Kenya)` alone
because this is not true.  However, if we have the condition as well, we can
have a correct extraction.

    Some people say:(Barack Obama, was born in, Kenya)

To see an example of Open IE being used, please visit http://openie.cs.washington.edu/.

## Improvements over Open IE 4.0

Firstly, Open IE 5.0 improves upon extractions from numerical sentences. For example, consider the following sentence.

    Barack Obama is 6 feet tall.

Open IE 5.0 gives the following extractions:
    
    (Barack Obama, has height of, 6 feet)
    (Barack Obama, is, 6 feet tall)

Open IE 5.0 can also extract implicit numerical relations from units in sentences. For example, consider the following sentence.

    James Valley has 5 sq kms of fruit orchards.
    
The extractions are the following:

    (James Valley, has area of fruit orchards, 5 sq kms)
    (James Valley, has, 5 sq kms of fruit orchards)
    

Secondly, Open IE 5.0 improves upon conjunctive sentences by breaking conjunctions in arguments to generate multiple extractions. For example, consider the following sentence.

    Jack and Jill visited India, Japan and South Korea.
    
Open IE 5.0 gives the following extractions:

    (Jack, visited, India)
    (Jill, visited, India)
    (Jack, visited, Japan)
    (Jill, visited, Japan)
    (Jack, visited, South Korea)
    (Jill, visited, South Korea)
    

## Citing Open IE 5.0

Open IE 5.0 is a combination of SRLIE, Relnoun, BONIE(Extraction from Numerical Sentences) and ListExtraction(Extraction from conjunctive sentences). The relevant papers for the first three are:

   1. Janara Christensen, Mausam, Stephen Soderland, Oren Etzioni. "An Analysis of Open Information Extraction based on Semantic Role Labeling". International Conference on Knowledge Capture (KCAP). Banff, Alberta, Canada. June 2011. [pdf](http://www.cse.iitd.ac.in/~mausam/papers/kcap11.pdf)
    
   2. Harinder Pal, Mausam. "Demonyms and Compound Relational Nouns in Nominal Open IE". Workshop on Automated Knowledge Base Construction (AKBC) at NAACL. San Diego, CA, USA. June 2016. [pdf](http://www.cse.iitd.ac.in/~mausam/papers/akbc16.pdf)
    
   3. Swarnadeep Saha, Harinder Pal, Mausam. "Bootstrapping for Numerical Open IE". Annual Meeting of the Association for Computational Linguistics (ACL). Vancouver, Canada. August 2017. [pdf](http://www.cse.iitd.ac.in/~mausam/papers/acl17.pdf)
    
A survey paper summarizing about ten years of progress in Open IE:
    
   Mausam. "Open Information Extraction Systems and Downstream Applications". Invited Paper for Early Career Spotlight Track. International Joint Conference on Artificial Intelligence (IJCAI). New York, NY. July 2016. [pdf](http://www.cse.iitd.ac.in/~mausam/papers/ijcai16a.pdf)


## Google Group

* [knowitall_openie](https://groups.google.com/forum/#!forum/knowitall_openie)

## Research

Open IE 5.0 is the successor to Open IE 4.x and Open IE 4.x is the successor to [Ollie](http://www.gitub.com/knowitall/ollie).
Open IE 5.0 improves extractions from noun relations([RelNoun](https://homes.cs.washington.edu/~mausam/papers/akbc16.pdf)), numerical relations([BONIE](https://homes.cs.washington.edu/~mausam/papers/acl17.pdf)) and conjunctive sentences([ListExtractor](https://github.com/swarnaHub/OpenIEListExtractor)). Whereas Ollie used bootstrapped dependency parse paths to extract relations (see [Open Language Learning for Information Extraction](https://homes.cs.washington.edu/~mausam/papers/emnlp12a.pdf)), Open IE 4.x uses similar argument and relation expansion heuristics to create Open IE extractions from SRL frames.  Open IE 4.x also extends the defintion of Open IE extractions to include n-ary extractions (extractions with 0 or more arguments 2s).

## Buiding

First, download the standalone jar for BONIE from [here](https://github.com/dair-iitd/OpenIE-standalone/releases/download/v5.0/BONIE.jar) and place it inside a `lib` folder(create the `lib` folder parallel to the `src` folder).

Also, download the standalone jar for Conjunctive Sentences work from [here](https://github.com/dair-iitd/OpenIE-standalone/releases/download/v5.0/ListExtractor.jar) and place it inside the `lib` folder.

Extractions from Conjunctive Sentences uses Berkeley Language Model. Download the Language Model file from [here](https://drive.google.com/file/d/0B-5EkZMOlIt2cFdjYUJZdGxSREU/view?usp=sharing) and place it inside a data folder(create the `data` folder parallel to the `src` folder)

`openie` uses java-7-openjdk & the [sbt build system](http://www.scala-sbt.org/), so downloading
dependencies and compiling is simple.  Just run:

    sbt compile
    
Open IE uses scala 2.10.2. In case of a version mismatch problem, try using Scala 2.10.2.

## Running

You can run `openie` with sbt or create a stand-alone jar.  `openie` requires
substantial memory.  `sbt` is configured to use these options by default:

   -Xmx10G -XX:+UseConcMarkSweepGC
   
OpenIE's large memory requirements largely accounts to the fact that it currently uses Berkeley Language Model in the background.

### Running with sbt

    sbt 'run-main edu.knowitall.openie.OpenIECli'

### Running from a stand-alone jar.

First create the stand-alone jar.

    sbt clean compile assembly

You may need to add the above memory options.

    sbt -J-Xmx10000M clean compile assembly

Then you can run the resulting jar file as normal.

    java -jar openie-assembly.jar

You may need to add the above memory options.

    java -Xmx10g -XX:+UseConcMarkSweepGC -jar openie-assembly.jar
    
The WordNet folder and the data/languageModel file must be placed parallel to the standalone openie jar, while running it.

### Command Line Interface

`openie` takes one sentence per line unless `--split` is specified.  If
`--split` is specified, the input text will be split into sentences.  You can
either pipe input from Standard Input, specify an input file (an option first
argument), or type sentences interactively.  Output will be written to Standard
Output unless a second option argument is specified for an output file.

`openie` takes a number of command line arguments.  To see them all run
`java -jar openie-assembly.jar --usage`.  Of particular interest are
`--ignore-errors` which continues running even if an exception is encountered, `--binary` which gives the binary(triples) output and `--split` which splits the input document text into sentences.

There are two formats--a simple format made for ease of reading and a
columnated format used for machine processing.  The format can be specified
with either `--format simple` or `--format column`.  The simple format is
chosen by default.

### Contact

* Swarnadeep Saha( writetoswarna@gmail.com )
