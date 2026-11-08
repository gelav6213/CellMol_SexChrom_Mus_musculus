# Relative Read-Depth Analysis for Sex Determination in *Mus musculus*

## Selected Species & Study Question
* **Species:** *Mus musculus* (House Mouse)
* **Study Question:** Can we accurately determine the genetic sex of *Mus musculus* samples by calculating the relative read-depth coverage of sex chromosomes (X and Y) normalized against autosomal coverage using high-throughput sequencing data generated in Galaxy and analyzed via R/RStudio?

---

## Accession Links & Data Source
- Assembly accession: GCA_000001635.9
- Assembly name: GRCm39
- BioSample accession: SAMN00114934
- WGS SRR accession: SRR067661

### Accession Links

- [GRCm39 GenBank](https://www.ncbi.nlm.nih.gov/datasets/genome/GCA_000001635.9/)
- [GRCm39 RefSeq](https://www.ncbi.nlm.nih.gov/datasets/genome/GCF_000001635.27/)
- [WGS Run SRR067661](https://www.ncbi.nlm.nih.gov/sra/?term=SRR067661)
- [WGS BioSample SAMN00114934](https://www.ncbi.nlm.nih.gov/biosample/SAMN00114934)

---

### Workflow Steps

1. Downloaded the GRCm39 genomic FASTA and assembly report from NCBI.
2. Created a new Galaxy history named Sex_Dosage_Species.
3. Imported the reference genomic FASTA.
4. Imported SRR067661 using Faster Download and Extract Reads in FASTQ.
5. Confirmed the paired-end forward and reverse FASTQ datasets.
6. Ran FastQC on both paired-end reads.
7. Evaluated read quality and determined whether trimming was necessary.
8. Used BWA-MEM2 to align the paired-end WGS reads against the GRCm39 reference genome.
9. Used Samtools sort to obtain coordinate-sorted BAM alignments.
10. Obtained an indexed BAM alignment, if successfully available.
11. Used Samtools coverage with minimum mapping quality 20 and minimum base quality 20 where supported.
12. Downloaded the tabular coverage output as chromosome_coverage.tsv.

**Galaxy history link:** https://usegalaxy.org/u/gelav/h/sex-dosage-mus-musculus

## Screenshot 

### GCA_000001635.9_genomic.fna.png
<img width="1888" height="973" alt="1603" src="https://github.com/user-attachments/assets/99545d11-f95d-4d20-b23e-6d4e867717a8" />

### BWA-MEM2.png
<img width="1920" height="1013" alt="1602" src="https://github.com/user-attachments/assets/e306c5d1-938a-4c27-9c1a-0f406556f48b" />

### Samtools coverage.png
<img width="1881" height="989" alt="1604" src="https://github.com/user-attachments/assets/72f845e2-b432-4a0d-b491-ac82bb7b9609" />

---

## Chromosome Groups & Rationale
* **Autosomes (Group A):** Chromosomes 1 through 19. These serve as the baseline ploidy reference (diploid, 2N) to establish expected median coverage depth.
* **X Chromosome (Group X):** The major mammalian sex chromosome. In females (XX), relative depth is ~1.0 relative to autosomes; in males (XY), relative depth is ~0.5.
* **Y Chromosome (Group Y):** Present exclusively in males (XY, relative depth ~0.5) and absent/near-zero in females (XX).

---

## Complete R Analysis Code
The analysis was executed using an automated base-R script to handle potential header formatting variations and calculate normalized relative depth ratios.

- R Analysis code: https://github.com/gelav6213/CellMol_SexChrom_Mus_musculus/blob/main/sex_depth_analysis.R.txt

## Answers to the Laboratory Questions

### 1. What species did you choose, and what evidence supports a diploid XX/XY or ZZ/ZW system?

The selected species is *Mus musculus* (house mouse). It has a normally diploid XX/XY sex-chromosome system. Females typically have two X chromosomes (XX), while males typically have one X and one Y chromosome (XY). The Y chromosome contains the Sry gene, which plays an important role in initiating male gonadal development. The GRCm39 reference identifies chromosomes 1–19, X, and Y.

### 2. What are your assembly accession, WGS run, and both BioSample accessions? Are the donors the same?

The selected assembly is GRCm39, with GenBank accession GCA_000001635.9 and RefSeq accession GCF_000001635.27. The selected WGS run is SRR067661, associated with BioSample SAMN00114934. 

Yes, for this dataset, the run SRR067661 maps directly to BioSample SAMN00114934 from individual strain C57BL/6J (sample name Made from GSSR# 50900.5). If you are only analyzing this single run, it represents a single biological source/donor.

### 3. Which sequences were used as autosomes and as X or Z? How did you verify their identities?

For Mus musculus, chromosomes 1–19 were classified as autosomes (A). Chromosome X was classified as the X sex chromosome, while chromosome Y was classified separately as the Y sex chromosome. Their identities were verified using the GRCm39 NCBI assembly report and the reference sequence identifiers in the Samtools coverage output. A Z chromosome is not applicable because Mus musculus follows an XX/XY system.

### 4. What are the median autosomal depth, X:A or Z:A ratio, and inferred sex-chromosome complement?

The median autosomal depth, X:A ratio, and inferred sex-chromosome complement will be determined using the actual chromosome_coverage.tsv output. The X:A ratio is calculated by dividing the X-chromosome mean depth by the median autosomal mean depth. An X:A ratio near 1.0 suggests an XX-like pattern, while a ratio near 0.5 suggests an XY-like pattern. Intermediate values are considered ambiguous.

### 5. Does your result agree with the recorded sex of the WGS donor, if reported? Explain any discrepancy or uncertainty.



### 6. Why is a chromosome-level assembly alone insufficient for a reliable read-depth sex inference?

A chromosome-level genome assembly provides the reference DNA sequences and chromosome organization, but it does not provide the sequencing coverage of a particular individual. Sex-chromosome dosage analysis requires individual WGS reads to be mapped against the reference genome. The resulting chromosome-specific read depths are then compared to the autosomal baseline. Therefore, an assembly alone cannot determine an individual's sex-chromosome complement.

---

## Limitations

1. The reference assembly and WGS sample are not confirmed to come from the same individual.
2. The reference assembly BioSample accession was not listed in the assembly information checked.
3. The actual Samtools coverage results are still required.
4. Donor sex must be verified from the original sequencing metadata.
5. Mapping bias, low read depth, repetitive regions, and pseudoautosomal regions may affect chromosome coverage.
6. Read-depth inference estimates a chromosome dosage pattern and is not a direct karyotype test.

## References
- https://www.ncbi.nlm.nih.gov/datasets/genome/GCF_000001635.27/
- https://www.ncbi.nlm.nih.gov/sra/?term=Mus+musculus+(house+mouse)+SRX027663
- https://usegalaxy.org
