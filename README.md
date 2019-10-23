# NanoR

![alt text](NanoR.png)

## NanoR: R package to analyze/visualize and compare ONT data

NanoR is available to be installed from source. Please have a look at [NanoR documentation](https://davidebolo1993.github.io/nanordoc/) for any installation or usage questions. Documentation is being updated (Old usage documentation is still available below).

[Source Code](https://github.com/davidebolo1993/NanoR/tree/master/NanoR)

[Documentation](https://davidebolo1993.github.io/nanordoc/)


## Citation

Are you using NanoR in your works? Please cite:

Davide Bolognini, Niccolò Bartalucci, Alessandra Mingrino, Alessandro Maria Vannucchi, Alberto Magi.
[NanoR: A user-friendly R package to analyze and compare nanopore sequencing data](https://journals.plos.org/plosone/article?id=10.1371/journal.pone.0216471).
PLoS One. 2019 May 9.

Davide Bolognini, Roberto Semeraro, Alberto Magi.
[Versatile Quality Control Methods for Nanopore Sequencing](https://journals.sagepub.com/doi/full/10.1177/1176934319863068).
Evol Bioinform Online. 2019 Jul 23.


### Usage (will be included in the new documentation shortly)


### MinION and GridION X5 basecalled .fast5 files, single/multi-read (M version)

```R


List<-NanoPrepareM(DataPass="/Path/To/PassedFast5Files",DataFail=NA,DataSkip=NA,Label="Exp", MultiRead=FALSE) # prepare data. To allow multi-read .fast5 files support simply switch MultiRead to TRUE

Table<-NanoTableM(NanoMList=List,DataOut="/Path/To/DataOut",Cores=6,GCC=FALSE) # extract metadata. To allow GC content computation, switch GCC to TRUE

NanoStatsM(NanoMList=List,NanoMTable=Table,DataOut="/Path/To/DataOut", KeepGGObj = FALSE) #plot statistics. To store table behind ggplot2-plots, switch KeepGGObj to TRUE

NanoFastqM(DataPass="/Path/To/PassedFast5Files",DataOut="/Path/To/DataOut",Label="Exp",Cores=6,FASTA=FALSE, Minquality=7, MultiRead=FALSE) # extract .fastq. To convert .fastq to .fasta as well, switch FASTA to TRUE; to extract .fastq only from .fast5 files with quality greater or equal than Minquality, increase the Minquality parameter; to allow support for multi-read .fast5 files, switch MultiRead to TRUE.


```

If working with folders containing passed, failed and skipped .fast5 files together, give this folder to the "DataPass" parameter of NanoPrepareM and NanoFastqM: NanoR will automatically filter out the low-quality sequences




###  MinION and GridION X5 sequencing summary and .fastq files (G version)

```R

List<-NanoPrepareG(DataSummary="/Path/To/DataSummary", DataFastq="/Path/To/DataFastq", Cores = 1, Label="Exp") #prepare data. Using multiple cores is only useful when dealing with multiple sequencing summary files (old behaviour of GridION X5)

Table<-NanoTableG(NanoGList=List,DataOut="/Path/To/DataOut",GCC=FALSE) #arrange metadata. To extract GC content from .fastq files, switch GCC to TRUE

NanoStatsG(NanoGList=List,NanoGTable=Table,DataOut="/Path/To/DataOut", KeepGGObj = FALSE) # plot statistics.  To store table behind ggplot2-plots, switch KeepGGObj to TRUE

NanoFastqG(DataSummary="/Path/To/DataSummary", DataFastq="/Path/To/DataFastq", DataOut="/Path/To/DataOut", Cores = 1, Label="Exp", FASTA=FALSE, Minquality = 7) #filter .fastq file on a minimum quality defined in Minquality. To filter .fastq files on higher quality, increase Minquality treshold; to convert .fastq to .fasta as well, switch FASTA to TRUE.Using multiple cores is only useful when dealing with multiple sequencing summary files (old behaviour of GridION X5)

```

Some of the plots generated by NanoR for MinION and GridION X5 data analysis are shown below:

**Reads number, basepairs number, reads length and reads quality for every 30 minutes of experimental run**

![alt text](Plots/RBLQ.png)

**Reads length vs reads quality**

![alt text](Plots/LvsQ.png)

**Channels and muxes activity**

![alt text](Plots/Activity.png)


In addition, NanoR plots the yield (cumulative reads and cumulative base pairs), the number and percentage of failed/passed reads and the GC content count histogram (if GCC has been computed).

### Compare MinION/GridION X5 data

```R

DataIn<-c("Path/To/AnalyzedFolder1/DataForComparison","Path/To/AnalyzedFolder2/DataForComparison","Path/To/AnalyzedFolder3/DataForComparison" #path to the NanoR-analyzed data

Labels<-c("Label1","Label2","Label3") #labels to identify the experiments

NanoCompare(DataIn=DataIn,DataOut="Path/To/DataOut",Labels=Labels) #compare

```

NanoCompare returns a Violins.pdf plot that compare reads number, base pairs number, reads length and reas quality every 10 hours of experimental run (maximum 80 hours of experimental run)
