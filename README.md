<h1>SMARTcleaner</h1>
<ul>
<li><a href="#Introduction">Introduction</a></li>
<li><a href="#Dependencies">Dependencies</a></li>
<li><a href="#Installation">Installation</a></li>
<li><a href="#Usage">Usage</a></li>
<ul>
<li><a href="#PE">Clean PE alignment files</a></li>
<li><a href="#SE">Clean SE alignment files</a></li>
<li><a href="#poly">Identify genomic poly(dT/dA) regions</a></li>
<li><a href="#stitch">Stitch short polyN regions into longer interrupted polyN regions</a></li>
<li><a href="#frag">Estimate falsely primed fragment length</a></li>
</ul>
<li><a href="#Benchmarks">Benchmarks</a></li>
<li><a href="#Reference">Reference</a></li>
</ul>

<h2><a name="Introduction">Introduction</a></h2>
<p>SMARTcleaner is designed to specifically clean the falsely amplified noise from DNA SMART ChIP-seq libraries. There are two modes of this software, PE mode and SE mode, depending on the sequencing method used. In PE mode, it accepts genome fasta file and bam files as input, and output cleaned bam files and noise bam files. In SE mode, it accepts bed format of genomic poly(dT/dA) regions and bam files, and output cleaned bam files and noise bam files. SMARTcleaner also provides helper functions to prepare the files required for the cleaning process.</p>

<h2><a name="Dependencies">Dependencies<a></h2>
<ul>
<li><a href="http://www.htslib.org/" target="_blank"> Samtools </a></li>
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
  <tr>
    <td>estimateFragLength</td>
    <td>estimate falsely primed fragment length</td>
  </tr>
</table>

<h3><a name="PE">Clean PE alignment files</a></h3>
<p><b>Usange</b>: SMARTcleaner cleanPEbam [options] &ltgenome&gt &ltpe.bam&gt</p>
<p>&ltgenome&gt: genome file in fasta format</p>
<p>&ltpe.bam&gt: alignment file for PE reads</p>
<p><b>Options</b>:</p>
  <p>-o &nbsp&nbsp DIR &nbsp&nbsp output results to DIR [./]</p>
  <p>-g &nbsp&nbsp NUM &nbsp&nbsp the gap size in bp between read2 and polyA/T [0]</p>
<p><b>Output</b>:</p>
<p>Two alignment files: one for clean signal, one for noise</p>

<h3><a name="SE">Clean SE alignment files</a></h3>
<p><b>Usange</b>: SMARTcleaner cleanSEbam [options] &ltbed&gt &ltse.bam&gt</p>
<p>&ltbed&gt: genomic poly(dT/dA) regions in bed format</p>
<p>&ltse.bam&gt: alignment file for SE reads</p>
<p><b>Options</b>:</p>
  <p>-o &nbsp&nbsp DIR &nbsp&nbsp output results to DIR [./]</p>
  <p>-l &nbsp&nbsp NUM &nbsp&nbsp falsely primed fragment length, estimated from data by default [null]</p>
  <p>-r &nbsp&nbsp STR &nbsp&nbsp method to resample reads near polyA or polyT regions ("opposite", "max") [opposite]</p>
<p><b>Output</b>:</p>
<p>Two alignment files: one for clean signal, one for noise</p>
<p><b>Attention</b>:</p>
<p>For SE mode, the program will read the whole genome into memory for fast query. The minial memory will vary depending on the genome size.</p>

<h3><a name="poly">Identify genomic poly(dT/dA) regions</a></h3>
<p><b>Usange</b>: SMARTcleaner identifyGenomicPolyN &ltgenome&gt [N]</p>
<p>&ltgenome&gt: genome/chromosome file in fasta format</p>
<p>[N]: the number of consecutive bases in <fasta>, default 12</p>
<p><b>Recomendation</b>: run this analysis separately on each chromosome, and combine the results later<p>
<p><b>Output</b>:</p>
<p>1-based bed file. There are 6 columns: chr, start, end, name (indicating "PolyT" or "PolyA" region), region size, strand. The first four columns are essential.</p>

<h3><a name="stitch">Stitch short polyN regions into longer interrupted polyN regions</a></h3>
<p><b>Usange</b>: SMARTcleaner stitchShortPolyN &ltgenome&gt &ltclosestPolyN&gt</p>
<p>&ltgenome&gt: genome/chromosome file in fasta format</p>
<p>&ltclosestPolyN&gt: closest polyN regions and distance. </p>
<p><b>Recomendation</b>: The closest polyN regions and distance could be generated using bedtools2. Refer to <a href="">wiki</a> for details.<p>
<p><b>Output</b>:</p>
<p>Two files: one containing the interrupted poly(dT/dA) regions, ready for cleaning in SE mode; the other containing the details of poly(dT/dA) regions that are stitiched together.</p>

<h3><a name="frag">Estimate falsely primed fragment length</a></h3>
<p><b>Usange</b>: SMARTcleaner estimateFragLength [options] &ltbed&gt &ltbam&gt</p>
<p>&ltbed&gt: genomic poly(dT/dA) regions in bed format[more details]</p>
<p>&ltbam&gt: alignment file for PE/SE reads</p>
<p><b>Options</b>:</p>
  <p>-o &nbsp&nbsp DIR &nbsp&nbsp output results to DIR [./]</p>
<p><b>Output</b>:</p>
<p>One R script and one figure generated from the R script for fragment length distribution near poly(dT/dA) regions.</p>
<p><b>Attention</b>:</p>
<p>If the figure was not generated automiatically, users can generate it by running the R script. There are long lines in the R script. Avoid opening and running the script file in RStudio, or the long lines will be truncated and result in errors. Instead, run "source("script.R")" in R command console, or run "Rscript script.R" on command line console.</p>

<h2><a name="Benchmarks">Benchmarks</a></h2>

<h2><a name="Reference">Reference</a></h2>
