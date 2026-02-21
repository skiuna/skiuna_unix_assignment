#UNIX Assignment

#Samuel Kiuna

##Data Inspection

###Attributes of `fang_et_al_genotypes.txt`

```
$ wc fang_et_al_genotypes.txt 
$ du -h fang_et_al_genotypes.txt
$ head -n 1 fang_et_al_genotypes.txt
$ head fang_et_al_genotypes.txt
$ head -5 fang_et_al_genotypes.txt
$ awk -F "\t" ' {print NF;exit}' fang_et_al_genotypes.txt
$ head fang_et_al_genotypes.txt | sed 's/\t/[TAB]/g'
$ grep -c "NA" fang_et_al_genotypes.txt
$ grep -c "?" fang_et_al_genotypes.txt
$ cut -f3 fang_et_al_genotypes.txt | sort | uniq
```

By inspecting this file using the code chunks above, I learned the following about it:

1. Tab-delimited and 6.7M in size.
2. 2783 rows (2782 SNPs + 1 header).
3. 986 columns.
4. Rows represent SNP markers, columns represent individual genotypes, and metadata.
5. Missing genotype data encoded as `?/?`, and metadata missing values encoded as `NA`.
   
   



###Attributes of `snp_position.txt`

```
$ wc -l snp_position.txt
$ du -h snp_position.txt
$ cat snp_position.txt
$ head snp_position.txt
$ head -5 snp_position.txt
$ awk -F "\t" ' {print NF;exit}' snp_position.txt
$ tail snp_position.txt 
$ cut -f3 snp_position.txt | sort | uniq
$ head snp_position.txt | sed 's/\t/[TAB]/g'


```

By inspecting this file, I learned that:

1. It's also tab-delimited with a header.
2. Contains 984 SNPs.
3. It has 15 columns, including SNP id, chromosome, and nucleotide position.
4. This file is much smaller (49K), and also tab-delimited.
5. SNPs occur on chromosomes 1-10, with some assigned to unknown or multiple genomic positions.





##Data Processing

###Maize Data

```
$ head -n 1 fang_et_al_genotypes.txt > header.txt
$ awk '$3=="ZMMIL" || $3=="ZMMLR" || $3=="ZMMMR"' fang_et_al_genotypes.txt > maize_body.txt
$ cat header.txt maize_body.txt > maize_genotypes.txt
$ awk -f transpose.awk maize_genotypes.txt > maize_transposed.txt

$ tail -n +2 snp_position.txt > snp_noheader.txt
$ cut -f1,3,4 snp_noheader.txt > snp_clean.txt
$ sort -k1,1 snp_clean.txt > snp_sorted.txt
$ sort -k1,1 maize_transposed.txt > maize_sorted.txt
$ join snp_sorted.txt maize_sorted.txt > maize_joined.txt
$ awk '$2=="unknown"' maize_joined.txt > maize_unknown.txt
$ awk '$2=="multiple"' maize_joined.txt > maize_multiple.txt
$ awk '$2==1' maize_joined.txt | sort -k3,3n > maize_chr1_inc.txt
$ awk '$2==1' maize_joined.txt | sort -k3,3nr > maize_chr1_dec.txt
$ awk '$2==1' maize_joined.txt | sort -k3,3n > maize_chr1_inc.txt
$ awk '$2==1' maize_joined.txt | sort -k3,3nr > maize_chr1_dec.txt
$ awk '$2==2' maize_joined.txt | sort -k3,3n > maize_chr2_inc.txt
$ awk '$2==2' maize_joined.txt | sort -k3,3nr > maize_chr2_dec.txt
$ awk '$2==3' maize_joined.txt | sort -k3,3n > maize_chr3_inc.txt
$ awk '$2==3' maize_joined.txt | sort -k3,3nr > maize_chr3_dec.txt
$ awk '$2==4' maize_joined.txt | sort -k3,3n > maize_chr4_inc.txt
$ awk '$2==4' maize_joined.txt | sort -k3,3nr > maize_chr4_dec.txt
$ awk '$2==5' maize_joined.txt | sort -k3,3n > maize_chr5_inc.txt
$ awk '$2==5' maize_joined.txt | sort -k3,3nr > maize_chr5_dec.txt
$ awk '$2==6' maize_joined.txt | sort -k3,3n > maize_chr6_inc.txt
$ awk '$2==6' maize_joined.txt | sort -k3,3nr > maize_chr6_dec.txt
$ awk '$2==7' maize_joined.txt | sort -k3,3n > maize_chr7_inc.txt
$ awk '$2==7' maize_joined.txt | sort -k3,3nr > maize_chr7_dec.txt
$ awk '$2==8' maize_joined.txt | sort -k3,3n > maize_chr8_inc.txt
$ awk '$2==8' maize_joined.txt | sort -k3,3nr > maize_chr8_dec.txt
$ awk '$2==9' maize_joined.txt | sort -k3,3n > maize_chr9_inc.txt
$ awk '$2==9' maize_joined.txt | sort -k3,3nr > maize_chr9_dec.txt
$ awk '$2==10' maize_joined.txt | sort -k3,3n > maize_chr10_inc.txt
$ awk '$2==10' maize_joined.txt | sort -k3,3nr > maize_chr10_dec.txt
$ sed 's/?\/?/?/g' maize_chr1_inc.txt > maize_chr1_inc_fixed.txt
$ sed 's/?\/?/?/g' maize_chr2_inc.txt > maize_chr2_inc_fixed.txt
$ sed 's/?\/?/?/g' maize_chr3_inc.txt > maize_chr3_inc_fixed.txt
$ sed 's/?\/?/?/g' maize_chr4_inc.txt > maize_chr4_inc_fixed.txt
$ sed 's/?\/?/?/g' maize_chr5_inc.txt > maize_chr5_inc_fixed.txt
$ sed 's/?\/?/?/g' maize_chr6_inc.txt > maize_chr6_inc_fixed.txt
$ sed 's/?\/?/?/g' maize_chr7_inc.txt > maize_chr7_inc_fixed.txt
$ sed 's/?\/?/?/g' maize_chr8_inc.txt > maize_chr8_inc_fixed.txt
$ sed 's/?\/?/?/g' maize_chr9_inc.txt > maize_chr9_inc_fixed.txt
$ sed 's/?\/?/?/g' maize_chr10_inc.txt > maize_chr10_inc_fixed.txt
$ sed 's/?\/?/-/g' maize_chr1_dec.txt > maize_chr1_dec_fixed.txt
$ sed 's/?\/?/-/g' maize_chr2_dec.txt > maize_chr2_dec_fixed.txt
$ sed 's/?\/?/-/g' maize_chr3_dec.txt > maize_chr3_dec_fixed.txt
$ sed 's/?\/?/-/g' maize_chr4_dec.txt > maize_chr4_dec_fixed.txt
$ sed 's/?\/?/-/g' maize_chr5_dec.txt > maize_chr5_dec_fixed.txt
$ sed 's/?\/?/-/g' maize_chr6_dec.txt > maize_chr6_dec_fixed.txt
$ sed 's/?\/?/-/g' maize_chr7_dec.txt > maize_chr7_dec_fixed.txt
$ sed 's/?\/?/-/g' maize_chr8_dec.txt > maize_chr8_dec_fixed.txt
$ sed 's/?\/?/-/g' maize_chr9_dec.txt > maize_chr9_dec_fixed.txt
$ sed 's/?\/?/-/g' maize_chr10_dec.txt > maize_chr10_dec_fixed.txt
```

I followed these steps for data processing:

1. Split fang_et_al_genotypes file into maize and teosinte using `awk` on column 3.

2. Transposed the maize file using `transpose.awk` to match the snp_position file format.

3. Removed header from snp_position.txt.

4. Extracted SNP_ID, Chromosome, and Position using `cut`.

5. Sorted the files using `sort` and joined on SNP_ID using `join`.

6. Separated unknown and multiple SNPs.

7. Splited by chromosome using `awk` and sorted by increasing and decreasing position using `sort`.

8. Generated 22 output files. 1 for unknown positions, 1 for multiple positions, 10 for the ten chromosomes ordered based on increasing position values, and 10 for the ten chromosomes ordered based on decreasing position values.

9. Encoded the missing data ?/? with the symbol (?) for the increasing position values files and symbol (-) for the decreasing position files.





###Teosinte Data

```
$ awk '$3=="ZMPBA" || $3=="ZMPIL" || $3=="ZMPJA"' fang_et_al_genotypes.txt > teosinte_body.txt
$ cat header.txt teosinte_body.txt > teosinte_genotypes.txt
$ awk -f transpose.awk teosinte_genotypes.txt > teosinte_transposed.txt
$ sort -k1,1 teosinte_transposed.txt > teosinte_sorted.txt
$ join snp_sorted.txt teosinte_sorted.txt > teosinte_joined.txt
$ awk '$2=="unknown"' teosinte_joined.txt > teosinte_unknown.txt
$ awk '$2=="multiple"' teosinte_joined.txt > teosinte_multiple.txt
$ awk '$2==1' teosinte_joined.txt | sort -k3,3n > teosinte_chr1_inc.txt
$ awk '$2==1' teosinte_joined.txt | sort -k3,3nr > teosinte_chr1_dec.txt
$ awk '$2==2' teosinte_joined.txt | sort -k3,3n > teosinte_chr2_inc.txt
$ awk '$2==2' teosinte_joined.txt | sort -k3,3nr > teosinte_chr2_dec.txt
$ awk '$2==3' teosinte_joined.txt | sort -k3,3n > teosinte_chr3_inc.txt
$ awk '$2==3' teosinte_joined.txt | sort -k3,3nr > teosinte_chr3_dec.txt
$ awk '$2==4' teosinte_joined.txt | sort -k3,3n > teosinte_chr4_inc.txt
$ awk '$2==4' teosinte_joined.txt | sort -k3,3nr > teosinte_chr4_dec.txt
$ awk '$2==5' teosinte_joined.txt | sort -k3,3n > teosinte_chr5_inc.txt
$ awk '$2==5' teosinte_joined.txt | sort -k3,3nr > teosinte_chr5_dec.txt
$ awk '$2==6' teosinte_joined.txt | sort -k3,3n > teosinte_chr6_inc.txt
$ awk '$2==6' teosinte_joined.txt | sort -k3,3nr > teosinte_chr6_dec.txt
$ awk '$2==7' teosinte_joined.txt | sort -k3,3n > teosinte_chr7_inc.txt
$ awk '$2==7' teosinte_joined.txt | sort -k3,3nr > teosinte_chr7_dec.txt
$ awk '$2==8' teosinte_joined.txt | sort -k3,3n > teosinte_chr8_inc.txt
$ awk '$2==8' teosinte_joined.txt | sort -k3,3nr > teosinte_chr8_dec.txt
$ awk '$2==9' teosinte_joined.txt | sort -k3,3n > teosinte_chr9_inc.txt
$ awk '$2==9' teosinte_joined.txt | sort -k3,3nr > teosinte_chr9_dec.txt
$ awk '$2==10' teosinte_joined.txt | sort -k3,3n > teosinte_chr10_inc.txt
$ awk '$2==10' teosinte_joined.txt | sort -k3,3nr > teosinte_chr10_dec.txt
$ sed 's/?\/?/?/g' teosinte_chr1_inc.txt > teosinte_chr1_inc_fixed.txt
$ sed 's/?\/?/?/g' teosinte_chr2_inc.txt > teosinte_chr2_inc_fixed.txt
$ sed 's/?\/?/?/g' teosinte_chr3_inc.txt > teosinte_chr3_inc_fixed.txt
$ sed 's/?\/?/?/g' teosinte_chr4_inc.txt > teosinte_chr4_inc_fixed.txt
$ sed 's/?\/?/?/g' teosinte_chr5_inc.txt > teosinte_chr5_inc_fixed.txt
$ sed 's/?\/?/?/g' teosinte_chr6_inc.txt > teosinte_chr6_inc_fixed.txt
$ sed 's/?\/?/?/g' teosinte_chr7_inc.txt > teosinte_chr7_inc_fixed.txt
$ sed 's/?\/?/?/g' teosinte_chr8_inc.txt > teosinte_chr8_inc_fixed.txt
$ sed 's/?\/?/?/g' teosinte_chr9_inc.txt > teosinte_chr9_inc_fixed.txt
$ sed 's/?\/?/?/g' teosinte_chr10_inc.txt > teosinte_chr10_inc_fixed.txt
$ sed 's/?\/?/?/g' teosinte_chr1_dec.txt > teosinte_chr1_dec_fixed.txt
$ sed 's/?\/?/?/g' teosinte_chr2_dec.txt > teosinte_chr2_dec_fixed.txt
$ sed 's/?\/?/?/g' teosinte_chr3_dec.txt > teosinte_chr3_dec_fixed.txt
$ sed 's/?\/?/?/g' teosinte_chr4_dec.txt > teosinte_chr4_dec_fixed.txt
$ sed 's/?\/?/?/g' teosinte_chr5_dec.txt > teosinte_chr5_dec_fixed.txt
$ sed 's/?\/?/?/g' teosinte_chr6_dec.txt > teosinte_chr6_dec_fixed.txt
$ sed 's/?\/?/?/g' teosinte_chr7_dec.txt > teosinte_chr7_dec_fixed.txt
$ sed 's/?\/?/?/g' teosinte_chr8_dec.txt > teosinte_chr8_dec_fixed.txt
$ sed 's/?\/?/?/g' teosinte_chr9_dec.txt > teosinte_chr9_dec_fixed.txt
$ sed 's/?\/?/?/g' teosinte_chr10_dec.txt > teosinte_chr10_dec_fixed.txt
```

Followed the exact steps followed in the maize file part.

1. Split fang_et_al_genotypes file into maize and teosinte using `awk` on column 3.

2. Transposed the teosinte file using `transpose.awk` to match the snp_position file format.

3. Removed header from snp_position.txt.

4. Extracted SNP_ID, Chromosome, and Position using `cut`.

5. Sorted the files using `sort` and joined on SNP_ID using `join`.

6. Separated unknown and multiple SNPs.

7. Splited by chromosome using `awk` and sorted by increasing and decreasing position using `sort`.

8. Generated 22 output files. 1 for unknown positions, 1 for multiple positions, 10 for the ten chromosomes ordered based on increasing position values, and 10 for the ten chromosomes ordered based on decreasing position values.
   
9. Encoded the missing data ?/? with the symbol (?) for the increasing position values files and symbol (-) for the decreasing position files.

