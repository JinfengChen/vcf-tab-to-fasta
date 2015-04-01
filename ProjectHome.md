# Information on vcf`_`tab`_`to`_`fasta`_`alignment.pl #

## Quick Start Instructions ##

Download script from [this Google Drive folder](https://drive.google.com/folderview?id=0B3JtKm_03RZDRmVPWk96ZEZHOHc&usp=sharing)

This is a tiny script to take all the SNPs in a VCF tabular file and concatenate them into a FASTA alignment. The tabular input file is created using the [VCFtools](http://vcftools.sourceforge.net/) utility [vcf-to-tab](http://vcftools.sourceforge.net/perl_module.html#vcf-to-tab):

```
zcat input.vcf.gz | vcf-to-tab > snps.tab
```

This will make a file that looks like this:

```
$ head input.vcf.tab
chr10   94051   C   ./  ./  ./  ./  ./  T/T
chr10   94056   T   ./  ./  ./  ./  ./  C/C
chr10   94180   G   ./  A/A ./  ./  ./  ./
```

Hopefully with less missing data. Then you can convert this into a FASTA alignment with:

```
perl vcf_tab_to_fasta_alignment.pl -i snps.tab > all_snps.fasta
```

Add the optional `--exclude_het` flag to exclude heterozygous sites:

```
perl vcf_tab_to_fasta_alignment.pl --exclude_het -i snps.tab > no_hets.fasta
```

Add the optional `--output_ref` flag to output the reference genome allele:

```
perl vcf_tab_to_fasta_alignment.pl --output_ref -i snps.tab > all_snps_with_ref.fasta
```


---


## Mini Example ##

First check out the script and associated data with this command:

```
svn checkout http://vcf-tab-to-fasta.googlecode.com/svn/trunk/ vcf-tab-to-fasta
```

This should download everything into a directory named vcf-tab-to-fasta. There are some small vcf-tab example files in the example`_`data directory, named chr22snps`_`head.tab and chrYsnps`_`head.tab. These were created using the [VCFtools](http://vcftools.sourceforge.net/) utility [vcf-to-tab](http://vcftools.sourceforge.net/perl_module.html#vcf-to-tab):

Once that's done, move into the directory:

```
cd vcf-tab-to-fasta
```

We can run the script to concatenate SNPs into a FASTA format file. First for chromosome 22:

```
perl vcf_tab_to_fasta_alignment.pl -i example_data/chr22snps_head.tab > chr22snps_head.fasta
```

And then for chromosome Y:

```
perl vcf_tab_to_fasta_alignment.pl -i example_data/chrYsnps_head.tab > chrYsnps_head.fasta
```

Now there should be two new FASTA files, chr22snps`_`head.fasta and chrYsnps`_`head.fasta. Let's make sure everthing worked OK. We can look at the 4th column of the chr22 tab data with this command:

```
awk '{print $4}' example_data/chr22snps_head.tab
```

The result is this:

```
HG00096
T/T
C/G
C/C
C/C
C/C
T/T
G/G
A/A
C/C
```

We can see that same individual, HG00096, in the FASTA file with this command:

```
grep -A1 "HG00096" chr22snps_head.fasta
```

The result is this. (Remember, S is the IUPAC code for G or C.)

```
>HG00096
TSCCCTGAC
```

Similarly, for the haploid Y data, the sixth column can be viewed with:

```
awk '{print $6}'  example_data/chrYsnps_head.tab
```

Which results in:

```
HG00103
G/
G/
./
C/
C/
./
A/
C/
G/
```

By looking at the same individual in the FASTA file...

```
grep -A1 "HG00103" chrYsnps_head.fasta
```

We see that the conversion worked well:

```
>HG00103
GG-CC-ACG
```


---


## Full Run Example - 1000 Genomes ##

First, download 1000 Genomes SNPs in VCF format for chr22

```
wget ftp://ftp.1000genomes.ebi.ac.uk/vol1/ftp/phase1/analysis_results/integrated_call_sets/ALL.chr22.integrated_phase1_v3.20101123.snps_indels_svs.genotypes.vcf.gz
wget ftp://ftp.1000genomes.ebi.ac.uk/vol1/ftp/phase1/analysis_results/integrated_call_sets/ALL.chr22.integrated_phase1_v3.20101123.snps_indels_svs.genotypes.vcf.gz.tbi
```

...and for chrY

```
wget ftp://ftp.1000genomes.ebi.ac.uk/vol1/ftp/phase1/analysis_results/integrated_call_sets/ALL.chrY.phase1_samtools_si.20101123.snps.low_coverage.genotypes.vcf.gz
wget ftp://ftp.1000genomes.ebi.ac.uk/vol1/ftp/phase1/analysis_results/integrated_call_sets/ALL.chrY.phase1_samtools_si.20101123.snps.low_coverage.genotypes.vcf.gz.tbi
```

Make a tabular VCF file with [VCFtools](http://vcftools.sourceforge.net/) utility [vcf-to-tab](http://vcftools.sourceforge.net/perl_module.html#vcf-to-tab) for chr22 and chrY.

```
zcat ALL.chr22.integrated_phase1_v3.20101123.snps_indels_svs.genotypes.vcf.gz | vcf-to-tab > chr22snps.tab
zcat ALL.chrY.phase1_samtools_si.20101123.snps.low_coverage.genotypes.vcf.gz  | vcf-to-tab > chrYsnps.tab
```

Download script from [this Google Drive folder](https://drive.google.com/folderview?id=0B3JtKm_03RZDRmVPWk96ZEZHOHc&usp=sharing)

Finally, concatenate SNPs into a FASTA format file with `vcf_tab_to_fasta_alignment`:

```
perl vcf_tab_to_fasta_alignment.pl -i chr22snps.tab > chr22snps.fasta
perl vcf_tab_to_fasta_alignment.pl -i chrYsnps.tab  > chrYsnps.fasta
```


---


## How to Cite ##

If you find the script useful in academic work, I'd appreciate your citing is as:

Bergey CM (2012). vcf-tab-to-fasta; http://code.google.com/p/vcf-tab-to-fasta

Here's the BibTeX entry:

```
@MISC{vcftabtofasta,
  author = "Christina Bergey",
  title = "vcf-tab-to-fasta",
  year = "2012",
  url = "http://code.google.com/p/vcf-tab-to-fasta"
}
```