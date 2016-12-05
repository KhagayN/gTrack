How to Graph Super Enhancers
============================

To illustrate gTrack's capability in exploring data sets such as ChIP-Seq data as well as reading BigWig data sets. This entire vignette attemps to replicate Zhang, X. et al (2016) which identified focally amplified super enhancers in epithelial cancers. 


.. sourcecode:: r
    

    ## The methods section of http://www.nature.com/ng/journal/v48/n2/full/ng.3470.html stated
    ## that GISTIC (used to find copy number variations) analyses were performed on available TCGA data
    ## TCGA data was found on the TCGA copy number portal which is created by the Broad Institute of
    ## MIT and Harvard.
    
    ## After finding version 3.0 of the SNP pipeline on 22 October 2014, clicking on the IGV
    ## session returned an XML document (http://portals.broadinstitute.org/tcga/gisticIgv/session.xml?analysisId=21&tissueId=548&type=.xml)
    ## which stored the web path to the *.seg.gz file. I downloaded that and found
    ## that it stored the log2 ratios (tumor coverage / normal coverage).
    
    ## wget http://www.broadinstitute.org/igvdata/tcga/tcgascape/141024_update/all_cancers.seg.gz
    ## wget http://www.broadinstitute.org/igvdata/tcga/tcgascape/141024_update/sample_info.txt
    
    ## gzip -d all_cancers.seg.gz


Using dt2gr (gUtils) and y.field (gTrack) and gr.colorfield and colormaps
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~


.. sourcecode:: r
    

    ##############################                         ##############################
    
    ##############################    Starting Analysis    ##############################
    
    ##############################                         ##############################
    
    library(gTrack) ## main sauce. 
    library(gUtils) ## for dt2gr 
    
    ## Load coverage data into data.table. Very fast, thanks data.table.
    ## seg_data <- fread('../../inst/extdata/files/all_cancers.seg')
    
    seg_data[Log2.Ratio <= 0, data_sign := "deletion"]


::

    ## Error in eval(expr, envir, enclos): object 'seg_data' not found


.. sourcecode:: r
    

    seg_data[Log2.Ratio > 0, data_sign := "insertion"]


::

    ## Error in eval(expr, envir, enclos): object 'seg_data' not found


.. sourcecode:: r
    

    ## Coerce into GRanges from data.table because gTrack operates on GRanges.
    seg_ranges <- dt2gr(seg_data)


::

    ## Warning in dt2gr(seg_data): coercing to GRanges via non-standard columns



::

    ## Warning in is(segs, "data.table"): restarting interrupted promise
    ## evaluation



::

    ## Error in is(segs, "data.table"): object 'seg_data' not found


.. sourcecode:: r
    

    ## first glimpse (gotta know what the data looks like). Probably should zoom in before even starting.



.. sourcecode:: r
    

    ## if you want the colors to be chosen automatically. 
    plot(gTrack(seg_ranges, y.field = 'Log2.Ratio', gr.colorfield = 'data_sign'))


::

    ## Error in listify(data, GRanges): object 'seg_ranges' not found




.. sourcecode:: r
    

    ## if you want to manually set the colors. Better because red/blue can be chosen instead of some random colors.
    plot(gTrack(seg_ranges, y.field = 'Log2.Ratio', colormaps = list('data_sign' = c(insertion = "blue", deletion = "red"))))


::

    ## Error in listify(data, GRanges): object 'seg_ranges' not found




.. sourcecode:: r
    

    ## Subset to MYC enhancer amplification regions.
    seg_data_chrom8 <- seg_data[ Chromosome == 8]


::

    ## Error in eval(expr, envir, enclos): object 'seg_data' not found


.. sourcecode:: r
    

    ## coerce into GRanges from data.table because gTrack operates on GRanges.
    seg_ranges_chrom8 <- dt2gr(seg_data_chrom8)


::

    ## Warning in dt2gr(seg_data_chrom8): coercing to GRanges via non-standard
    ## columns



::

    ## Warning in is(segs, "data.table"): restarting interrupted promise
    ## evaluation



::

    ## Error in is(segs, "data.table"): object 'seg_data_chrom8' not found




.. sourcecode:: r
    

    ## if you want to manually set the colors. Better because red/blue can be chosen instead of some random colors. 
    plot(gTrack(seg_ranges_chrom8, y.field = 'Log2.Ratio', colormaps = list('data_sign' = c(insertion = "blue", deletion = "red"))), win = seg_ranges_chrom8)


::

    ## Error in listify(data, GRanges): object 'seg_ranges_chrom8' not found



Using parse.gr
~~~~~~~~~~~~~~


.. sourcecode:: r
    

    ##############################                         ##############################
    
    ##############################    Plot MYC Enhancers   ##############################
    
    ##############################                         ##############################
    
    ## first MYC(myc) (s)uper-(e)nhancer.
    myc_se <- parse.gr(c('8:129543949-129554294'))
    ## zoom into that region to view CNA.
    win <- myc_se
    plot(gTrack(seg_ranges_chrom8, y.field = 'Log2.Ratio', colormaps = list('data_sign' = c(insertion = "blue", deletion = "red"))), win)


::

    ## Error in listify(data, GRanges): object 'seg_ranges_chrom8' not found


.. sourcecode:: r
    

    ## second MYC super-enhancer
    myc_se <- parse.gr(c('8:129166547-129190290'))
    win <- myc_se
    plot(gTrack(seg_ranges_chrom8, y.field = 'Log2.Ratio', colormaps = list('data_sign' = c(insertion = "blue", deletion = "red"))), win)


::

    ## Error in listify(data, GRanges): object 'seg_ranges_chrom8' not found


.. sourcecode:: r
    

    ## it looks like both regions have focal insertions and deletions. 
    plot(gTrack(seg_ranges_chrom8, colormaps = list('data_sign' = c(insertion = "blue", deletion = "red"))), win = seg_ranges_chrom8+10e6)


::

    ## Error in listify(data, GRanges): object 'seg_ranges_chrom8' not found




.. sourcecode:: r
    

    ##############################                         ##############################
    
    ##############################    Setting Thresholds   ##############################
    
    ##############################                         ##############################
    
    ## max width is 50MB to remove very broad copy number changes.
    ## min width is 20KB to exclude artifacts.
    
    seg_data_chrom8 <- seg_data_chrom8[End.bp - Start.bp <= 30e3]


::

    ## Error in eval(expr, envir, enclos): object 'seg_data_chrom8' not found


.. sourcecode:: r
    

    seg_ranges_chrom8 <- dt2gr(seg_data_chrom8)


::

    ## Warning in dt2gr(seg_data_chrom8): coercing to GRanges via non-standard
    ## columns



::

    ## Warning in is(segs, "data.table"): restarting interrupted promise
    ## evaluation



::

    ## Error in is(segs, "data.table"): object 'seg_data_chrom8' not found


.. sourcecode:: r
    

    plot(gTrack(seg_ranges_chrom8, colormaps = list('data_sign' = c(insertion = "blue", deletion = "red"))), win = seg_ranges_chrom8+10e6)


::

    ## Error in listify(data, GRanges): object 'seg_ranges_chrom8' not found




.. sourcecode:: r
    

    ## explore data set for determining threshold for log2 ratio.
    
    ##############################                         ##############################
    
    ##############################       Random Fact       ##############################
    
    ##############################                         ##############################
    
    ## There are more insertions than deletions.
    sorted_ratios <- sort(seg_data_chrom8$'Log2.Ratio')


::

    ## Error in sort(seg_data_chrom8$Log2.Ratio): object 'seg_data_chrom8' not found


.. sourcecode:: r
    

    length(sorted_ratios) ## 70K


::

    ## Error in eval(expr, envir, enclos): object 'sorted_ratios' not found


.. sourcecode:: r
    

    #### -1 and 2
    seg_data_chrom8_2 <- seg_data_chrom8[Log2.Ratio >= -1 & Log2.Ratio <= 2]


::

    ## Error in eval(expr, envir, enclos): object 'seg_data_chrom8' not found


.. sourcecode:: r
    

    seg_ranges_chrom8_2 <- dt2gr(seg_data_chrom8_2)


::

    ## Warning in dt2gr(seg_data_chrom8_2): coercing to GRanges via non-standard
    ## columns



::

    ## Warning in is(segs, "data.table"): restarting interrupted promise
    ## evaluation



::

    ## Error in is(segs, "data.table"): object 'seg_data_chrom8_2' not found


.. sourcecode:: r
    

    plot(gTrack(seg_ranges_chrom8_2, colormaps = list('data_sign' = c(insertion = "blue", deletion = "red"))), win = seg_ranges_chrom8_2+10e6)


::

    ## Error in listify(data, GRanges): object 'seg_ranges_chrom8_2' not found


.. sourcecode:: r
    

    #############################                          ################################
                 # Not much of a change, will ignore setting thresholds for Log2.Ratio
    ############################                           ################################


Reading bigWig in gTrack
~~~~~~~~~~~~~~~~~~~~~~~~


.. sourcecode:: r
    

    ## bigWig downloaded from https://www.encodeproject.org/experiments/ENCSR000AUI/
    
    ## fold change.
    plot(gTrack('~/my_git_packages/super_enhancers/db/ENCFF038AQV.bigWig', color = 'green'), win = parse.gr('8:128635434-128941434'))


::

    ## Warning in gr.findoverlaps(query, subject, ...): seqlength mismatch .. no
    ## worries, just letting you know

    ## Warning in gr.findoverlaps(query, subject, ...): seqlength mismatch .. no
    ## worries, just letting you know



::

    ## Warning in gr.findoverlaps(gr, windows): seqlength mismatch .. no worries,
    ## just letting you know


.. figure:: figure/bigWig-1.png
    :alt: plot of chunk bigWig

    plot of chunk bigWig
.. sourcecode:: r
    

    ### store gencode genes.
    ge = track.gencode()


::

    ## Pulling gencode annotations from /gpfs/commons/groups/imielinski_lab/lib/R-3.3.0/gTrack/extdata/gencode.composite.collapsed.rds


.. sourcecode:: r
    

    ### Plot ENCODE, peak super-enhancer, and copy number data. 
    ### without super-enhancers.
    
    plot(c(gTrack('~/my_git_packages/super_enhancers/db/ENCFF038AQV.bigWig', color = 'green'), gTrack(seg_ranges_chrom8, colormaps = list('data_sign' = c(insertion = "blue", deletion = "red"))), ge), win = parse.gr('8:128635434-128941434'))


::

    ## Error in listify(data, GRanges): object 'seg_ranges_chrom8' not found


.. sourcecode:: r
    

    ### with super-enhancers. 
    plot(c(gTrack('~/my_git_packages/super_enhancers/db/ENCFF038AQV.bigWig', color = 'green', bar = TRUE), gTrack(seg_ranges_chrom8, colormaps = list('data_sign' = c(insertion = "blue", deletion = "red"))), ge), win = parse.gr('8:128735434-129641434'))


::

    ## Error in listify(data, GRanges): object 'seg_ranges_chrom8' not found


.. sourcecode:: r
    

    ### Split the copy number data into two objects - one for insertions & other for deletions.
    
    seg_data_chrom8_insertions <- seg_data_chrom8[data_sign == "insertion"]


::

    ## Error in eval(expr, envir, enclos): object 'seg_data_chrom8' not found


.. sourcecode:: r
    

    seg_data_chrom8_deletions <- seg_data_chrom8[data_sign == "deletion"]


::

    ## Error in eval(expr, envir, enclos): object 'seg_data_chrom8' not found


.. sourcecode:: r
    

    seg_ranges_chrom8_insertions <- dt2gr(seg_data_chrom8_insertions)


::

    ## Warning in dt2gr(seg_data_chrom8_insertions): coercing to GRanges via non-
    ## standard columns



::

    ## Warning in is(segs, "data.table"): restarting interrupted promise
    ## evaluation



::

    ## Error in is(segs, "data.table"): object 'seg_data_chrom8_insertions' not found


.. sourcecode:: r
    

    seg_ranges_chrom8_deletions <- dt2gr(seg_data_chrom8_deletions)


::

    ## Warning in dt2gr(seg_data_chrom8_deletions): coercing to GRanges via non-
    ## standard columns

    ## Warning in dt2gr(seg_data_chrom8_deletions): restarting interrupted promise
    ## evaluation



::

    ## Error in is(segs, "data.table"): object 'seg_data_chrom8_deletions' not found


.. sourcecode:: r
    

    ### with super-enhancers & gencode & ChIP-seq & insertions/deletions split.
    plot(c(gTrack('~/my_git_packages/super_enhancers/db/ENCFF038AQV.bigWig', color = 'green', bar = TRUE), gTrack(seg_ranges_chrom8_insertions, col = "blue"), gTrack(seg_ranges_chrom8_deletions, col = "red"), ge), win = parse.gr('8:128735434-129641434'))


::

    ## Error in listify(data, GRanges): object 'seg_ranges_chrom8_insertions' not found


.. sourcecode:: r
    

    plot(gTrack(seg_ranges_chrom8_insertions, y.field = "Log2.Ratio", col = "blue"), win = parse.gr('8:128735434-129641434'))


::

    ## Error in listify(data, GRanges): object 'seg_ranges_chrom8_insertions' not found




.. sourcecode:: r
    

    ### Filtering broad events
    seg_data_chrom8_deletions2 <- seg_data_chrom8_deletions[Log2.Ratio >= -0.6]


::

    ## Error in eval(expr, envir, enclos): object 'seg_data_chrom8_deletions' not found


.. sourcecode:: r
    

    seg_data_chrom8_insertions2 <- seg_data_chrom8_insertions[Log2.Ratio >= 0.6]


::

    ## Error in eval(expr, envir, enclos): object 'seg_data_chrom8_insertions' not found


.. sourcecode:: r
    

    seg_ranges_chrom8_insertions <- dt2gr(seg_data_chrom8_insertions)


::

    ## Warning in dt2gr(seg_data_chrom8_insertions): coercing to GRanges via non-
    ## standard columns



::

    ## Warning in is(segs, "data.table"): restarting interrupted promise
    ## evaluation



::

    ## Error in is(segs, "data.table"): object 'seg_data_chrom8_insertions' not found


.. sourcecode:: r
    

    seg_ranges_chrom8_deletions <- dt2gr(seg_data_chrom8_deletions)


::

    ## Warning in dt2gr(seg_data_chrom8_deletions): coercing to GRanges via non-
    ## standard columns

    ## Warning in dt2gr(seg_data_chrom8_deletions): restarting interrupted promise
    ## evaluation



::

    ## Error in is(segs, "data.table"): object 'seg_data_chrom8_deletions' not found


.. sourcecode:: r
    

    plot(c(gTrack('~/my_git_packages/super_enhancers/db/ENCFF038AQV.bigWig', color = 'green', bar = TRUE), gTrack(seg_ranges_chrom8_insertions, col = "blue"), gTrack(seg_ranges_chrom8_deletions, col = "red"), ge), win = parse.gr('8:128735434-129641434'))


::

    ## Error in listify(data, GRanges): object 'seg_ranges_chrom8_insertions' not found


.. sourcecode:: r
    

    ### Replicable pipeline
    
    ## Subset to MYC enhancer amplifications regions.
    seg_data_chrom8 <- seg_data[ Chromosome == 8]


::

    ## Error in eval(expr, envir, enclos): object 'seg_data' not found


.. sourcecode:: r
    

    ## coerce data.table into GRanges because gTrack operates on GRanges. 
    seg_ranges_chrom8 <- dt2gr(seg_data_chrom8)


::

    ## Warning in dt2gr(seg_data_chrom8): coercing to GRanges via non-standard
    ## columns

    ## Warning in dt2gr(seg_data_chrom8): restarting interrupted promise
    ## evaluation



::

    ## Error in is(segs, "data.table"): object 'seg_data_chrom8' not found


.. sourcecode:: r
    

    seg_data_chrom8 <- seg_data_chrom8[End.bp - Start.bp <= 10e6]


::

    ## Error in eval(expr, envir, enclos): object 'seg_data_chrom8' not found


.. sourcecode:: r
    

    seg_data_chrom8_deletions <- seg_data_chrom8[Log2.Ratio <= 0, data_sign := "deletion"]


::

    ## Error in eval(expr, envir, enclos): object 'seg_data_chrom8' not found


.. sourcecode:: r
    

    seg_data_chrom8_insertions <- seg_data_chrom8[Log2.Ratio > 0, data_sign := "insertion"]


::

    ## Error in eval(expr, envir, enclos): object 'seg_data_chrom8' not found


.. sourcecode:: r
    

    seg_data_chrom8_insertions <- seg_data_chrom8[data_sign == "insertion"]


::

    ## Error in eval(expr, envir, enclos): object 'seg_data_chrom8' not found


.. sourcecode:: r
    

    seg_data_chrom8_deletions <- seg_data_chrom8[data_sign == "deletion"]


::

    ## Error in eval(expr, envir, enclos): object 'seg_data_chrom8' not found


.. sourcecode:: r
    

    gray = 'gray20'
    gt.h3k36 = gTrack('~/DB/Roadmap/consolidated//E114-H3K36me3.pval.signal.bigwig', name = 'H3K36me3', bar = TRUE, col = gray)
    gt.h3k4 = gTrack('~/DB/Roadmap/consolidated//E114-H3K4me3.pval.signal.bigwig', name = 'H3K4me3', bar = TRUE, col = gray)
    gt.enh = gTrack('~/DB/Roadmap/consolidated//E114-H3K27ac.pval.signal.bigwig', name = 'H3K27Ac', bar = TRUE, col = gray)
    gt.open = gTrack('~/DB/Roadmap/consolidated//E114-DNase.pval.signal.bigwig', name = 'DNAase', bar = TRUE, col = gray)
    gt.rnapos = gTrack('~/DB/Roadmap/consolidated/E114.A549.norm.pos.bw', name = 'RNAseq+', bar = TRUE, col = gray)
    gt.rnaneg = gTrack('~/DB/Roadmap/consolidated/E114.A549.norm.neg.bw', name = 'RNAseq-', bar = TRUE, col = gray, y0 = 0, y1 = 1200)
    
    THRESH = 1
    seg_data_chrom8_deletions <- seg_data_chrom8_deletions[Log2.Ratio >= -THRESH]


::

    ## Error in eval(expr, envir, enclos): object 'seg_data_chrom8_deletions' not found


.. sourcecode:: r
    

    seg_data_chrom8_insertions <- seg_data_chrom8_insertions[Log2.Ratio >= THRESH]


::

    ## Error in eval(expr, envir, enclos): object 'seg_data_chrom8_insertions' not found


.. sourcecode:: r
    

    seg_ranges_chrom8_insertions <- dt2gr(seg_data_chrom8_insertions)


::

    ## Warning in dt2gr(seg_data_chrom8_insertions): coercing to GRanges via non-
    ## standard columns

    ## Warning in dt2gr(seg_data_chrom8_insertions): restarting interrupted
    ## promise evaluation



::

    ## Error in is(segs, "data.table"): object 'seg_data_chrom8_insertions' not found


.. sourcecode:: r
    

    seg_ranges_chrom8_deletions <- dt2gr(seg_data_chrom8_deletions)


::

    ## Warning in dt2gr(seg_data_chrom8_deletions): coercing to GRanges via non-
    ## standard columns

    ## Warning in dt2gr(seg_data_chrom8_deletions): restarting interrupted promise
    ## evaluation



::

    ## Error in is(segs, "data.table"): object 'seg_data_chrom8_deletions' not found


.. sourcecode:: r
    

    plot(c(gTrack('~/my_git_packages/super_enhancers/db/ENCFF038AQV.bigWig', color = 'green', bar = TRUE), gTrack(seg_ranges_chrom8_insertions, col = "blue"), gTrack(seg_ranges_chrom8_deletions, col = "red"), ge), win = parse.gr('8:128735434-129641434'))


::

    ## Error in listify(data, GRanges): object 'seg_ranges_chrom8_insertions' not found


.. sourcecode:: r
    

    acov = as(coverage(seg_ranges_chrom8_insertions), 'GRanges')


::

    ## Error in coverage(seg_ranges_chrom8_insertions): object 'seg_ranges_chrom8_insertions' not found


.. sourcecode:: r
    

    dcov = as(coverage(seg_ranges_chrom8_deletions), 'GRanges')


::

    ## Error in coverage(seg_ranges_chrom8_deletions): object 'seg_ranges_chrom8_deletions' not found


.. sourcecode:: r
    

    plot(c(gt.rnapos, gt.enh, gTrack('~/my_git_packages/super_enhancers/db/ENCFF038AQV.bigWig', color = 'green', bar = TRUE), gTrack(acov, 'score', bar = TRUE), gTrack(dcov, 'score', bar = TRUE),  gTrack(seg_ranges_chrom8_insertions, col = "blue"), gTrack(seg_ranges_chrom8_deletions, col = "red"), ge), win = parse.gr('8:128735434-129641434'))+1e6


::

    ## Error in listify(data, GRanges): object 'acov' not found




