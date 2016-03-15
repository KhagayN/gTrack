
Tutorial 
--------

.. code-block:: bash
   
   # make sure to load gTrack
   library(gTrack)

.. code-block:: bash 

   # load data from TCGA 
   tcgaData <- read.delim("inst/extdata/BEAUX_p_TCGA_b109_SNP_2N_GenomeWideSNP_6_A01_772082.hg18.seg")

   # convert data.frame to GRanges object
   tcgagr <- makeGRangesFromDataFrame(tcgaData)
   
   # wrap gTrack around TCGA GRanges object 
   tcgagt <- gTrack(tcgagr)
   
   # plot gTrack object 
   plot(tcgagt)

.. figure:: figures/tcgatoomanywindows.png
   :alt:
   :scale: 75%

.. code-block:: bash
   
   # Changed default number of windows (from entire genome to the first five windows). 
   # Windows argument requires a subset of a GRanges Object. Check documentation for more details. 
   plot(tcgagt , window = tcgagr[1:5])   

.. figure:: figures/tcga5windows.png 
   :alt:
   :scale: 75%  

.. code-block:: bash
   
   # use amplifications/deletions as y-values 
   tcgagt <- gTrack(tcgagrr , y.field="Segment_Mean")
   plot(tcgagt , windows = tcgagrr[1:5] , col = "red")   
 
.. figure:: figures/deletions.png
   :alt:
   :scale: 75% 