# Information on vcf_tab_to_fasta_alignment.pl

## Quick Start Instructions

This is a tiny script to take all the SNPs in a VCF tabular file and concatenate them into a FASTA alignment. The tabular input file is created using the [VCFtools](http://vcftools.sourceforge.net/) utility [vcf-to-tab](http://vcftools.sourceforge.net/perl_module.html#vcf-to-tab):

	zcat input.vcf.gz | vcf-to-tab > snps.tab

This will make a file that looks like this:

	$ head input.vcf.tab
	chr10	94051	C	./	./	./	./	./	T/T
	chr10	94056	T	./	./	./	./	./	C/C
	chr10	94180	G	./	A/A	./	./	./	./

Hopefully with less missing data. Then you can convert this into a FASTA alignment with:

	perl vcf_tab_to_fasta_alignment.pl snps.tab > all_snps.fasta

