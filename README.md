KMERFINDER
===================

This project documents the KmerFinder service


Documentation
=============

## What is it?

The KmerFinder service contains one python script *kmerfinder.py* which is the script of the latest
version of the KmerFinder service. The method is used to find the best match (species identification) to the reads in one
or more *fastq* files  or one *fasta* file in a (kmer) database produced using the KMA program.
The method outputs the best matches, along with additional taxonomic information, if that option is selected.

## Content of the repository
1. kmerfinder.py      - the program
2. test     	- test folder
3. README.md
4. Dockerfile   - dockerfile for building the kmerfinder docker container


## Installation

Setting up KmerFinder program
```bash
# Go to wanted location for resfinder
cd /path/to/some/dir
# Clone and enter the KmerFinder directory
git clone https://bitbucket.org/genomicepidemiology/kmerfinder.git
cd kmerfinder
```

Build Docker image from Dockerfile
```bash
# Build container
docker build -t kmerfinder .
# Run test
docker run --rm -it \
       --entrypoint=/test/test.sh kmerfinder
```

## Download and install KmerFinder database(s)

You can find instructions on how to download and install KmerFinder databases [here](https://bitbucket.org/genomicepidemiology/kmerfinder_db/src/master/)
```bash
# Go to the directory where you have stored the KmerFinder database
cd /path/to/database/dir
KmerFinder_DB=$(pwd)
```

## Dependencies
In order to run the program without using docker, Python 3.5 (or newer) should be installed.

#### KMA
Additionally KMA should be installed.
The newest version of KMA can be installed from here:
```url
https://bitbucket.org/genomicepidemiology/kma
```

## Usage

The program can be invoked with the -h option to get help and more information of the service.

```bash
docker run --rm -it \
        -v $KmerFinder_DB:/database \
        -v $(pwd):/workdir \
       kmerfinder -h
```

One way to run the program is to go inside the folder where your input files are:

```bash
cd /path/to/input/file
```
And then run:

```bash
docker run --rm -it \
        -v $KmerFinder_DB:/database \
        -v $(pwd):/workdir \
       kmerfinder -i [INPUTFILE] -o . -db /database/[DATABASE_NAME] -tax /database/[TAXONOMYFILE] -x
```

This way your results files will be placed inside your input files directory. In case you omit the "-o" argument
a folder named "output" will be created in you input files directory with the results of the analysis.

If you would like to save your results in a folder with specific name, you can run the above command with:

```bash
-o ./[FOLDERNAME]
```
and a folder with a specified name , holding the results of the analysis will be created inside the input file directory.

In case you do not want to use the taxonomy files you can omit the options: -tax /database/[TAXONOMYFILE] -x

Below is an example of running the above command using the bacteria database downloaded from our FTP site.

```bash
docker run --rm -it \
        -v $KmerFinder_DB:/database \
        -v $(pwd):/workdir \
       kmerfinder -i myfile.fastq.gz -o ./myresults -db /database/bacteria.ATG -tax /database/bacteria.name -x
```

Of course you can define the full path to your input file(s) and run the above commant as:
```bash
docker run --rm -it \
        -v $KmerFinder_DB:/database \
        -v /full/path/to/input/files:/workdir \
       kmerfinder -i [INPUTFILE] -o ./results -db /database/[DATABASE_NAME] -tax /database/[TAXONOMYFILE] -x
```

When running the docker file you have to mount 2 required directories:

1. KmerFinder_DB (KmerFinder database directory), which includes the databases downloaded from the CGE ftp site: [ftp://ftp.cbs.dtu.dk/public/CGE/databases/KmerFinder/version/]
2. An output/input folder from where the input file can be reached and an output files can be saved.

Here you can mount either the current working directory (using $pwd) which points to your input file(s)
or specify a full path to the directory of your input file(s) and use this as the output directory.

-i INPUTFILE	input file(s) (fasta or fastq)

-db DATABASE_NAME   KmerFinder database

-o OUTDIR	outpur directory relative to pwd or speficied directory of input files

-tax TAXONOMYFILE   file with taxonomic information for the KmerFinder database

-x 		extended output. Will create an extented output including taxonomic information

Example without Docker:
```bash
python3 kmerfinder.py -i [INPUTFILE/S] -o [OUTDIR] -db [KMERFINDER_DB location]/[SPECIE]/[SPECIE].[PREFIX] -tax [KMERFINDER_DB location]/[SPECIE]/[SPECIE].tax 
```

## Web-server

A webserver implementing the methods is available at the [CGE website](http://www.genomicepidemiology.org/) and can be found here:
http://cge.cbs.dtu.dk/services/KmerFinder/

Citation
=======

When using the method please cite:

Benchmarking of Methods for Genomic Taxonomy. Larsen MV, Cosentino S,
Lukjancenko O, Saputra D, Rasmussen S, Hasman H, Sicheritz-Pontén T,
Aarestrup FM, Ussery DW, Lund O. J Clin Microbiol. 2014 Feb 26.
[Epub ahead of print]

Rapid whole genome sequencing for the detection and characterization of
microorganisms directly from clinical samples. Hasman H, Saputra D,
Sicheritz-Ponten T, Lund O, Svendsen CA, Frimodt-Møller N, Aarestrup FM.
J Clin Microbiol.  2014 Jan;52(1):139-46.

Rapid and precise alignment of raw reads against redundant databases with KMA Philip T.L.C. Clausen, Frank M. Aarestrup, Ole Lund.

License
=======


Licensed under the Apache License, Version 2.0 (the "License");
you may not use this file except in compliance with the License.
You may obtain a copy of the License at

   http://www.apache.org/licenses/LICENSE-2.0

Unless required by applicable law or agreed to in writing, software
distributed under the License is distributed on an "AS IS" BASIS,
WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND, either express or implied.
See the License for the specific language governing permissions and
limitations under the License.
