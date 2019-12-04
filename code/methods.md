Rationale
---------

Like the proverbial man [searching for his lost keys under the lamp
post](https://www.matthewhirschey.com/articles/exploratory-mind) because
the light shines there, searching for biological truths often occurs
under ‘lamp posts’ because that’s where scientists can see.

Weave in cost and speed of science. How to people come up with
hypotheses?? Popular genes? Has to end with ddh.

Methods
-------

Functional genomics is a field of molecular biology that aims to
describe gene (and protein) functions and interactions, with the overall
goal to understand the function of all genes and proteins in a genome.
To do so, experimental strategies generally involve high-throughput,
genome-wide approaches rather than a more traditional “gene-by-gene”
approach. The advent and rapid adoption of data-sharing platforms has
provided high-quality data sets for public interrogation. Integration of
functional genomics data holds tremendous promise to knowledge and a
deep understanding of the dynamic properties of an organism.

**INSERT SCHEMATIC OF CONCEPT**

### Generate Data

Project Achilles is a systematic effort by the [Broad
Institute](https://www.broadinstitute.org) aimed at identifying and
cataloging gene essentiality across hundreds of genomically
characterized cancer cell lines using highly standardized pooled
genome-scale loss-of-function screens. This project uses
lentiviral-based pooled RNAi or CRISPR/Cas9 libraries in genome-scaled
pooled loss-of-function screening, which allows for the stable
suppression of each gene individually in a subset of cells within a
pooled format allowing for a cost-effective genome scale interrogation
of gene essentiality. Next, using computational modeling, an accurate
determination of gene essentiality is given for each gene in a single
cell line. A lower score means that a gene is more likely to be
dependent in a given cell line. A core of -1 corresponds to the median
of all common essential genes, whereas a score of 0 is equivalent to a
gene that is not essential; a positive score indicates a gain in fitness
upon gene ablation and often identifies tumor supressors. The overall
goal is to identify all essentail genes in 2000 cell lines over the
5-year project period.

Essential gene data from Project Achilles were downloaded from the
DepMap portal at: [depmap.org](https://depmap.org/portal/download/). The
19Q3 release contains gene essentiality scores for 18334 across 625 cell
lines. To find patterns in gene dependencies across cell lines, we
generated a Pearson correlation matrix of all genes by all genes. These
data generated correlation values that matched values published on
[depmap.org](https://depmap.org), validating the first step in our
analysis.

Next, given some cells do not express all genes, we sought to remove
dependency scores for gene-cell line pairs that have an expression value
of zero under basal conditions. The [Cancer Cell Line
Encyclopedia](https://portals.broadinstitute.org/ccle/about) project is
a collaboration between the Broad Institute, and the Novartis Institutes
for Biomedical Research and its Genomics Institute of the Novartis
Research Foundation to conduct a detailed genetic and pharmacologic
characterization of a large panel of human cancer models. Across 1210
cell lines in this project, 16.4% of all gene expression values are
zero, confirming this notion.

![](methods_files/figure-markdown_strict/unnamed-chunk-2-1.png)

Of the 625 cell lines for which gene essentiality data is collected, 621
have genome-wide gene expression data. From these cell lines, we removed
dependency scores for genes from cell line that have a corresponding
gene expression value of zero. For some genes expressed in highly
specific cell types, this operation removed many dependency values.
Thus, we set a threshold of zeros, meaning that if a gene had fewer than
cell lines with dependency values, the correaltion pattern would be
dependent upon too few of associations to be meaningful and were
therefore removed.

![](methods_files/figure-markdown_strict/unnamed-chunk-3-1.png) The 1871
removed genes that had too few cells with expression and dependency
data.

<table>
<thead>
<tr class="header">
<th style="text-align: left;">x</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td style="text-align: left;">A3GALT2</td>
</tr>
<tr class="even">
<td style="text-align: left;">AADACL2</td>
</tr>
<tr class="odd">
<td style="text-align: left;">AADACL3</td>
</tr>
<tr class="even">
<td style="text-align: left;">AADACL4</td>
</tr>
<tr class="odd">
<td style="text-align: left;">ACCSL</td>
</tr>
<tr class="even">
<td style="text-align: left;">ACER1</td>
</tr>
<tr class="odd">
<td style="text-align: left;">ACOD1</td>
</tr>
<tr class="even">
<td style="text-align: left;">ACSM2A</td>
</tr>
<tr class="odd">
<td style="text-align: left;">ACSM2B</td>
</tr>
<tr class="even">
<td style="text-align: left;">ACSM4</td>
</tr>
<tr class="odd">
<td style="text-align: left;">ACTL7A</td>
</tr>
<tr class="even">
<td style="text-align: left;">ACTL9</td>
</tr>
<tr class="odd">
<td style="text-align: left;">ACTRT1</td>
</tr>
<tr class="even">
<td style="text-align: left;">ACTRT2</td>
</tr>
<tr class="odd">
<td style="text-align: left;">ADAD1</td>
</tr>
<tr class="even">
<td style="text-align: left;">ADAM18</td>
</tr>
<tr class="odd">
<td style="text-align: left;">ADAM2</td>
</tr>
<tr class="even">
<td style="text-align: left;">ADAM29</td>
</tr>
<tr class="odd">
<td style="text-align: left;">ADAM30</td>
</tr>
<tr class="even">
<td style="text-align: left;">ADAM7</td>
</tr>
</tbody>
</table>

These ‘cleaned’ dependency data were then used to generate correlation
values between the remaining 16463 gene-gene pairs.

### Summary Statistics

### Gene Query for TP53

Distribution of dependency scores across 625 cell lines

dep\_plot2 dep\_plot1

### Cells with strong TP53 genetic dependencies

    #knitr::kable(target_achilles_bottom)

### Cells with low or inverse TP53 genetic dependencies

    #knitr::kable(target_achilles_top)

### Positive Ranked Dependency from DepMap

r paste0(“The”, length(dep\_top$gene), “genes that show the highest
postive correlation for similar genetic dependencies are listed here.”)

    #knitr::kable(dep_top)

### Positive Correlated Gene Sets

    #flat_top_complete %>% 
    #  select(enrichr, Term, Overlap) %>% 
    #  slice(1:10) %>% 
    #  knitr::kable()

### Negative Ranked Dependency from DepMap

r paste0(“The”, length(dep\_bottom$gene), " ranked genes that show the
most negative correlation for similar genetic dependencies are listed
here.")

    #knitr::kable(dep_bottom)

### Negative Correlated Gene Sets

    #flat_bottom_complete %>% 
    #  select(enrichr, Term, Overlap) %>% 
    #  slice(1:10) %>% 
    #  knitr::kable()

Code Availability
-----------------

[Generate Data](https://github.com/hirscheylab/depmap/tree/master/code)
[Statistical
Analyses](https://github.com/hirscheylab/depmap/tree/master/code)
[Pathway
Generator](https://github.com/hirscheylab/depmap/tree/master/code)

Select References
-----------------

Aviad Tsherniak, Francisca Vazquez, Phillip G. Montgomery, Barbara A.
Weir, … Gregory Kryukov, Glenn S. Cowley, Stanley Gill, William F.
Harrington, Sasha Pantel, John M. Krill-Burger, Robin M. Meyers, Levi
Ali, Amy Goodale, Yenarae Lee, Guozhi Jiang, Jessica Hsiao, William F.
J. Gerath, Sara Howell, Erin Merkel, Mahmoud Ghandi, Levi A. Garraway,
David E. Root, Todd R. Golub, Jesse S. Boehm, & William C. Hahn.
Defining a Cancer Dependency Map. Cell July 27, 2017. DOI:
j.cell.2017.06.010  
Andrew J. Aguirre, Robin M. Meyers, Barbara A. Weir, Francisca Vazquez,
… Cheng-Zhong Zhang, Uri Ben-David, April Cook, Gavin Ha, William F.
Harrington, Mihir B. Doshi, Maria Kost-Alimova, Stanley Gill, Han Xu,
Levi D. Ali, Guozhi Jiang, Sasha Pantel, Yenarae Lee, Amy Goodale,
Andrew D. Cherniack, Coyin Oh, Gregory Kryukov, Glenn S. Cowley, Levi A.
Garraway, Kimberly Stegmaier, Charles W. Roberts, Todd R. Golub, Matthew
Meyerson, David E. Root, Aviad Tsherniak, & William C. Hahn. Genomic
Copy Number Dictates a Gene-Independent Cell Response to CRISPR/Cas9
Targeting. Cancer Discovery 6, 914-929. June 3, 2016. Glenn S. Cowley,
Barbara A. Weir, Francisca Vazquez, Pablo Tamayo, … Justine A. Scott,
Scott Rusin, Alexandra East-Seletsky, Levi D. Ali, William F.J. Gerath,
Sarah E. Pantel, Patrick H. Lizotte, Guozhi Jiang, Jessica Hsiao, Aviad
Tsherniak, Elizabeth Dwinell, Simon Aoyama, Michael Okamoto, William
Harrington, Ellen Gelfand, Thomas M. Green, Mark J. Tomko, Shuba Gopal,
Terence C. Wong, Hubo Li, Sara Howell, Nicolas Stransky, Ted Liefeld,
Dongkeun Jang, Jonathan Bistline, Barbara Hill Meyers, Scott A.
Armstrong, Ken C. Anderson, Kimberly Stegmaier, Michael Reich, David
Pellman, Jesse S. Boehm, Jill P. Mesirov, Todd R. Golub, David E. Root,
& William C. Hahn. Parallel genome-scale loss of function screens in 216
cancer cell lines for the identification of context-specific genetic
dependencies. Nature Scientific Data 1, Article number: 140035.
September 30, 2014.  
Mehmet Gönen, Barbara A. Weir, Glenn S. Cowley, Francisca Vazquez, …
Yuanfang Guan, Alok Jaiswal, Masayuki Karasuyama, Vladislav Uzunangelov,
Tao Wang, Aviad Tsherniak, Sara Howell, Daniel Marbach, Bruce Hoff, Thea
C. Norman, Antti Airola, Adrian Bivol, Kerstin Bunte, Daniel Carlin,2
Sahil Chopra, Alden Deran, Kyle Ellrott, Peddinti Gopalacharyulu, Kiley
Graim, Samuel Kaski, Suleiman A. Khan, Yulia Newton, Sam Ng, Tapio
Pahikkala, Evan Paull, Artem Sokolov, Hao Tang,1 Jing Tang, Krister
Wennerberg, Yang Xie, Xiaowei Zhan, Fan Zhu, Broad-DREAM Community, Tero
Aittokallio, Hiroshi Mamitsuka, Joshua M. Stuart, Jesse S. Boehm, David
E. Root, Guanghua Xiao, Gustavo Stolovitzky, William C. Hahn, & Adam A.
Margolin. A Community Challenge for Inferring Genetic Predictors of Gene
Essentialities through Analysis of a Functional Screen of Cancer Cell
Lines. Cell Syst. 2017 Nov 22;5(5):485-497.e3. doi:
10.1016/j.cels.2017.09.004. Epub 2017 Oct 4.  
Xiaoyang Zhang, Peter S. Choi, Joshua M. Francis, Galen F. Gao, … Joshua
D. Campbell, Aruna Ramachandran, Yoichiro Mitsuishi, Gavin Ha, Juliann
Shih, Francisca Vazquez, Aviad Tsherniak, Alison M. Taylor, Jin Zhou,
Zhong Wu, Ashton C. Berger, Marios Giannakis, William C. Hahn, Andrew D.
Cherniack, & Matthew Meyerson. Somatic super-enhancer duplications and
hotspot mutations lead to oncogenic activation of the KLF5 transcription
factor. Cancer Discov September 29 2017 DOI:
10.1158/2159-8290.CD-17-0532  
Hubo Li, Brenton G. Mar, Huadi Zhang, Rishi V. Puram, Francisca Vazquez,
Barbara A. Weir, William C. Hahn, Benjamin Ebert & David Pellman. The
EMT regulator ZEB2 is a novel dependency of human and murine acute
myeloid leukemia. Blood 2017 Jan 26;129(4):497-508. doi:
10.1182/blood-2016-05-714493. Epub 2016 Oct 18.  
Brenton R. Paolella, William J. Gibson, Laura M. Urbanski, John A.
Alberta, … Travis I. Zack, Pratiti Bandopadhayay, Caitlin A. Nichols,
Pankaj K. Agarwalla, Meredith S. Brown, Rebecca Lamothe, Yong Yu, Peter
S. Choi, Esther A. Obeng, Dirk Heckl, Guo Wei, Belinda Wang, Aviad
Tsherniak, Francisca Vazquez, Barbara A. Weir, David E. Root, Glenn S.
Cowley, Sara J. Buhrlage, Charles D. Stiles, Benjamin L. Ebert, William
C. Hahn, Robin Reed, & Rameen Beroukhim. Copy-number and gene dependency
analysis reveals partial copy loss of wild-type SF3B1 as a novel cancer
vulnerability. Elife 2017 Feb 8;6. pii: e23268. doi:
10.7554/eLife.23268.  
Jong Wook Kim, Olga B. Botvinnik, Omar Abudayyeh, Chet Birger, … Joseph
Rosenbluh, Yashaswi Shrestha, Mohamed E. Abazeed, Peter S. Hammerman,
Daniel DiCara, David J. Konieczkowski, Cory M. Johannessen, Arthur
Liberzon, Amir Reza Alizad-Rahvar, Gabriela Alexe, Andrew Aguirre,
Mahmoud Ghandi, Heidi Greulich, Francisca Vazquez, Barbara A. Weir,
Eliezer M. Van Allen, Aviad Tsherniak, Diane D. Shao, Travis I. Zack,
Michael Noble, Gad Getz, Rameen Beroukhim, Levi A. Garraway, Masoud
Ardakani, Chiara Romualdi, Gabriele Sales, David A. Barbie, Jesse S.
Boehm, William C. Hahn, Jill P. Mesirov, & Pablo Tamayo. Characterizing
genomic alterations in cancer by complementary functional associations.
Nature Biotechnology 2016 May;34(5):539-46. doi: 10.1038/nbt.3527. Epub
2016 Apr 18.  
Gregory V. Kryukov, Frederick H. Wilson, Jason R. Ruth, Joshiawa Paulk,
… Aviad Tsherniak, Sara E. Marlow, Francisca Vazquez, Barbara A. Weir,
Mark E. Fitzgerald, Minoru Tanaka, Craig M. Bielski, Justin M. Scott,
Courtney Dennis, Glenn S. Cowley, Jesse S. Boehm, David E. Root, Todd R.
Golub, Clary B. Clish, James E. Bradner, William C. Hahn, & Levi A.
Garraway. MTAP deletion confers enhanced dependency on the PRMT5
arginine methyltransferase in cancer cells. Science 2016 Mar
11;351(6278):1214-8. doi: 10.1126/science.aad5214. Epub 2016 Feb 11.  
Hugh S. Gannon, Nathan Kaplan, Aviad Tsherniak, Francisca Vazquez,
Barbara A. Weir, William C. Hahn & Matthew Meyerson. Identification of
an “Exceptional Responder” Cell Line to MEK1 Inhibition: Clinical
Implications for MEK-Targeted Therapy. Molecular Cancer Research 2016
Feb;14(2):207-15. doi: 10.1158/1541-7786.MCR-15-0321. Epub 2015 Nov 18.
PMCID: PMC4755909.  
Kimberly H. Kim, Woojin Kim, Thomas P. Howard, Francisca Vazquez, …
Aviad Tsherniak, Jennifer N. Wu, Weishan Wang, Jeffrey R. Haswell, Loren
D. Walensky, William C. Hahn, Stuart H. Orkin, & Charles W. M.
Roberts.SWI/SNF-mutant cancers depend on catalytic and non-catalytic
activity of EZH2. Nature Medicine 2015 Dec;21(12):1491-6. doi:
10.1038/nm.3968. Epub 2015 Nov 9.  
Mark M. Pomerantz, Fugen Li, David Y. Takeda, Romina Lenci, … Apurva
Chonkar, Matthew Chabot, Paloma Cejas, Francisca Vazquez, Jennifer Cook,
Ramesh A. Shivdasani, Michaela Bowden, Rosina Lis, William C Hahn,
Philip W. Kantoff, Myles Brown, Massimo Loda, Henry W. Long, & Matthew
L. Freedman. The androgen receptor cistrome is extensively reprogrammed
in human prostate tumorigenesis. Nature Genetics 2015
Nov;47(11):1346-51. doi: 10.1038/ng.3419. Epub 2015 Oct 12. PMCID:
PMC4707683.

Methods updated November 29, 2019