Student Name: Nicolas Bailly

**Introduction**

This report has two objectives. Firstly, to assemble the genome of Salmonella enterica from raw reads provided in FASTQ format. The second objective is to assess the accuracy of the consensus genome by comparing it to a reference genome downloaded from NCBI, allowing the quantification of similarities and differences. As such, the goal is to determine the strengths and challenges of bioinformatics methods to assemble and align bacterial genomes.

Salmonella enterica is of particular interest because it contains a high diversity of antimicrobial resistance genes (Li et al., 2021). Among isolates obtained from commercial meat products, researchers have found 33 different serotypes (Li et al., 2021), demonstrating an abundant phylogenetic diversity that contributes to the variety of resistance genes observed. Regarding sequencing technologies, methods that rely on short reads often produce low-quality genomes with high numbers of contigs (Smits, 2019). As such, newer approaches like the Oxford Nanopore sequencer reduce the number of contigs produced (Lu et al., 2006).

Several methods exist to assemble a genome and to align it to a reference genome. Regarding the former, the flye package has a lower error rate than other methods, but it uses a high amount of RAM (Wick and Holt, 2020). Compared to it, Canu has a substantially longer runtime and performs poorly with circular DNA molecules while producing relatively reliable assemblies (Wick and Holt, 2020). Regarding variant calling as a comparison method, incorrect read alignment to the reference genome may lead to amplification biases (Zverinova and Guryev, 2022). However, its usage on long reads is significantly useful to detect structural differences and to differentiate between tandem and dispersed copy-number variants (Zverinova and Guryev, 2022). The Whole Genome Assembly (WHA) method also exhibits trade-offs. While it is capable of managing a large volume of genomic data in a time-efficient way, its accuracy is often limited (Saada et al., 2024). As such, flye and variant calling will be used as part of the methodology.

In terms of my overall approach, I will use flye to assemble the genome (–nano-raw option), minimap2 to align it to the reference genome, SAMtools (view, sort, index options) to process the alignment, and IGV to visualize it. The latter three have pros and cons: Even though minimap2 is uniquely capable of performing read overlapping, the whole downstream analysis depends on its metrics being correctly set (Wick and Holt, 2020). SAMtools efficiently processes large BAM files, but indexing them is memory-intensive (Weeks and Luecke, 2017). Likewise, even though the Integrative Genomics Viewer (IGV) aids in the interpretation of the alignments, it may be hard to navigate when using large genomes. Regarding the general parameters, using 32 cores throughout all the analysis will reduce the computational time, but will limit reproducibility by requiring access to external servers (like Canada Compute). Furthermore, the use of –nano-raw will allow the assembly of long sequences while still requiring polishing if the raw reads have high error rates.

**Methods**

The starting raw reads of Salmonella enterica are in a FASTQ format produced via Oxford Nanopore sequencing. These will be processed in the Narval cluster of the Digital Research Alliance of Canada using Bash. Firstly, the seqtk package (v. 1.4) will be used to remove the lower-quality bases from both ends. If necessary, fixed numbers of bases will also be trimmed from the left (-b 5 option) and right (-e 10 option) sides of the reads (Heng Li, 2012). The reference genome will be downloaded from NCBI. Then, flye (v 2.9.6) and minimap2 (v 1.18) packages will be used to assemble the genome (--nano-raw) and to align it to the reference one (Kolgomorov et al., 2016; Dana-Farber Cancer Institute, 2018), respectively. 

SAMtools (v 1.18) will subsequently be used to convert the alignment to a BAM format, view it (-bS option), sort it,  and index it (Thomas et al., 2025, Genome Research Ltd. 2025). Thereafter, bcftools (v. 1.22) will be used to call variants by comparing the aligned reads with the reference genome and producing an output BCF file (-Ob, mpileup --max-depth 1000 options) (SAMtools team). This file will then be visualized using the IGV (Robinson et al., 2017). Aside from the number of cores used being 32 and the specified options, the rest of the parameters are default.

**References**

1.Dana-Farber Cancer Institute (2018) Minimap2, GitHub. Available at: https://github.com/lh3/minimap2?tab=License-1-ov-file (Accessed: 17 January 2026).

2.Genome Research Ltd. (2025) Samtools, GitHub. Available at: https://github.com/samtools/samtools?tab=License-1-ov-file (Accessed: 17 January 2026).

3.Kolmogorov, M. (2016) Flye, GitHub. Available at: https://github.com/mikolmogorov/Flye/tree/flye?tab=License-1-ov-file (Accessed: 17 January 2026).

4.Li, C. et al. (2021) ‘Long-read sequencing reveals evolution and acquisition of antimicrobial resistance and virulence genes in Salmonella enterica’, Frontiers in Microbiology, 12. doi:10.3389/fmicb.2021.777817.

5.Li, H. (2012) Seqtk, GitHub. Available at: https://github.com/lh3/seqtk?tab=MIT-1-ov-file (Accessed: 17 January 2026).

6.Lu, H., Giordano, F. and Ning, Z. (2016) ‘Oxford Nanopore Minion Sequencing and Genome Assembly’, Genomics, Proteomics & Bioinformatics, 14(5), pp. 265–279. doi:10.1016/j.gpb.2016.05.004.

7.Robinson, P. and Zemo, J. T. (2017) ‘Integrative genomics viewer (IGV): Visualizing alignments and variants’, Computational Exome and Genome Analysis, pp. 233–245. doi:10.1201/9781315154770-17.

8.Saada, B. et al. (2024) ‘Whole genome alignment: Methods, challenges, and future directions’, Applied sciences. doi:10.20944/preprints202402.1197.v1.

9.SAMtools Team. BCFtools HOW TO. Available at: https://samtools.github.io/bcftools/howtos/index.html (Accessed: 17 January 2026).

10.Smits, T.H. (2019) ‘The importance of genome sequence quality to microbial comparative genomics’, BMC Genomics, 20(1). doi:10.1186/s12864-019-6014-5.

11.Thomas, C. et al. (2025) ‘Accurately assembling nanopore sequencing data of highly pathogenic bacteria’, BMC Genomics, 26(1). doi:10.1186/s12864-025-11793-6.

12.Weeks, N.T. and Luecke, G.R. (2017) ‘Optimization of SAMtools sorting using openmp tasks’, Cluster Computing, 20(3), pp. 1869–1880. doi:10.1007/s10586-017-0874-8. 

13.Wick, R.R. and Holt, K.E. (2020) ‘Benchmarking of long-read assemblers for prokaryote whole genome sequencing’, F1000Research, 8, p. 2138. doi:10.12688/f1000research.21782.3.

14.Zverinova, S. and Guryev, V. (2022) ‘Variant calling: Considerations, practices, and developments’, Human Mutation, 43(8), pp. 976–985. doi:10.1002/humu.24311.
