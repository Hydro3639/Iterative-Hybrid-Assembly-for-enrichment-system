# Iterative-Hybrid-Assembly-for-the-Enrichment-Ecoystem
#### Unicycler based iterative hybrid assembly for high-quality MAGs from `enrichment ecosystems`

* IHA (Iterative Hybrid Assembly) is a Unicycler-based workflow for high-quality and high-contiguity genome reconstruction from `the enrichment system`. The proposed Iterative method requires that high-quality and high-contiguity genomes could be retrieved from the initial hybrid assembly using Unicycler. If MAGs are highly fragmented, other assemblers might be considered. Notably, compared with short-read assembly and binning process, the hyrbid assembly process might take longer time (2-3x). However, the time for hybrid assembly process in the following cycle will shorter as the dataset is decreasing. Normally, A workstaion with 40 threads and 256 Gbp RAM is good enough to handle the process.

* As Unicycler is designed for bacterial isolates, the Unicycler author expressly cautions against the use of this assembler for the analysis of data other than bacterial isolates. So we first established the feasibility of using Unicycler to retrieve genomes by different mock datasets (ZymoBIOMICS Microbial Community Standards (EVEN), Nicholls et. al., 2019), Human Microbiome Project (HMP), Kuleshov et. al., 2014 and GIS20, Bertrand et al., 2019). The evaluation results presented the robustness of this assembly tool. However, if you applied Unicycler to assemble high-complexity metagenomes, e.g., soil or activated sludge samples, the performance might be not so positive due to the algorithms and assumptions of Unicycler and SPAdes. We have also developed the hybrid assembly workflow for highly complex environmental samples and integrated with this iterative hybrid assembly/binning method (`will coming soon`).

* Here we provided some basic bash scripts, so it would be easier for the beginer to follow.

* If you are using the IHA workflow for the genome reconstruction, please cite the corresponding reference papers, particullary the paper described the [Unicycler](https://github.com/rrwick/Unicycler)!

  * Unicycler: Resolving bacterial genome assemblies from short and long sequencing reads <br>
  * MetaWRAP—a flexible pipeline for genome-resolved metagenomic data analysis
  (will update other papers sooner)

## Citing our paper
* If you found IHA workflow usefull in your research, a citation would be appreciated! <br>High-Quality Bacterial Genomes of a Partial-Nitritation/Anammox System by an Iterative Hybrid Assembly Method
