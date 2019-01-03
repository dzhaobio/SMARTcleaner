<h1>SMARTcleaner</h1>
<ul>
<li><a href="#Introduction">Introduction</a></li>
<li><a href="#Dependencies">Dependencies</a></li>
<li><a href="#Installation">Installation</a></li>
<li><a href="#Usage">Usage</a></li>
<ul>
<li><a href="#PE">Clean PE alignment files</a></li>
<li><a href="#SE">Clean SE alignment files</a></li>
<li><a href="#poly">Identify genomic poly(T/A) regions</a></li>
<li><a href="#stitch">Stitch short polyN regions into longer interrupted polyN regions</a></li>
<li><a href="#frag">Estimate falsely primed fragment length</a></li>
</ul>
<li><a href="#Benchmarks">Benchmarks</a></li>
<li><a href="#Reference">Reference</a></li>
</ul>

<h2><a name="Introduction">Introduction</a></h2>
<p>SMARTcleaner is designed to specifically clean the falsely amplified noise from DNA SMART ChIP-seq libraries. There are two modes of this software, PE mode and SE mode, depending on the sequencing method used. In PE mode, it accepts genome fasta file and bam files as input, and output cleaned bam files and noise bam files. In SE mode, it accepts bed format of genomic poly(T/A) regions and bam files, and output cleaned bam files and noise bam files. SMARTcleaner also provides helper functions to prepare the files required for the cleaning process.</p>

<h2><a name="Dependencies">Dependencies<a></h2>
<ul>
<li><a href="http://www.htslib.org/" target="_blank"> Samtools v0.1.19 (or higher)</a></li>
<li><a href="https://www.perl.org/" target="_blank"> Perl v5.10 (or higher) </a></li>
</ul>

<h2><a name="Installation">Installation</a></h2>
<p>SMARTcleaner is easy to install and use. First, make sure samtools is availabe in your current working environment. If not, edit the evionronment variable, eg. $PATH in Linux system, to include samtools. Next, download SMARTcleaner and put it under the directory "~/bin". Then, run "chmod 755 ~/bin/SMARTcleaner". Now SMARTcleaner should be ready for use.</p>

<h2><a name="Usage">Usage</a></h2>
<p><b>Usange</b>: SMARTcleaner &ltsub-command&gt [options]</p>
<table>
  <tr>
    <th>Sub-command</th>
    <th>Function</th>
  </tr>
  <tr>
    <td>cleanPEbam</td>
    <td>clean PE bam files</td>
  </tr>
  <tr>
    <td>cleanSEbam</td>
    <td>clean SE bam files</td>
  </tr>
  <tr>
    <td>identifyGenomicPolyN</td>
    <td>generate genomic polyN region</td>
  </tr>
  <tr>
    <td>stitchShortPolyN</td>
    <td>stitch short polyN regions into longer interrupted polyN regions</td>
  </tr>
  <tr>
    <td>estimateFragLength</td>
    <td>estimate falsely primed fragment length</td>
  </tr>
</table>
<p>For more details, please refer to <a href="https://github.com/dzhaobio/SMARTcleaner/wiki">wiki pages</a>. </p>

<h3><a name="PE">Clean PE alignment files</a></h3>
<p><b>Usange</b>: SMARTcleaner cleanPEbam [options] &ltgenome&gt &ltpe.bam&gt</p>
<p>&ltgenome&gt: genome file in fasta format</p>
<p>&ltpe.bam&gt: alignment file for PE reads</p>
<p><b>Options</b>:</p>
  <p>-o &nbsp&nbsp DIR &nbsp&nbsp output results to DIR [./]</p>
  <p>-g &nbsp&nbsp NUM &nbsp&nbsp the gap size in bp between read2 and polyA/T [0]</p>
  <p>-l &nbsp&nbsp NUM &nbsp&nbsp the genomic sequence length for checking polyA/T sites [12]</p>
  <p>-c &nbsp&nbsp NUM &nbsp&nbsp the cutoff number of A/T bases for defining polyA/T sites [10]</p>
<p><b>Output</b>:</p>
<p>Two alignment files: one for clean signal, one for noise</p>
<p><b>Attention</b>:</p>
<p>For PE mode, the program will read the whole genome into memory for fast query. The minial memory will vary depending on the genome size.</p>

<h3><a name="SE">Clean SE alignment files</a></h3>
<p><b>Usange</b>: SMARTcleaner cleanSEbam [options] &ltbed&gt &ltse.bam&gt</p>
<p>&ltbed&gt: genomic poly(T/A) regions in bed format</p>
<p>&ltse.bam&gt: alignment file for SE reads (sorted by coordinates)</p>
<p><b>Options</b>:</p>
  <p>-o &nbsp&nbsp DIR &nbsp&nbsp output results to DIR [./]</p>
  <p>-l &nbsp&nbsp NUM &nbsp&nbsp region size for resampling falsely primed fragments [null]</p>
<p><b>Output</b>:</p>
<p>Two alignment files: one for clean signal, one for noise</p>
<p><b>Attention</b>:</p>
<p>The genomic poly(T/A) regions for human, mouse and yeast are prebuilt and put in the "data" folder. For other genomes, please refer to <a href="https://github.com/dzhaobio/SMARTcleaner/wiki">wiki pages</a> for how to prepare the required files.</p>

<h3><a name="poly">Identify genomic poly(T/A) regions</a></h3>
<p><b>Usange</b>: SMARTcleaner identifyGenomicPolyN &ltgenome&gt [N]</p>
<p>&ltgenome&gt: genome/chromosome file in fasta format</p>
<p>[N]: the number of consecutive bases in <fasta>, default 5</p>
<p><b>Output</b>:</p>
<p>1-based bed file. There are 6 columns: chr, start, end, name (indicating "PolyT" or "PolyA" region), region size, strand. The first four columns are essential.</p>

<h3><a name="stitch">Stitch short polyN regions into longer interrupted polyN regions</a></h3>
<p><b>Usange</b>: SMARTcleaner stitchShortPolyN &ltgenome&gt &ltclosestPolyN&gt</p>
<p>&ltgenome&gt: genome/chromosome file in fasta format</p>
<p>&ltclosestPolyN&gt: closest polyN regions and distance. </p>
<p><b>Output</b>:</p>
<p>Two files: one containing the interrupted poly(T/A) regions, ready for cleaning in SE mode; the other containing the details of poly(T/A) regions that are stitiched together. Refer to <a href="https://github.com/dzhaobio/SMARTcleaner/wiki">wiki pages</a> for details.</p>

<h3><a name="frag">Estimate falsely primed fragment length</a></h3>
<p><b>Usange</b>: SMARTcleaner estimateFragLength [options] &ltbed&gt &ltbam&gt</p>
<p>&ltbed&gt: genomic poly(T/A) regions in bed format</p>
<p>&ltbam&gt: alignment file for PE/SE reads</p>
<p><b>Options</b>:</p>
  <p>-o &nbsp&nbsp DIR &nbsp&nbsp output results to DIR [./]</p>
  <p>-f &nbsp&nbsp NUM &nbsp&nbsp flanking region size (bp) around poly(dT/dA) for estimating fragment length [2000]</p>
<p><b>Output</b>:</p>
<p>One R script and one figure generated from the R script for fragment length distribution near poly(T/A) regions.</p>
<p><b>Attention</b>:</p>
<p>If the figure was not generated automiatically, users can generate it by running the R script. There are long lines in the R script. Avoid opening and running the script file in RStudio, or the long lines will be truncated and result in errors. Instead, run "source("script.R")" in R command console, or run "Rscript script.R" on command line console.</p>

<h2><a name="Benchmarks">Benchmarks</a></h2>

<h2><a name="Reference">Reference</a></h2>
Dejian Zhao, Deyou Zheng. (2018). "SMARTcleaner: identify and clean off-target signals in SMART ChIP-seq analysis." BMC Bioinformatics 19(1): 544.
