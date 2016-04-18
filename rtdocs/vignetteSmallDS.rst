Vignette Using a Small Data Set
===============================

**Overlapping Segments**

**gr.tile(gr , w)**

**tile ranges across GRanges**


.. sourcecode:: r
    

    ## Given a GRanges object, separate the interval by "w".
    ## In this example, the output will be 20 equal sized tiles
    
    print(GRanges(1 , IRanges(1,100)))


::

    ## GRanges object with 1 range and 0 metadata columns:
    ##       seqnames    ranges strand
    ##          <Rle> <IRanges>  <Rle>
    ##   [1]        1  [1, 100]      *
    ##   -------
    ##   seqinfo: 1 sequence from an unspecified genome; no seqlengths


.. sourcecode:: r
    

    gr <- gr.tile(GRanges(1, IRanges(1,100)), w=5)



.. sourcecode:: r
    

    ## Plot how the tiles would like
    plot(gTrack(gr))

.. figure:: figure/rst-plot-1.png
    :alt: plot of chunk rst-plot

    plot of chunk rst-plot

**Overlapping Tiles - gTrack(gr + n)**

**indicate the degree of overlap**


.. sourcecode:: r
    

    plot(gTrack(gr+10))

.. figure:: figure/rst-plot2-1.png
    :alt: plot of chunk rst-plot2

    plot of chunk rst-plot2

**stack.gap**

**vector or scaler numeric specifiying x gap between stacking non-numeric GRanges or GRangesLists items in track(s).**


.. sourcecode:: r
    

    gr <- GRanges(seqnames = Rle(c("chr1" , "chr2" , "chr1" , "chr3") ,
      c(1,3,2,4)), ranges = IRanges(c(1,3,5,7,9,11,13,15,17,19) , end =
        c(2,4,6,8,10,12,14,16,18,20), names = head(letters,10)), GC=seq(1,10,length=10), name=seq(5,10,length=10))
    print(gr)


::

    ## GRanges object with 10 ranges and 2 metadata columns:
    ##     seqnames    ranges strand |        GC             name
    ##        <Rle> <IRanges>  <Rle> | <numeric>        <numeric>
    ##   a     chr1  [ 1,  2]      * |         1                5
    ##   b     chr2  [ 3,  4]      * |         2 5.55555555555556
    ##   c     chr2  [ 5,  6]      * |         3 6.11111111111111
    ##   d     chr2  [ 7,  8]      * |         4 6.66666666666667
    ##   e     chr1  [ 9, 10]      * |         5 7.22222222222222
    ##   f     chr1  [11, 12]      * |         6 7.77777777777778
    ##   g     chr3  [13, 14]      * |         7 8.33333333333333
    ##   h     chr3  [15, 16]      * |         8 8.88888888888889
    ##   i     chr3  [17, 18]      * |         9 9.44444444444444
    ##   j     chr3  [19, 20]      * |        10               10
    ##   -------
    ##   seqinfo: 3 sequences from an unspecified genome; no seqlengths




.. sourcecode:: r
    

    plot(gTrack(gr))

.. figure:: figure/rst-plot3-1.png
    :alt: plot of chunk rst-plot3

    plot of chunk rst-plot3


.. sourcecode:: r
    

    plot(gTrack(gr , stack.gap = 2))

.. figure:: figure/rst-plot4-1.png
    :alt: plot of chunk rst-plot4

    plot of chunk rst-plot4


.. sourcecode:: r
    

    plot(gTrack(gr , stack.gap = 3))

.. figure:: figure/rst-plot5-1.png
    :alt: plot of chunk rst-plot5

    plot of chunk rst-plot5

**gTrack(gr , y.field = 'GC')**

**vector or scalar numeric specifiying gap between tracks (add a dimension to the data)**


.. sourcecode:: r
    

    plot(gTrack(gr , y.field = 'GC'))

.. figure:: figure/y.field-plot-1.png
    :alt: plot of chunk y.field-plot

    plot of chunk y.field-plot

