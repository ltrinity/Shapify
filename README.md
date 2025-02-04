# Shapify

#### Description:
Software implementation of Shapify.      
Shapify is an algorithm for predicting the pseudoknotted secondary structures of RNA using relaxed Hierarchical Folding. 
[![DOI](https://zenodo.org/badge/459850807.svg)](https://zenodo.org/badge/latestdoi/459850807)

Shapify is based on Iterative HFold, see below paper for details.
Paper: https://www.researchgate.net/publication/262810273_A_fast_and_robust_iterative_algorithm_for_prediction_of_RNA_pseudoknotted_secondary_structures

On the dataset tested in this paper, Iterative HFold generally has better accuracy that its predecessor, [HFold](https://github.com/HosnaJabbari/HFold).

#### Supported OS: 
Linux, macOS


### Installation:  
Requirements: A compiler that supports C++11 standard (tested with g++ version 4.9.0 or higher), Pthreads, and CMake version 3.1 or greater.    

[CMake](https://cmake.org/install/) version 3.1 or greater must be installed in a way that HFold can find it.    
To test if your Mac or Linux system already has CMake, you can type into a terminal:      
```
cmake --version
```
If it does not print a cmake version greater than or equal to 3.1, you will have to install CMake depending on your operating system.

#### Mac:    
Easiest way is to install homebrew and use that to install CMake.    
To do so, run the following from a terminal to install homebrew:      
```  
/usr/bin/ruby -e "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/master/install)"   
```    
When that finishes, run the following from a terminal to install CMake.     
```   
brew install cmake   
``` 
#### Linux:    
Run from a terminal     
```
wget http://www.cmake.org/files/v3.8/cmake-3.8.2.tar.gz
tar xzf cmake-3.8.2.tar.gz
cd cmake-3.8.2
./configure
make
make install
```
[Linux instructions source](https://geeksww.com/tutorials/operating_systems/linux/installation/downloading_compiling_and_installing_cmake_on_linux.php)

#### Steps for installation   
1. [Download the repository](https://github.com/HosnaJabbari/HFold_iterative.git) and extract the files onto your system.
2. From a command line in the root directory (where this README.md is) run
```
cmake -H. -Bbuild
cmake --build build
```   
If you need to specify a specific compiler, such as g++, you can instead run something like   
```
cmake -H. -Bbuild -DCMAKE_CXX_COMPILER=g++
cmake --build build
```   
This can be useful if you are getting errors about your compiler not having C++11 features.

After installing you can move the executables wherever you wish, but you should not delete or move the simfold folder, or you must recompile the executables. If you move the folders and wish to recompile, you should first delete the created "build" folder before recompiling.

#### How to use:
    Arguments:
        HFold_iterative:
            --s <sequence>
            --r <structure>
            --i </path/to/file>
            --o </path/to/file>
            --n <number of suboptimal structures to output>
            --hotspot_only </path/to/file>
            --shape </path/to/file>
            --b <number>
            --m <number>

        Remarks:
            make sure the <arguments> are enclosed in "", for example --r "..().." instead of --r ..()..
            input file for --i must be .txt
            if --i is provided with just a file name without a path, it is assuming the file is in the diretory where the executable is called
            if --o is provided with just a file name without a path, the output file will be generated in the diretory where the executable is called
            if --o is provided with just a file name without a path, and if --i is provided, then the output file will be generated in the directory where the input file is located

            You can also include SHAPE data to be used. 
            The SHAPE data must be in a file with 1 number per line.
            The number corresponds with each nucleotide in order, and the file must be exactly the same length as the sequence.
            --shape ("filename") to specify a file for shape data
            --b (number) to specify an intercept for the shape data (default is -0.600000)
            --m (number) to specify a slope for the shape data (default is 1.800000)

            --hotspot_only does not run the actual HFold_iterative program. This tells the program to generate 
            restricted structures and write to the provided file. Usually used for large sequences and need to split it 
            up and run it independently with an outside script
            example file when calling ./HFold_iterative --s "GCAACGAUGACAUACAUCGCUAGUCGACGC" --hotspot_only "./hotspot_file.txt"
            Seq1_hotspot_0: ____(((((_____)))))___________
            Seq1_hotspot_1: ____((((______________))))____
            Seq1_hotspot_2: ____((((_______))))___________
            Seq1_hotspot_3: _______((((___________))))____
            Seq1_hotspot_4: ____(((_________)))___________
            -----

    
    Sequence requirements:
        containing only characters GCAUT

    Structure requirements:
        -pseudoknot free
        -containing only characters ._(){}[]
        Remarks:
            Restricted structure symbols:
                () restricted base pair
                _ no restriction


    Input file requirements:
            Line1: Sequence
            Line2: Structure
        sample:
            GCAACGAUGACAUACAUCGCUAGUCGACGC
            (____________________________)

#### Example:
    assume you are in the directory where the HFold_iterative executable is loacted
    ./HFold_iterative --s "GCAACGAUGACAUACAUCGCUAGUCGACGC"
    ./HFold_iterative --i "/home/username/Desktop/myinputfile.txt"
    ./HFold_iterative --i "/home/username/Desktop/myinputfile.txt" -o "/home/username/Desktop/some_folder/outputfile.txt"
    ./HFold_iterative --s "GCAACGAUGACAUACAUCGCUAGUCGACGC" --r "(____________________________)"
    ./HFold_iterative --s "GCAACGAUGACAUACAUCGCUAGUCGACGC" --n 10
    ./HFold_iterative --s "GCAACGAUGACAUACAUCGCUAGUCGACGC" --r "(____________________________)" -o "outputfile.txt"
    ./HFold_iterative --s "GCAACGAUGACAUACAUCGCUAGUCGACGC" --r "(____________________________)" -o "/home/username/Desktop/some_folder/outputfile.txt"
    ./HFold_iterative --s "GCAACGAUGACAUACAUCGCUAGUCGACGC" -r "(____________________________)" --shape "shapefile" --b -0.4 --m 1.3
    
#### Exit code:
    0       success
    1	    invalid argument error 
    3	    thread error
    4       i/o error
    5       pipe error
    6       positive energy error
    error code with special meaning: http://tldp.org/LDP/abs/html/exitcodes.html
    2	    Misuse of shell builtins (according to Bash documentation)
    126	    Command invoked cannot execute
    127	    "command not found"
    128	    Invalid argument to exit	
    128+n	Fatal error signal "n"
    130	    Script terminated by Control-C
    255	    Exit status out of range (range is 0-255)
