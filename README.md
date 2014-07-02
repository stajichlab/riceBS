riceBS
======

Rice Bisulfide sequencing support


I.  Prepare your files for Bisulfite Bioinformatic processing

  1. Start with making a directory called "reads" and download fastq files into that directory.


  2. create a file with old_names new_names for fastq

  ex:

%] cat Bio20_2014_Sprint_run3
flowcell246_lane1_pair1_GGCTAC.fastq	A123_1.p1.fq
flowcell246_lane1_pair2_GGCTAC.fastq    A123_1.p2.fq 
flowcell246_lane1_pair1_ATGTCA.fastq    NB_1.p1.fq
flowcell246_lane1_pair2_ATGTCA.fastq    NB_1.p2.fq


  3. rename (really these are symlinked files) the files with informative names.
	Use rename_fq.pl with 2 arguments
	a. the directory of the files
        b. the file made in step 1.

  ex:

%]rename_fq.pl ~/bigdata/Bio20_2014Spring_run3/BS_PCR Bio20_2014_Sprint_run3 
%]ls
A119_1.p1.fq@  A123_2.p1.fq@           README                                 flowcell246_lane1_pair2_ACTTGA.fastq*
A119_1.p2.fq@  A123_2.p2.fq@           flowcell246_lane1_pair1_ACTTGA.fastq*  flowcell246_lane1_pair2_AGTCAA.fastq*
A119_2.p1.fq@  BSmap.pl*               flowcell246_lane1_pair1_AGTCAA.fastq*  flowcell246_lane1_pair2_ATGTCA.fastq*  

II. Run Bismark
 
 1. run make_runBismark.sh and give the following arguments. Collect the output as a shell script to run on the queue.
      a. genome DIR  ( /rhome/jinghuas/bigdata/bisulfide_genome/mping ) 
      b. FQ1   ( A119_1.p2.fq )
      c. FQ2   ( A119_1.p2.fq )
      d. BASE  ( A119_1 )
  
      e. is the file in which the shell script is stored ( > A119.1_runBismark.sh )

  ex: one at a time: 
make_runBismark.sh /rhome/jinghuas/bigdata/bisulfide_genome/mping A119_1.p1.fq A119_1.p2.fq A119_1 > A119.1_runBismark.sh
    
  ex: all at once

for i in NB_1 NB_2 A119_1 A119_2 A119_3 A123_1 A123_2 ; do make_runBismark.sh /rhome/jinghuas/bigdata/bisulfide_genome/mping ${i}.p1.fq ${i}.p2.fq ${i} output > ${i}.runBismark.sh ; done


  2. submit to the queue.

    ex:

   qsub A119_1.runBismark.sh

III. Post process Bismark output for readability and files for Gbrowse
  1. cd output
  2. run extractor and process by running make_extract_convert.sh with the sample name. You will need to collect the ouput for use as a shell script.
     a. sample_name ( same as base ) ( A119_1 )
 

 ex: one at a time
  make_extract_convert.sh A119_1 > A119_1.extract_process.sh

 ex: all at once
  for i in NB_1 NB_2 A119_1 A119_2 A119_3 A123_1 A123_2  ; do make_extract_convert.sh $i > ${i}.extract_process.sh ; done

  3. submit each to the queue

    ex.

    qsub A119_1.extract_process.sh
  
IV. Combine Data
  1. make sure you are in output, same as in step III.
  2. run combine script
     a. path of the directory with the split data ( split_methyl ) 
     b. a file name to write the output to ( combine.txt )
    
     ex.

     combine_split.pl split_methyl/  > combine.txt

V. copy data to your own computer or html dir
   --- own computer ---
   1. connect with FUGU
   2. copy what you need.

   ---- html dir ---
   1. cp onefile ~/.html/.
   2. cp -r directory_name ~/.html/.
