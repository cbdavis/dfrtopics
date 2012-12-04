# dfr-analysis

Bits and pieces to help analyze the data available from JSTOR's Data for Research (DfR) service. Skews to my needs as a literary scholar and digital-humanist-in-progress. Skews to my amateurishness as a programmer. Very much in-progress, hacked together, catch-as-catch-can, I am not an expert, I am not a lawyer, etc., etc., etc. Use and share freely, at your own risk. 

## combine_citations

A simple shell script to combine multiple `citations.CSV` metadata files
from jstor and get rid of any duplicate lines. Usage:

    combine_citations citations1.CSV citations2.CSV ... > citations_all.csv

## count2txt

I like to play around with the topic modeling tool [MALLET](http://mallet.cs.umass.edu). I have yet to figure out how to get `mallet` at the command line to accept documents in the form of wordcounts, which is how jstor DfR provides them. `mallet` wants to count words itself. So this script lets you! Fun and easy! takes `wordcount*.CSV` files from jstor and "inflates" them into bags of words. By default, will make you one ginormous file, with each "bag" on a line. This is suitable for input to the `mallet import-file` command. Usage:

    count2txt wordcounts*.CSV > wordbags.txt

I often use this on thousands of `wordcounts*.CSV` files at once, in which case shell expansion at the command line isn't so great. So actually a more typical usage is:

    cd wordcounts; ls -1 | xargs ~/dfr-analysis/count2txt > wordbags.txt

Then you can do:

    mallet import-file --input wordbags.txt --label 0 --keep-sequence --output dfr.mallet
    mallet train-topics --input dfr.mallet ....

## metadata.R

Functions for manipulating the metadata with R.

## subset_txt

Perl script to output a subset of the bag-of-words collection produced by count2txt.

## topics.R

Basic R script for loading the topic model, getting basic information from it, and making time-series plots of various kinds. In progress.

## sort_doc_topics

Perl script to convert the result of

    mallet train-topics --output-doc-topics filename.tsv

into a table with columns ordered by topic number. This can then be fed into the ``read.sorted.doc.topics`` method in ``topics.R``.

## More to come!

### _mallet

Experimental zsh completion for mallet. Totally incomplete and broken, because have you *read* the `zshcompsys` manpage????