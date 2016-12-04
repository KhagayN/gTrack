How to Graph Super Enhancers
============================

To illustrate gTrack's capability in exploring data sets such as
ChiP-Seq data



.. sourcecode:: r
    

    ## The methods section of
    http://www.nature.com/ng/journal/v48/n2/full/ng.3470.html stated that
    GISTIC analyses were performed on the TCGA data available on the TCGA
    copy number portal which is created by the Broad Institute of MIT and
    Harvard.
    
    ## After finding version 3.0 of the SNP pipeline on 22 October 2014,
    clicking on the IGV session and an XML document
    (http://portals.broadinstitute.org/tcga/gisticIgv/session.xml?analysisId=21&tissueId=548&type=.xml)
    was returned which stored the web path to the *.seg.gz file. I
    downloaded that and found that it stored the log2 ratios (tumor
    coverage / normal coverage).
    
    wget http://www.broadinstitute.org/igvdata/tcga/tcgascape/141024_update/all_cancers.seg.gz
    wget http://www.broadinstitute.org/igvdata/tcga/tcgascape/141024_update/sample_info.txt
    
    gzip -d all_cancers.seg.gz
    
    ## command grouping to view some content of the files. 
    head -1 all_cancers.seg & tail all_cancers.seg


::

    ## Error: <text>:2:6: unexpected '/'
    ## 1: ## The methods section of
    ## 2: http:/
    ##         ^


