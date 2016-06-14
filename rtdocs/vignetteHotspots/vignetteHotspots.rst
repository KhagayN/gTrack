How To Graph Relationships In The Genome 
=========================================================

Genes interact with each other and gTrack is capable of graphing them. 

In this vignette, the **draw.paths** and **circle** parameters of gTrack will aide in illustrating gene interactions. Specifically, they will be used in graphing variants in random sequences. 

Using Draw.paths Parameter 
~~~~~~~~~~~~~~~~~~~~~~~~~~

To prepare a data set that illustrates the draw.paths parameter, a GRangesList storing RANDOM sequences in chromosomes 1,2, and 3 is created. Then, two graphs, one with and one without the draw.paths parameter will be made. The difference in the two show the affect the draw.paths parameter has on graphs. 


.. sourcecode:: r
    

    gene1 = sort(sample(gUtils::gr.tile(gUtils::parse.gr('1:1-5e3+'), 50), 5))
    gene2 = rev(sort(sample(gUtils::gr.tile(gUtils::parse.gr('2:1-5e3-'), 50), 12)))
    gene3 = sort(sample(gUtils::gr.tile(gUtils::parse.gr('3:1-5e3+'), 50), 8))
    
    ##Create a column that keeps a counter of the exon number.
    
    gene1$exon = 1:length(gene1)
    gene2$exon = 1:length(gene2)
    gene3$exon = 1:length(gene3)
    
    ## Combine into GRangesList
    grl = GRangesList(gene1 = gene1, gene2 = gene2, gene3 = gene3)
    
    gt.genes = gTrack(grl)
    
    ## Plot two graphs, one with and one without the draw.paths parameter. 
    fusion = GRangesList(c(grl$gene1[1:3], grl$gene2[5:9], grl$gene3[7:8]))
    gt.fusion = gTrack(fusion, draw.paths = FALSE)
    gt.fusion.o = gTrack(fusion, draw.paths = TRUE)
    
    ## separating the windows for the graph. 
    win = gUtils::parse.gr(c('1:1-1e4', '2:1-1e4', '3:1-1e4'))


.. sourcecode:: r
    

    plot(c(gt.genes, gt.fusion, gt.fusion.o), win +1e3)

.. figure:: figure/-plotList-1.png
    :alt: plot of chunk -plotList

    plot of chunk -plotList

Graphing Copy Number Variations
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

To illustrate gTrack's functionality in graphing copy number variations(CNVs), a data set of sequences is created and a few of them will are picked as variants. This data will be graphed and because there are outliers (variants), they will be easily visable. This vignette also exemplifies how/when to use the gTrack **stack.gap** parameter.  


.. sourcecode:: r
    

    ## create sequences from chromosomes 1-3. 
    fake.genome = c('1'=1e4, '2'=1e3, '3'=5e3)
    tiles = gr.tile(fake.genome, 1)
    
    ## Choose 5 random indices. These indices will store the variants. 
    hotspots = sample(length(tiles), 5)
    
    ## for each sequence, calculate the shortest distance to one of the hotspots.
    d = pmin(Inf, values(distanceToNearest(tiles, tiles[hotspots]))$distance, na.rm = TRUE)
    ## for sequences near the hotspots, the "prob" will be a higher positive number. It becomes smaller as it moves farther from the hotspot. 
    prob = .05 + exp(-d^2/10000)


.. sourcecode:: r
    

    ## sample 2000 of the sequences. the one nearer to the hotspots will "probably" be selected.
    mut = sample(tiles, 2000, prob = prob, replace = TRUE) 


::

    ## Error in sample.int(length(x), size, replace, prob): incorrect number of probabilities


.. sourcecode:: r
    

    ## graph with different degrees of stack.gap. The higher numeric supplied to stack.gap helps separate the data, visually. 
    gt.mut0 = gTrack(mut, circle = TRUE, stack.gap = 0, name = "Track 0")


::

    ## Error in listify(data, GRanges): object 'mut' not found


.. sourcecode:: r
    

    gt.mut2 = gTrack(mut, circle = TRUE, stack.gap = 2, name = "Track 2")


::

    ## Error in listify(data, GRanges): object 'mut' not found


.. sourcecode:: r
    

    gt.mut10 = gTrack(mut, circle = TRUE, stack.gap = 10, name = "Track 10")


::

    ## Error in listify(data, GRanges): object 'mut' not found


.. sourcecode:: r
    

    gt.mut50 = gTrack(mut, circle = TRUE, stack.gap = 50, name = "Track 50")


::

    ## Error in listify(data, GRanges): object 'mut' not found




.. sourcecode:: r
    

    win = si2gr(fake.genome)
    plot(c(gt.mut0, gt.mut2, gt.mut10, gt.mut50), win)


::

    ## Error in plot(c(gt.mut0, gt.mut2, gt.mut10, gt.mut50), win): object 'gt.mut0' not found


