# Lazega Law Firm

https://raw.githubusercontent.com/bavla/Rnet/master/net/mRel/Lazega/Lazega-Law-Firm.paj

The data are available for example at
- https://manliodedomenico.com/data.php
- https://www.stats.ox.ac.uk/~snijders/siena/Lazega_lawyers_data.htm

	
The multirelational social network on 71 nodes consists of 3 relations (Co-work (1104), Friendship (575), Advice (892)) between partners and associates of a corporate law partnership. Node properties: seniority (years with the firm),
status (1=partner; 2=associate),
gender (1=man; 2=woman),
office (1=Boston; 2=Hartford; 3=Providence),
age,
practice (1=litigation; 2=corporate),
law school (1: harvard, yale; 2: ucon; 3: other).

References
1. Emmanuel Lazega - "The Collegial Phenomenon: The Social Mechanisms of Cooperation Among Peers in a Corporate Law Partnership". Oxford University Press (2001)
2. Tom A.B. Snijders, Philippa E. Pattison, Garry L. Robins, and Mark S. Handcock - "New specifications for exponential random graph models". Sociological Methodology (2006), 99-153.


## Conversion to Pajek project file

We converted into Pajek format the data from the first source.
```
> # Lazega-Law-Firm
>
> wdir <- "D:/vlado/data/multiRel/Lazega-Law-Firm_Multiplex_Social/Dataset"
> setwd(wdir)
> source("https://raw.githubusercontent.com/bavla/Rnet/master/R/Pajek.R")
> R <- read.table("Lazega-Law-Firm_layers.txt",stringsAsFactors=FALSE,header=TRUE)
> R
  layerID layerLabel
1       1     advice
2       2 friendship
3       3    co-work
> L <- read.table("Lazega-Law-Firm_multiplex.edges",header=FALSE)
> dim(L)
[1] 2571    4
> head(L)
  V1 V2 V3 V4
1  1  1  2  1
2  1  1 17  1
3  1  1 20  1
4  1  2  1  1
5  1  2  6  1
6  1  2 17  1
> N <- read.table("Lazega-Law-Firm_nodes.txt",stringsAsFactors=FALSE,header=TRUE)
> dim(N)
[1] 71  8
> head(N)
  nodeID nodeStatus nodeGender nodeOffice nodeSeniority nodeAge nodePractice nodeLawSchool
1      1          1          1          1            31      64            1             1
2      2          1          1          1            32      62            2             1
3      3          1          1          2            13      67            1             1
4      4          1          1          1            31      59            2             3
5      5          1          1          2            31      59            1             2
6      6          1          1          2            29      55            1             1
>
> net <- file("Lazega.net","w")
> n <- nrow(N)
> NR <- paste("L",1:n,sep="")
> cat("% Lazega Law Firm",date(),"\n*vertices",n,"\n",file=net)
> for(i in 1:n) cat(i,' "',NR[i],'"\n',sep="",file=net)
> for(i in 1:nrow(R)) cat("*arcs :",i,' "',R$layerLabel[i],'"\n',sep="",file=net)
> cat("*arcs\n",file=net)
> for(i in 1:nrow(L)) cat(L$V1[i],": ",L$V2[i]," ",L$V3[i]," ",L$V4[i],"\n",sep="",file=net)
> close(net)
> vector2clu(N$nodeStatus,Clu="status.clu")
> vector2clu(N$nodeGender,Clu="gender.clu")
> vector2clu(N$nodeOffice,Clu="office.clu")
> vector2vec(N$nodeSeniority,Vec="seniority.vec")
> vector2vec(N$nodeAge,Vec="age.vec")
> vector2clu(N$nodePractice,Clu="practice.clu")
> vector2clu(N$nodeLawSchool,Clu="lawSchool.clu")
```
To produce the Pajek project file we read all the created files into Pajek and save them as a project file. Finally we add some metadata using a text editor.

