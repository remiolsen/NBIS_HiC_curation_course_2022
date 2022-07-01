# NBIS_HiC_curation_course_2022

# Introduction

This will be an informal meeting focused on the manual curation of Hi-C scaffolded assemblies. It's a method that might seem more like an art than a science, and thus be inscrutable for us science-minded folks. My goal with this meeting is to hopefully give you some knowledge and confidence to get started with your own assembly curation projects.  

This meeting will be held online via zoom:
[...]


A preliminary agenda for this meeting is 

* Me giving a presentation on Hi-C scaffolding and assembly curation
* A practical part where the participants will use PretextView to curate a draft genome
* Lastly, a discussion / Q&A session

Requirements:

1. A copy of PretextView installed on your PC or laptop. It should be available for both Windows, Linux and MacOS: https://github.com/wtsi-hpag/PretextView/releases/tag/0.2.5

2. Uppmax SNIC account. Most work will be performed on your own computers but, there is some small amount of data you will need to have access to.

# Instructions

The workspace for this course can be found at:

WORKSPACE=/proj/snic2021-5-291/nobackup/NBIS_HiC_curation_course_2022

## PretextView

Download the pretext file (`$WORKSPACE/files/bCalAnn.annotated.pretext`) from Rackham, e.g. using `scp user@rackham2.uppmax.uu.se:/${WORKSPACE}/files/bCalAnn.annotated.pretext .` 
Similarly download the tpf file in `${WORKSPACE}/files/scaffolds_FINAL.fasta.tpf

Using PretextView your tasks are the following:

1. Open the pretext file
2. Order and orient contigs according to chromosomes as best you can, using the `Edit Mode` in PretextView
3. "Paint" in the the chromosome as scaffolds using the `Scaffold Edit Mode` in PretextView
4. Generate an AGP file and save it locally to your computer

A detailed user guide for using PretextView can be found here: https://gitlab.com/wtsi-grit/rapid-curation/-/blob/main/PretextView%20-%20Tutorial.pdf

An important tip to avoid some headaches is to not "cut" contigs, only select the whole contig then moving and/or inverting them. 
Also keep snap-mode on at all times.
Rapid curation does support these cutting operations, but the cuts and their co-ordinates will have to be amended to a tabulated file, tpf.
See the manual for more details: https://gitlab.com/wtsi-grit/rapid-curation/-/blob/main/RAPID%20CURATION%20TRAINING%20MANUAL.pdf

If you want contigs into others, you must edit the tpf file to indicate that you must either create a new gap or mark an 
existing gap as the insertion site. A helpful video on this is: https://www.youtube.com/watch?v=LWy6pwCQNDU

## Generate fasta file

On the cluster go to `$WORKSPACE/users` and create a folder with your name. Copy your local agp file and you tpf file to your folder.

When CWD in your folder, run the following command to generate a final tpf file and chromosome description file:
```
singularity exec ../../software/rapidcuration_extras_20220629.sif python ../../software/rapid-curation/rapid_pretext2tpf_XL.py your.tpf your.agp  > report.txt
```

You can check the (stdOut) report.txt file to spot errors in the, usually from mismatches between the input tpf file and agp files. Look out for `ERROR $N tpf components not used in output file!!` or mismatches in the input and output genome sizes, eg.:
```
Input genome size:		1,007,374,986bp
Output genome size:		891,752,494bp		(primary + haps)
```

When you are satisfied, the final command to generate the fasta file is

```
singularity exec ../../software/rapidcuration_extras_20220629.sif perl ../../software/rapid-curation/rapid_join.pl -fa ../../files/scaffolds_FINAL.fasta -tpf rapid_prtxt_XL.tpf -csv chrs.csv -out scaffolds_curated
```

