# mscom
Music structure analysis via community detection.

## Description

The code for the evaluation of the segmentation algorithms reported in our paper can be found in ``comparison.ipynb`` as a jupyter notebook. The study of monotonicity of the human-annotated segmentations of the tracks in the [SALAMI dataset](https://ddmal.music.mcgill.ca/research/SALAMI/), as well as LSD's segmentations, is reported in ``monotonicity_study.ipynb``. Finally, the test for statistical significance of the evaluations (both the pre and post analyses) can be found in ``statistical_tests.ipynb``.

As detailed in our paper, the tracks considered for our experiments correspond to the publicly available portion of the SALAMI dataset and they come from two different sources: the Internet Archive (instructions can be found [here](https://github.com/DDMAL/salami-data-public)) and the [SALAMI Youtube project](http://jblsmith.github.io/Getting-SALAMI-from-YouTube/). The full list of SALAMI's tracks that we used for the evaluation is reported in ``data/salami_tracks``.

*TODO: link to the Zenodo project with the new dataset SALAMI HXT.*


## An example of hierarchical segmentation with DMSCOM

To provide a practical example of the proposed methodology, we report the structural segmentation obtained with MSCOM on the SALAMI track 676 ("The Loner" by Neil Young).
As it can be seen in Figure 1, the long-term structure of this rock song clearly emerges from the block-like visual patterns in the recurrence plot.
A closer visual analysis also reveals the presence of nested sub-structures corresponding to progressively shorter musical ideas.
These structural patterns at different resolution levels are what we aim to detect with the hierarchical multi-resolution community detection procedure of MSCOM.

![alt text](pics/music_graphs.png)
> **Figure 1**: Music graphs extracted from SALAMI 676: the recurrence graph captures the similarities between the harmonic features (chroma features) extracted from the track, whereas the proximity graph detects similarity between timbral features (mel-frequency cepstral coefficients). After a filtration process of the former one, these graphs are combined in a single graph (the rightmost).

As shown in Figure 2, it is possible to visualise the nested structures in terms of communities formed through the hierarchical segmentation performed by MSCOM on the track under analysis. Even though each colour should uniquely corresponds to a community and viceversa, some of them are repeated in the diagram due to the limited number of colours available in the chosen colour map.
The sunburst chart below, obtained from the segmentation performed by MSCOM on the track under analysis, makes it possible to visualise how our hierarchical segmentation enables to reveal nested structures in terms of communities progressively splitting into smaller groups.
As noticeable from the chart, communities do not split homogeneously as we traverse the hierarchy: some groups are more resistant to fragmentation and they can persist until the deepest levels in the hierarchy, whereas others tend to break rather quickly. Moreover, the fragmentation ratio of communities from one level to the next one is also related to the structural complexity of each group. In fact, if a certain community corresponds to a structural pattern which does not contain any other sub-structures, it would fragment into several smaller communities, which in the extreme case would be as many as the number of nodes in the original group.


![alt text](pics/salami_676_breakage_fix.png)
> **Figure 2**: Hierarchical segmentation of "SALAMI 676" with colours identifying communities. The innermost circle corresponds to the first segmentation level in the hierarchy, where all nodes belong to the same community, i.e. the starting point for the community detection algorithm; conversely, every node in the outmost circle forms a community per se. This plot is thus helpful to visualise the structure of the detected communities by illustrating how they progressively break into smaller ones.


![alt text](pics/l18.png)
> **Figure 3**: The combined graph obtained from SALAMI 676 (the adjacency matrix plotted in Figure 1) with nodes coloured according to the segmentation of the network at the 18th level (also plotted in Figure 4 and compared with human structural annotations). Considering the inheritance from the proximity graph, nodes are connected as a chain to reflect the temporal dimension of music whereas edges connecting non-subsequent nodes denote the harmonic and melodic similarity among them.

The SALAMI dataset provides two levels of reference structural annotations for each track: an upper-level segmentation, where the structural patterns corresponds to sections, and a lower-level segmentation that can reach the phrase level. To show the behaviour of MSCOM on the track under analysis, we compare both these human annotations with the segmentations estimated by our algorithm at the levels with closest alignment. In particular, Figure 4 compares the upper-level human annotation (the rightmost plot) with the estimation at level 18, for which we can observe a distinct overlapping. The additional segmentations reported (at levels 4 and 16) demonstrate that our algorithm can also detect superstructures which are more general than sections. A closer analysis is needed to check if a correspondence with the actual human perception of these musical structures can be found.


![alt text](pics/upper_comp.png)
> **Figure 4**: Comparison of the upper-level human segmentation for "SALAMI 676" (the rightmost) with the estimated segmentation at levels 4, 16, 18 in the hierarchy produced by MSCOM. Whereas the structural segmentation at the 18th level is at the same granularity of the upper-level reference annotations, the segmentations at levels 4 and 16 managed to uncover superstructures.

Analogously, Figure 5 presents a comparison of the lower-level human segmentation with the segmentation estimated by MSCOM at the 59th level. From a visual inspection we can notice that reference and estimated segmentations are similar with each other, even tough some structures are more fine-grained in the estimations. A reasonable explanation for this behaviour is related to the fact that communities do not split homogeneously along the hierarchy, thus a closer alignment with the reference segmentations would be obtained by combining multiple segmentation levels together and enforcing some properties during the creation of communities. In doing so, we would reduce the premature fragmentation of communities and ensure a more balanced split of musical structures across the hierarchy. However, we preferred to avoid any a posteriori manipulation that could bias the detection of structural patterns by modularity maximisation, and provide instead a parameter-free algorithm for structural analysis of music. We also report two segmentations deeper in the hierarchy in order to show the ability of MSCOM to detect structural patterns at a finer level than musical phrases, thereby unveiling motifs, i.e. the smallest structural unit possessing thematic identity.

![alt text](pics/lower_comp.png)
> **Figure 5**: Comparison of the lower-level human segmentation for "SALAMI 676" (the rightmost) with the estimated segmentations at levels 88, 68, 59 in the hierarchy produced by MSCOM. Analogously to the previous figure, whereas the 59th segmentation level has the best alignment with the lower-level reference annotation, the segmentations at levels 68 and 88 enables the discovery of more granular musical structures, providing a deeper analysis of the structure of the piece.
