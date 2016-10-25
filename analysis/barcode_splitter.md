When a sequencing run is done at Princeton, the data we get back must be split out by indexed Pool before it can be put into process_radtags to separate samples by barcode.

Barcode splitter has to be run on pass 1 and pass 2 of each lane

From the previous step, [Prep the analysis space](./prep_seq_space.md), you already have a bcsplit directory in your working directory containing lane1 and lane2 directories.  Lanes 1 and 2 have to be run in separate directories otherwise their outputs will overwrite each other.  It saves time to run these lanes simultaneously.

The command line of barcode splitter consists of:
  - barcode_splitter.py - the program
  - -bcfile ../../logs/03seq-index - the location and name of the index file created above
  - -idxread 2 - the number of the read that contains the index
  - - suffix .fastq.gz - the suffix of the input files

These options are followed by the location and name of lane # read 1, a space and then the location and name of lane # read 2

Run barcode splitter on 1st lane with nohup  - notice the only difference between the filenames is at the very end.
`nohup barcode_splitter.py --bcfile ../../logs/03seq-index --idxread 2 --suffix .fastq.gz /local/shared/pinsky_lab/sequencing/hiseq_2014_08_07_SEQ03/clownfish-ddradseq-seq03-for-222-cycles-ha1wgadxx_1_read_1_passed_filter.fastq.gz /local/shared/pinsky_lab/sequencing/hiseq_2014_08_07_SEQ03/clownfish-ddradseq-seq03-for-222-cycles-ha1wgadxx_1_read_2_index_read_passed_filter.fastq.gz &`

Run barcode splitter on 2nd lane with nohup
`nohup barcode_splitter.py --bcfile ../../logs/03seq-index --idxread 2 --suffix .fastq.gz /local/shared/pinsky_lab/sequencing/hiseq_2014_08_07_SEQ03/clownfish-ddradseq-seq03-for-222-cycles-ha1wgadxx_2_read_1_passed_filter.fastq.gz /local/shared/pinsky_lab/sequencing/hiseq_2014_08_07_SEQ03/clownfish-ddradseq-seq03-for-222-cycles-ha1wgadxx_2_read_2_index_read_passed_filter.fastq.gz`

The nohup.out should be saved as a .tsv in the logs directory and can be used to gather useful information about the number of reads that were assigned to each pool.

The output of barcode splitter is a log file, the names of all of the pools split into read-1 and read-2 fastq.gz files and unnamed reads that the program was unable to assign to an index.  Read-2 and unnamed read files can be deleted.

Cat the 2 lanes into one file for process radtags
`cat ./lane2/P012-read-1.fastq.gz ./lane1/P012-read-1.fastq.gz > P012.fastq.gz`

[Return to analysis protocol](./hiseq_ddocent.md)