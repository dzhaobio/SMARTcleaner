<h1>SMARTcleaner</h1>

<h2>Introduction</h2>
<p>SMARTcleaner is designed to specifically clean the falsely amplified noise from DNA SMART ChIP-seq libraries. There are two modes of this software, PE mode and SE mode, depending on the sequencing method used. In PE mode, it accepts genome fasta file and bam files as input, and output cleaned bam files and noise bam files. In SE mode, it accepts bed format of genomic poly(dT/dA) regions and bam files, and output cleaned bam files and noise bam files. SMARTcleaner also provides helper functions to prepare the files required for the cleaning process.</p>

<h2>Dependencies</h2>
<ul>
<li><a href="http://www.htslib.org/" target="_blank"> Samtools </a></li>
<li><a href="https://www.perl.org/" target="_blank"> Perl </a></li>
</ul>

<h2>Usage</h2>
<p>Usange: SMARTcleaner &ltsub-command&gt [options]</p>
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
    <td>estimateFragLength</td>
    <td>estimate falsely primed fragment length</td>
  </tr>
</table>

<h3>Clean PE alignment files</h3>
<p>Usange: SMARTcleaner cleanPEbam [options] &ltgenome&gt &ltpe.bam&gt</p>
<p>&ltgenome&gt: genome file in fasta format</p>
<p>&ltpe.bam&gt: alignment file for PE reads</p>
<p>Options:
  <p>-o &nbsp&nbsp DIR &nbsp&nbsp output results to DIR [./]</p>
  <p>-g &nbsp&nbsp NUM &nbsp&nbsp the gap size in bp between read2 and polyA/T [0]</p>
</p>

<h3>Clean SE alignment files</h3>
<p>Usange: SMARTcleaner cleanSEbam [options] &ltbed&gt &ltse.bam&gt</p>
<p>&ltbed&gt: genomic poly(dT/dA) regions in bed format[more details]</p>
<p>&ltse.bam&gt: alignment file for SE reads</p>
<p>Options:
  <p>-o &nbsp&nbsp DIR &nbsp&nbsp output results to DIR [./]</p>
  <p>-l &nbsp&nbsp NUM &nbsp&nbsp falsely primed fragment length, estimated from data by default [null]</p>
  <p>-r &nbsp&nbsp STR &nbsp&nbsp method to resample reads near polyA or polyT regions ("opposite", "max") [opposite]</p>
</p>

<h3>Identify genomic poly(dT/dA) regions</h3>
<p>Usange: SMARTcleaner identifyGenomicPolyN &ltgenome&gt [N]</p>
<p>&ltgenome&gt: genome/chromosome file in fasta format</p>
<p>[N]: the number of consecutive bases in <fasta>, default 12</p>
<p>Recomendation: run this analysis separately on each chromosome, and combine the results later<p>

<h3>Estimate falsely primed fragment length</h3>
<p>Usange: SMARTcleaner estimateFragLength [options] &ltbed&gt &ltbam&gt</p>
<p>&ltbed&gt: genomic poly(dT/dA) regions in bed format[more details]</p>
<p>&ltbam&gt: alignment file for PE/SE reads</p>
<p>Options:
  <p>-o &nbsp&nbsp DIR &nbsp&nbsp output results to DIR [./]</p>
</p>

<h2>Benchmarks</h2>
