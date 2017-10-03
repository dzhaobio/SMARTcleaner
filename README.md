<h1>SMARTcleaner</h1>

<h2>Introduction</h2>
<p>SMARTcleaner is designed to specifically clean the falsely amplified noise from DNA SMART ChIP-seq libraries. There are two modes of this software, PE mode and SE mode, depending on the sequencing method used. In PE mode, it accepts genome fasta file and bam files as input, and output cleaned bam files and noise bam files. In SE mode, it accepts bed format of genomic poly(dT/dA) regions and bam files, and output cleaned bam files and noise bam files. SMARTcleaner also provides helper functions to prepare the files required for the cleaning process.</p>

<h2>Dependencies</h2>
<ul>
<li><a href="http://www.htslib.org/" target="_blank"> Samtools </a></li>
<li><a href="https://www.perl.org/" target="_blank"> Perl </a></li>
</ul>

<h2>Usage</h2>
<p>SMARTcleaner &lt sub-command &gt [options]</p>

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

<h2>Clean PE alignment files</h2>
<p>SMARTcleaner cleanPEbam [options] &lt genome &gt &lt pe.bam &gt</p>
<p>Options:
  <p>-o    DIR    output results to DIR [./]</p>
  <p>-g    NUM    the gap size in bp between read2 and polyA/T [0]</p>
</p>
<h2>Clean SE alignment files</h2>

<h2>Identify genomic poly(dT/dA) regions</h2>

<h2>Estimate falsely primed fragment length</h2>

<h2>Benchmarks</h2>
