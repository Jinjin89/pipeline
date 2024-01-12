# Pipeline

## 1. RNA

### 1.1 STAR-mRNA

done

### 1.2 Trinity_CTAT


### 1.3 star-fusion

This tutorial adheres to the outlined pipeline available at here[^1] and here[^2] and here[^3]

1. required data
    data resources:
        * https://github.com/FusionAnnotator/CTAT_HumanFusionLib/releases
        * https://data.broadinstitute.org/Trinity/CTAT_RESOURCE_LIB/

    ```
    Genome data:
        genome.fa    : genome sequence
        genome.gtf   : transcript structure annotations for these genes.

    RNA-Seq data:
        rnaseq_1.fastq.gz :  RNA-Seq read 1 data ('left' read)
        rnaseq_2.fastq.gz :  RNA-Seq read 2 data ('right' read)

    Meta data:
        CTAT_HumanFusionLib.dat.gz : fusion annotation library
    ```

2. required software

    STAR-Fusion
    STAR aligner
    Trinity
    GMAP
    NCBI BLAST+

    ```bash
    mamba install -c bioconda star-fusion
    mamba install -c bioconda dfam # used for building ctat_genome_lib_build_dir
    ```

3. workflow

    1. build the genome
        ```bash
        prep_genome_lib.pl \
            --genome_fa {genome.fa} \
            --gtf {genome.gtf} \
            --fusion_annot_lib {CTAT_HumanFusionLib} \
            --dfam_db human \
            --pfam_db current \
            --human_gencode_filter
        ```
    2. run the star-fusion 

        ```bash
        {params.tool}\
            --left_fq {input.fq1} \
            --right_fq {input.fq2} \
            --genome_lib_dir {params.ctat_genome_lib_build_dir}\
            -O {params.dir}\
            --CPU {threads}\
            --STAR_PATH {params.STAR_PATH}
        ```


### Arriba

Arriba takes the main output file of STAR (`Aligned.out.bam`) as input (parameter `-x`). If STAR was run with the parameter --chimOutType WithinBAM, then this file contains all the information needed by Arriba to find fusions. When STAR was run with the parameter `--chimOutType SeparateSAMold`, the main output file lacks chimeric alignments. Instead, STAR writes them to a separate output file named `Chimeric.out.sam`. In this case, the file needs to be passed to Arriba via the parameter `-c` in addition to the main output file `Aligned.out.bam`.


### 1.4 spladder
done


### 1.5 mutation calling


## 2. WES


## 3. WGS


## 4. panel


[^1]: https://github.com/STAR-Fusion/STAR-Fusion-Tutorial/wiki
[^2]: https://github.com/STAR-Fusion/STAR-Fusion/wiki
[^3]: https://github-wiki-see.page/m/STAR-Fusion/STAR-Fusion/wiki/installing-star-fusion
