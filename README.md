#  Vertebrate Genome Assembly using HiFi + Hi-C (VGP Pipeline)

##  Project Overview
This project demonstrates a **high-quality de novo genome assembly** using the **Vertebrate Genome Project (VGP) pipeline**, focusing on the **Hi-C phased mode of hifiasm**.

The workflow integrates:
- PacBio HiFi long reads (high accuracy, long reads)
- Illumina Hi-C data (chromosome-scale scaffolding)
- Bioinformatics tools for assembly, polishing, and quality evaluation

The final goal is to reconstruct a **chromosome-level genome assembly** with high completeness and accuracy.

---

##  Objectives
- Perform de novo genome assembly using HiFi reads
- Use Hi-C data for haplotype phasing (Hi-C phased mode)
- Evaluate genome quality using reference-free methods
- Scaffold contigs into chromosome-scale sequences
- Assess completeness and correctness of assembly

---

##  Input Data
- PacBio HiFi reads (trimmed using Cutadapt)
- Illumina Hi-C paired-end reads
- Synthetic dataset of *Saccharomyces cerevisiae* (yeast model genome)

---

##  Tools Used
- Cutadapt (adapter removal)
- Meryl (k-mer counting)
- GenomeScope2 (genome profiling)
- hifiasm (Hi-C phased assembly)
- gfastats (assembly statistics)
- BUSCO (gene completeness)
- Merqury (k-mer based evaluation)
- BWA-MEM2 (Hi-C mapping)
- YaHS (Hi-C scaffolding)
- PretextMap (contact maps visualization)

---

# ⚙️ Workflow Steps

## 1️⃣ Data Preprocessing
- HiFi reads were cleaned using **Cutadapt**
- Adapters and low-quality reads were removed
- Output: Clean HiFi read collection

---

## 2️⃣ Genome Profiling
### k-mer Analysis
- Tool: Meryl (k = 31)
- Generated k-mer frequency distribution

### Genome Estimation
- Tool: GenomeScope2
- Estimated:
  - Genome size ≈ ~11.7 Mb
  - Heterozygosity ≈ 0.5%
  - Diploid structure confirmed (bimodal distribution)

📊 **Result Figure: GenomeScope Plot**
<img width="800" height="800" alt="genomescope_plot" src="https://github.com/user-attachments/assets/94fdbc79-9187-4786-83a6-384d315a9859" />


---

## 3️⃣ Genome Assembly (Hi-C Phased Mode)
- Tool: hifiasm
- Input:
  - HiFi reads
  - Hi-C paired-end reads

### Output:
- Hap1 contigs (phased assembly)
- Hap2 contigs (phased assembly)

---

## 4️⃣ Assembly Statistics
- Tool: gfastats

| Metric | Hap1 | Hap2 |
|--------|------|------|
| Contigs count | ~16 | ~17 |
| Total length | ~11.3 Mb | ~12.2 Mb |
| Assembly type | Contigs | Contigs |

📊 Interpretation:
- Both haplotypes show similar contiguity
- No major imbalance between assemblies

---

## 5️⃣ Assembly Quality Assessment

### BUSCO Analysis
- Evaluates gene completeness

| Category | Result |
|----------|--------|
| Complete BUSCOs | High percentage (~complete genome) |
| Duplicated genes | Very low |
| Missing genes | Minimal |

📊 Busco Figure

<img width="500" height="500" alt="busco 3" src="https://github.com/user-attachments/assets/6098474b-4742-4ed3-b483-0dfaaa1db845" /><img width="500" height="500" alt="busco2" src="https://github.com/user-attachments/assets/de11c1b1-4d61-4cb2-a1d1-1559cf720856" />


---

### Merqury Evaluation
- Reference-free validation using k-mers

Key observations:
- High k-mer completeness
- Proper haplotype separation
- Low error rate (high QV score)

📊 Merqury plots 

<img width="800" height="800" alt="merqury 1" src="https://github.com/user-attachments/assets/9b2a71a2-ba1c-40ce-8ac9-36e83573bf05" />
<img width="800" height="800" alt="merqury 2" src="https://github.com/user-attachments/assets/e6c07722-4e19-4d06-a76b-ac91bdf2f31c" />


---

## 6️⃣ Hi-C Mapping & Contact Maps
- Reads aligned using BWA-MEM2
- Contact maps generated using PretextMap

### Observations:
- Strong diagonal patterns → correct chromosome structure
- Clear chromosome territories
- Minimal misassemblies after scaffolding


---

## 7️⃣ Hi-C Scaffolding (YaHS)
- Tool: YaHS
- Uses Hi-C interaction frequencies to:
  - Order contigs
  - Orient contigs
  - Build chromosome-scale scaffolds

### Output:
- Chromosome-level scaffolds
- Improved genome continuity

---

## 8️⃣ Final Evaluation
- Final assembly compared to reference genome

### Key Results:
- Genome size nearly identical to reference
- Chromosome structure successfully reconstructed
- High structural accuracy confirmed via Hi-C maps
<img width="1000" height="500" alt="hi-c_pretext_conclusion" src="https://github.com/user-attachments/assets/206f82cd-f02e-4902-934a-3ea0b83cddb0" />


---

# 📊 Summary of Results

| Feature | Outcome |
|--------|--------|
| Assembly type | Chromosome-level |
| Phasing | Successful (Hi-C based) |
| Completeness | High (BUSCO supported) |
| Accuracy | High (Merqury QV confirmed) |
| Scaffolding | Successful (YaHS + Hi-C) |

---

#  Conclusion
This project successfully demonstrated a **complete vertebrate genome assembly workflow** using:
- Long-read sequencing (HiFi)
- Chromatin conformation data (Hi-C)
- Modern genome assembly tools

The Hi-C phased approach produced:
- Well-separated haplotypes
- Highly complete genome assembly
- Accurate chromosome-scale scaffolds

Overall, the pipeline shows how modern sequencing technologies can reconstruct complex genomes with high precision.

---

# 🔄 Workflow Flowchart

```mermaid
flowchart TD

A[Raw HiFi Reads] --> B[Cutadapt Filtering]
B --> C[k-mer Analysis - Meryl]
C --> D[GenomeScope2 Profiling]

D --> E[hifiasm Hi-C Phased Assembly]

E --> F[Hap1 Contigs]
E --> G[Hap2 Contigs]

F --> H[gfastats Statistics]
G --> H

H --> I[BUSCO Analysis]
H --> J[Merqury Evaluation]

F --> K[BWA-MEM2 Hi-C Mapping]
K --> L[Pretext Contact Map]

F --> M[YaHS Scaffolding]
M --> N[Final Chromosome Assembly]

N --> O[Final Quality Assessment]
