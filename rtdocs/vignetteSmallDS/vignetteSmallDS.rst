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

.. figure:: figure/plot-tiles-1.png
    :alt: plot of chunk plot-tiles

    plot of chunk plot-tiles

**Overlapping Tiles - gTrack(gr + n)**

**indicate the degree of overlap**


.. sourcecode:: r
    

    plot(gTrack(gr+10))

.. figure:: figure/plot-overlappingtiles-1.png
    :alt: plot of chunk plot-overlappingtiles

    plot of chunk plot-overlappingtiles

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

.. figure:: figure/plot-gr-1.png
    :alt: plot of chunk plot-gr

    plot of chunk plot-gr


.. sourcecode:: r
    

    plot(gTrack(gr , stack.gap = 2))

.. figure:: figure/plot-stack.gap2-1.png
    :alt: plot of chunk plot-stack.gap2

    plot of chunk plot-stack.gap2


.. sourcecode:: r
    

    plot(gTrack(gr , stack.gap = 3))

.. figure:: figure/plot-stack.gap3-1.png
    :alt: plot of chunk plot-stack.gap3

    plot of chunk plot-stack.gap3

**gTrack(gr , y.field = 'GC')**

**vector or scalar numeric specifiying gap between tracks (add a dimension to the data)**


.. sourcecode:: r
    

    plot(gTrack(gr , y.field = 'GC'))

.. figure:: figure/plot-y.fieldGC-1.png
    :alt: plot of chunk plot-y.fieldGC

    plot of chunk plot-y.fieldGC

**gTrack(gr , bars = TRUE/FALSE)**


.. sourcecode:: r
    

    plot(gTrack(gr , y.field = 'GC' , bars = TRUE , col = 'light blue'))

.. figure:: figure/plot-bars-1.png
    :alt: plot of chunk plot-bars

    plot of chunk plot-bars

**gTrack(gr , lines = TRUE/FALSE)**


.. sourcecode:: r
    

    plot(gTrack(gr , y.field = 'GC' , lines = TRUE , col = 'purple'))

.. figure:: figure/plot-lines-1.png
    :alt: plot of chunk plot-lines

    plot of chunk plot-lines

**gTrack(gr , circles = TRUE/FALSE)**


.. sourcecode:: r
    

    plot(gTrack(gr , y.field = 'GC' , circles = TRUE , col = 'magenta' , border = '60'))

.. figure:: figure/plot-circles-1.png
    :alt: plot of chunk plot-circles

    plot of chunk plot-circles

**colorfield**

**map values to colors! Legend is automatically added**


.. sourcecode:: r
    

    plot(gTrack(gr , y.field = 'GC' , bars = TRUE , col = NA , colormaps = list(GC = c("1"="red" , "2" = "blue" , "3"="magenta", "4"="light blue" ,"5"="black" , "6"="green", "7"="brown" , "8"="pink", "9"="yellow", "10" = "orange")) ))

.. figure:: figure/plot-colorfield-1.png
    :alt: plot of chunk plot-colorfield

    plot of chunk plot-colorfield

**gr.colorfield**


.. sourcecode:: r
    

    plot(gTrack(gr , y.field = 'GC' , bars = TRUE , col = NA , gr.colorfield = 'GC'))

.. figure:: figure/plot-gr.colorfield-1.png
    :alt: plot of chunk plot-gr.colorfield

    plot of chunk plot-gr.colorfield

**gr.labelfield**


.. sourcecode:: r
    

    plot(gTrack(gr , y.field = 'GC' , bars = TRUE , col = NA , gr.colorfield = 'GC' , gr.labelfield = 'name'))

.. figure:: figure/plot-labelfield-1.png
    :alt: plot of chunk plot-labelfield

    plot of chunk plot-labelfield

**GRangesList**


