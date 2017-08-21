Shallow shotgun sequencing tutorial
=======

## Installation
These installation instructions are streamlined for Linux and macOS systems. The tool SHOGUN is installable on Windows with a few minor tweaks to this tutorial. This package requires anaconda, which is a system agnostic package and virtual environment manager. Follow the installation instructions for your system at <http://conda.pydata.org/miniconda.html>.

Once anaconda is installed, get the environment file:

```
wget https://raw.githubusercontent.com/knights-lab/SHOGUN/master/environment.yml
```

Then install the requirements into the environment 'shogun':
```
conda env create --file environment.yml
```

Next, you need the tools UTree and BURST on your path. These are contained in the lab SOP.

```
export PATH="/project/flatiron2/sop:$PATH"
```

Now activate the environment.

```
# OSX, Linux
source activate shogun
```

With the shogun environment activated, we can now run the following to check the SHOGUN version number. This is linked to the Github commit string.

```
shogun --version
shogun, version v0.0.1+293.g6531389
```

To verify your installation, make sure to run the test suite. For testing, we are currently using the built in python unittests. In order to run the test suite, change directory into the root folder of the repository. Then run:

```
cd /project/flatiron2/ben/projects/SHOGUN/SHOGUN
python -m unittest discover shogun
```

After the testing is complete, we can try running our analysis. To do so, run the following command:

```
shogun --log debug align --input ./shogun/tests/data/combined_seqs.fna --aligner utree --database /project/flatiron2/analysis_SHOGUN/data/references/rep82_dusted --output ~/scratch_shogun --function --level species --threads 1
```

### SHOGUN help for Command-Line
SHOGUN is a command line application. It is meant to be run with a single command. The helpful for the command is below.

```
Usage: shogun [OPTIONS] COMMAND [ARGS]...

  SHOGUN command-line interface

  --------------------------------------

Options:
  --debug / --no-debug
  --version             Show the version and exit.
  --help                Show this message and exit.

Commands:
  align         Run the SHOGUN aligner
  function      Run the SHOGUN functional algorithm.
  redistribute  Run the SHOGUN redistribution algorithm.
  ```

#### align
  The command ```align``` runs the standard pipeline, including alignment, redistribution and functional annotational. If you wish, you can run the alignment with all backends ```<Bowtie2, BURSt, UTree>```, followed by redistribution at all taxonomic, and functional alignment at levels genus through strain.

```
Usage: shogun align [OPTIONS]

  Run the SHOGUN aligner

Options:
  -a, --aligner [all|bowtie2|embalmer|utree]
                                  The aligner to use.  [default: embalmer]
  -i, --input PATH                The file containing the combined seqs.
                                  [required]
  -d, --database PATH             The database file.
  -o, --output PATH               The output folder directory  [default:
                                  /mnt/c/Users/bhill/results-170801]
  -l, --level [kingdom|phylum|class|order|family|genus|species|strain|all|off]
                                  The level to collapse taxatables and
                                  functions too (not required, can specify
                                  off).
  --function / --no-function      Run functional algorithms.
  -t, --threads INTEGER           Number of threads to use.
  --help                          Show this message and exit.
```

#### function
This command assigns function at a certain taxonomic level. Lower level KEGG IDs are assigned to higher level KEGG IDs through plurality voting. Note that plasmids are not included the KEGG ID annotation.

```
Usage: shogun function [OPTIONS]

  Run the SHOGUN functional algorithm.

Options:
  -i, --input PATH                The the taxatable.  [required]
  -d, --database PATH             The path to the folder containing the
                                  function database.  [required]
  -o, --output PATH               The output file  [default:
                                  /mnt/c/Users/bhill/results-170801]
  -l, --level [family|genus|species|strain]
                                  The level to collapse to.
  --help                          Show this message and exit.
```


#### redistribute
  This command redistributes the reads at a certain taxonomic level. This assumes that you have a BIOM txt file output from SHOGUN align, or even a summarized table from redistribute at a lower level.

  ```
  Usage: shogun redistribute [OPTIONS]

  Run the SHOGUN redistribution algorithm.

Options:
  -i, --input PATH                The taxatable.  [required]
  -d, --database PATH             The path to the database.  [required]
  -l, --level [kingdom|phylum|class|order|family|genus|species|strain|all]
                                  The level to collapse to.
  -o, --output PATH               The output file  [default:
                                  /mnt/c/Users/bhill/taxatable-170801.txt]
  --help                          Show this message and exit.
  ```