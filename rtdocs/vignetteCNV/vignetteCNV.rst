Vignette Using CNV Data
=======================

**Operations on CNV data of one sample** 


.. sourcecode:: r
    

    library(gUtils)
    library(gTrack)
    
    setwd("~/Documents/gTrack/gTrack/")
    ##load data from TCGA
    
    tcgaData <- read.delim("inst/extdata/BEAUX_p_TCGA_b109_SNP_2N_GenomeWideSNP_6_A01_772082.hg18.seg.txt")
    
    ## convert data.frame to GRanges object
    
    tcgagr <- GRanges(tcgaData)
    
    # wrap gTrack around TCGA GRanges object
    
    tcgagt <- gTrack(tcgagr)



.. sourcecode:: r
    

    # plot gTrack object
    
    plot(tcgagt)

.. figure:: figure/plot-entireData-1.png
    :alt: plot of chunk plot-entireData

    plot of chunk plot-entireData


.. sourcecode:: r
    

    # Changed default number of windows (from entire genome to the first five windows).
    # Windows argument requires a subset of a GRanges Object. Check documentation for more details.
    plot(tcgagt , window = tcgagr[1:5])

.. figure:: figure/plot-subsetofData-1.png
    :alt: plot of chunk plot-subsetofData

    plot of chunk plot-subsetofData


.. sourcecode:: r
    

    # use amplifications/deletions as y-values
    tcgagt <- gTrack(tcgagr , y.field="Segment_Mean")
    plot(tcgagt , windows = tcgagr[1:5] , col = "red")

.. figure:: figure/plot-amplifcations-1.png
    :alt: plot of chunk plot-amplifcations

    plot of chunk plot-amplifcations


.. sourcecode:: r
    

    # add a second sample to the graph
    # create gTrack object for sample
    
    setwd("~/Documents/gTrack/gTrack/")
    tcgaData2 <- read.delim("inst/extdata/BEAUX_p_TCGA_b109_SNP_2N_GenomeWideSNP_6_A01_772082.hg19.seg.txt")
    tcgagr2 <- GRanges(tcgaData2)
    tcgagt2 <- gTrack(tcgagr2 , y.field="Segment_Mean")



.. sourcecode:: r
    

    # plot the two samples
    plot(c(tcgagt2 , tcgagt), windows = tcgagr2[1:5] , col = "red")

.. figure:: figure/plot-twoSamples-1.png
    :alt: plot of chunk plot-twoSamples

    plot of chunk plot-twoSamples


.. sourcecode:: r
    

    # physically separate gaps between tracks
    plot(c(tcgagt2 , tcgagt), windows = tcgagr2[1:5] , col = "red" , ygap = 5)


::

    ## Warning in ywid * grl.segs$ywid: longer object length is not a multiple of
    ## shorter object length

    ## Warning in ywid * grl.segs$ywid: longer object length is not a multiple of
    ## shorter object length


.. figure:: figure/plot-ygap-1.png
    :alt: plot of chunk plot-ygap

    plot of chunk plot-ygap


.. sourcecode:: r
    

    # study of the CNVs in breast cancer
    fn = list.files("~/Documents/gTrack/gTrack/inst/extdata/Level_3/")
    
    # create data.tables for each patient but, combine them into one HUGE data.table using rbindlist
    dt = rbindlist(lapply(fn , function(x) fread(x , colClasses = "character")[ , file:=x]))


::

    ## Error in fread(x, colClasses = "character"): File 'BEAUX_p_TCGA_b109_SNP_2N_GenomeWideSNP_6_A01_772082.hg19.seg.txt' does not exist. Include one or more spaces to consider the input a system command.


.. sourcecode:: r
    

    # certain arguments (window) of gTrack require numeric vectors. Thus, "character" vectors need
    # to be converted into "numeric" vectors.
    
    dt$Start = type.convert(dt$Start)


::

    ## Error in type.convert(dt$Start): the first argument must be of mode character


.. sourcecode:: r
    

    dt$End = type.convert(dt$End)


::

    ## Error in type.convert(dt$End): the first argument must be of mode character


.. sourcecode:: r
    

    # because we are graphing segment mean, that column also needs to be "numeric"
    dt$Segment_Mean = type.convert(dt$Segment_Mean)


::

    ## Error in type.convert(dt$Segment_Mean): the first argument must be of mode character


.. sourcecode:: r
    

    # convert data.table into GRanges object
    dtgr = GRanges(dt)


::

    ## Error in (function (classes, fdef, mtable) : unable to find an inherited method for function 'Rle' for signature '"data.table", "missing"'


.. sourcecode:: r
    

    # wrap a gTrack object around it and plot
    dtgt <- gTrack(dtgr , y.field = "Segment_Mean")


::

    ## Error in listify(data, GRanges): object 'dtgr' not found




.. sourcecode:: r
    

    plot(dtgt , window = dtgr[1:5])


::

    ## Error in plot(dtgt, window = dtgr[1:5]): error in evaluating the argument 'x' in selecting a method for function 'plot': Error: object 'dtgt' not found




.. sourcecode:: r
    

    # show amplifications only (use gUtils operators!)
    dtgr = dtgr %Q% (Segment_Mean > 0)


::

    ## Error in dtgr %Q% (Segment_Mean > 0): error in evaluating the argument 'x' in selecting a method for function '%Q%': Error: object 'dtgr' not found


.. sourcecode:: r
    

    dtgt <- gTrack(dtgr , y.field = "Segment_Mean")


::

    ## Error in listify(data, GRanges): object 'dtgr' not found




.. sourcecode:: r
    

    plot(dtgt , window = dtgr[1:5])


::

    ## Error in plot(dtgt, window = dtgr[1:5]): error in evaluating the argument 'x' in selecting a method for function 'plot': Error: object 'dtgt' not found




.. sourcecode:: r
    

    # show deletions only (again, use gUtils operators!)
    
    # recreate the original GRanges object
    dtgr = GRanges(dt)


::

    ## Error in (function (classes, fdef, mtable) : unable to find an inherited method for function 'Rle' for signature '"data.table", "missing"'


.. sourcecode:: r
    

    # subset properly
    dtgr = dtgr %Q% (Segment_Mean < 0)


::

    ## Error in dtgr %Q% (Segment_Mean < 0): error in evaluating the argument 'x' in selecting a method for function '%Q%': Error: object 'dtgr' not found


.. sourcecode:: r
    

    dtgt <- gTrack(dtgr , y.field = "Segment_Mean")


::

    ## Error in listify(data, GRanges): object 'dtgr' not found




.. sourcecode:: r
    

    plot(dtgt , window = dtgr[1:5])


::

    ## Error in plot(dtgt, window = dtgr[1:5]): error in evaluating the argument 'x' in selecting a method for function 'plot': Error: object 'dtgt' not found


