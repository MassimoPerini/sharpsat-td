# SharpSAT-TD

Main differences with the original SharpSAT-TD:
- Weights can be arbitrarily large integers
- Tree decomposition is disabled
- CMake edited for MacOS. To run on linux, remove the lines between #macos and #end macos inside CMakeLists.txt

[![DOI](https://zenodo.org/badge/DOI/10.5281/zenodo.4880703.svg)](https://doi.org/10.5281/zenodo.4880703)

Submission to model counting competition 2021 by Tuukka Korhonen and Matti Järvisalo (University of Helsinki).
SharpSAT-TD is based on [SharpSAT](https://github.com/marcthurley/sharpSAT), with the main new features being the use of tree decompositions in decision heuristics, new preprocessor, and directly supporting weighted model counting.


SharpSAT-TD supports exact model counting, exact weighted model counting with arbitrary precision floats, and exact weighted model counting with doubles.
See a detailed descriptions in [description.pdf](https://github.com/Laakeri/sharpsat-td/blob/main/description.pdf) and in [arXiv](https://arxiv.org/abs/2308.15819).
If you use SharpSAT-TD in a research paper, please cite at least one of [(Korhonen and Järvisalo, CP 2021)](https://drops.dagstuhl.de/entities/document/10.4230/LIPIcs.CP.2021.8) or [(Korhonen and Järvisalo, arXiv 2023)](https://arxiv.org/abs/2308.15819).


# Compiling

The external dependencies needed are the [GMP library](https://gmplib.org/), the [MPFR library](https://www.mpfr.org/), and [CMAKE](https://cmake.org/).

To compile and link dynamically use

``./setupdev.sh``

To compile and link statically use

``./setupdev.sh static``


The binaries sharpSAT and flow_cutter_pace17 will be copied to the [bin/](https://github.com/Laakeri/sharpsat-td/tree/main/bin) directory.

# Running

The currently supported input/output formats are those of [Model counting competition 2021](https://mccompetition.org/assets/files/2021/competition2021.pdf). Note that in the weighted model counting format, it is assumed that the weight of a literal and its negation sum up to exactly 1. SharpSAT-TD uses this assumption, although editing the input reading logic should be relatively straightforward by editing preprocessor/instance.cpp.


Example unweighted model counting:
`cd bin`
`./sharpSAT -decot 1 -decow 100 -tmpdir . -cs 3500 ../examples/track1_009.cnf`


Example weighted model counting with arbitrary precision:
`cd bin`
`./sharpSAT -WE -decot 1 -decow 100 -tmpdir . -cs 3500 -prec 20 ../examples/track2_003.wcnf`


In the competition setting the value of the `-decot` flag was 120.

## Flags

- `-decot` - the number of seconds to run flowcutter to find a tree decomposition. Required. Recommended value 60-600 if running with a total time budjet of 1800-3600 seconds.
- `-tpmdir` - the directory to store temporary files for running flowcutter. Required.
- `-decow` - the weight of the tree decomposition in the decision heuristic. Recommended value >1 if the heuristic should care about the tree decomposition.
- `-cs` - limit of the cache size. If the memory upper bound is X megabytes, then the value here should be around x/2-500.
- `-WE` - enable weighted model counting with arbitrary precision.
- `-WD` - enable weighted model counting with double precision. WARNING: With this flag there may be large errors in the output.
- `-prec` - the number of digits in output of weighted model counting. Does not affect the internal precision.
