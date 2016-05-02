Vignette Showing How to Graph Mutations 
=======================================

**Draw.paths**



.. sourcecode:: r
    

    ## To create fake genes, create two GRanges objects each filled with random ranges from chromosomes 1 and 2. Ranges fall within the 1-5e3 sequence
    
    gene1 = sort(sample(gr.tile(parse.gr('1:1-5e3+'), 50), 5))
    gene2 = rev(sort(sample(gr.tile(parse.gr('2:1-5e3-'), 50), 12)))
    gene3 = sort(sample(gr.tile(parse.gr('3:1-5e3+'), 50), 8))
    
    ## Combine into GRangesList
    grl = GRangesList(gene1 = gene1, gene2 = gene2, gene3 = gene3)
    gt.genes = gTrack(grl)
    
    ## Subset GRangesList and plot but, connect ranges using draw.paths
    fusion = GRangesList(c(grl$gene1[1:3], grl$gene2[5:9], grl$gene3[7:8]))
    gt.fusion = gTrack(fusion, draw.paths = TRUE)


.. sourcecode:: r
    

    plot(c(gt.fusion, gt.genes))

.. figure:: figure/-plotPaths-1.png
    :alt: plot of chunk -plotPaths

    plot of chunk -plotPaths

**Add Exons by gr.labelfield**


.. sourcecode:: r
    

    ##Create a column that keeps track of the exons
    
    gene1$exon = 1:length(gene1)
    gene2$exon = 1:length(gene2)
    gene3$exon = 1:length(gene3)
    
    grl = GRangesList(gene1 = gene1, gene2 = gene2, gene3 = gene3)
    gt.genes = gTrack(grl, col = 'gray90', gr.labelfield = 'exon')
    fusion = GRangesList(c(grl$gene1[1:3], grl$gene2[5:9], grl$gene3[7:8]))
    gt.fusion = gTrack(fusion, draw.paths = FALSE)
    gt.fusion.o = gTrack(fusion, draw.paths = TRUE)
    win = parse.gr(c('1:1-1e4', '2:1-1e4', '3:1-1e4'))



.. sourcecode:: r
    

    plot(c(gt.genes, gt.fusion, gt.fusion.o), win +1e3)

.. figure:: figure/-plotList-1.png
    :alt: plot of chunk -plotList

    plot of chunk -plotList

**Example of How to Graph Variants** 


.. sourcecode:: r
    

    ## Create a GRanges
    fake.genome = c('1'=1e4, '2'=1e3, '3'=5e3)
    tiles = gr.tile(fake.genome, 1)
    
    ## Choose 5 random indices 
    hotspots = sample(length(tiles), 5)
    ##Create numeric vector representing each range in GRanges object. 
    prob = rep(1, length(tiles))
    ## Simulate hotspots by adding 50 to those random indices.
    prob[hotspots] = prob[hotspots] + 50
    
    ## randomly select 2000 sequences. Probability of choosing variants is high.
    mut = sample(tiles, 2000, prob = prob)
    gt.mut = gTrack(mut, circle = TRUE, stack.gap = 100)
    win = si2gr(fake.genome)



.. sourcecode:: r
    

    plot(gt.mut, win)

.. figure:: figure/mutations-plot-1.png
    :alt: plot of chunk mutations-plot

    plot of chunk mutations-plot


