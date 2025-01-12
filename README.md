# LongPhase
Long-read phasing has been used for reconstructing diploid genomes, improving variant calling, and resolving microbial strains in metagenomics.

---
### Installation LongPhase
```
git clone --recurse-submodules https://github.com/twolinin/LongPhase.git
cd LongPhase
bash setup.sh
```

---
### Usage
SNP phasing parameters
```
LongPhase phase \
-s SNP.vcf \
-b alignment.bam \
-r reference.fasta \
-t 4 \ # number of thread  
-o output_prefix \
--ont # or --pb
```

SNP and SV co-phasing parameters
```
LongPhase phase \
-s SNP.vcf \
--sv-file SV.vcf \
-b alignment.bam \
-r reference.fasta \
-t 4 \ # number of thread
-o output_prefix \
--ont # or --pb
```

| Parameter | Description | Default |
| :------------ |:---------------|-------------:|
|--help|display this help and exit.||
|--dot|each contig/chromosome will generate dot file.||
|--ont|Using Oxford Nanopore genomic reads.||
|--pb|Using PacBio HiFi/CCS genomic reads.||
|--sv-file=Name|input SV vcf file. ||
|-s, --snp-file=Name|input SNP vcf file.||
|-b, --bam-file=Name|input bam file.||
|-o, --out-prefix=Name|prefix of phasing result.|result|
|-r, --reference=Name|reference fasta.||
|-t, --threads=Num|number of thread.|1|
|-d, --distance=Num|phasing two variant if distance less than threshold.|300000|

---
### SNP phasing Result
From the results of genotype(GT) and phase set identifier(PS) below, we can see that the two haplotypes are CCCCC and GATGT.

```
##FORMAT=<ID=GT,Number=1,Type=String,Description="Genotype">
##FORMAT=<ID=PS,Number=1,Type=Integer,Description="Phase set identifier">
#CHROM  POS     ID      REF     ALT     QUAL    FILTER  INFO    FORMAT  default
1       16809   .       C       G       8.4     PASS    .       GT:GQ:DP:AD:VAF:PL:PS   1|0:8:79:51,28:0.35443:7,0,16:16809
1       16949   .       A       C       8.4     PASS    .       GT:GQ:DP:AD:VAF:PL:PS   0|1:8:67:43,21:0.313433:7,0,25:16809
1       21580   .       C       T       13.9    PASS    .       GT:GQ:DP:AD:VAF:PL:PS   1|0:14:75:50,24:0.32:13,0,30:16809
1       23359   .       C       G       13.2    PASS    .       GT:GQ:DP:AD:VAF:PL:PS   1|0:13:52:24,18:0.346154:13,0,42:16809
1       24132   .       C       T       11.1    PASS    .       GT:GQ:DP:AD:VAF:PL:PS   1|0:11:63:41,17:0.269841:10,0,29:16809
```

---
### SNP and SV co-phasing Result
From the results of genotype(GT) and phase set identifier(PS) below, we can see that the two haplotypes are A\<INS\>G\<noSV\>TCC and G\<noSV\>A\<INS\>ATT.

SNP file
```
##FORMAT=<ID=GT,Number=1,Type=String,Description="Genotype">
##FORMAT=<ID=PS,Number=1,Type=Integer,Description="Phase set identifier">
#CHROM  POS     ID      REF     ALT     QUAL    FILTER  INFO    FORMAT  default
1       465289  .       A       G       34.2    PASS    .       GT:GQ:DP:AD:VAF:PL:PS   1|0:4:16:2,11:0.6875:31,0,1:382189
1       544890  .       G       A       3.1     PASS    .       GT:GQ:DP:AD:VAF:PL:PS   1|0:3:31:30,0:0:0,0,31:382189
1       545612  .       T       A       6.8     PASS    .       GT:GQ:DP:AD:VAF:PL:PS   1|0:7:37:28,8:0.216216:5,0,32:382189
1       545653  .       C       T       14      PASS    .       GT:GQ:DP:AD:VAF:PL:PS   1|0:14:44:30,14:0.318182:13,0,26:382189
1       561458  .       C       T       5.1     PASS    .       GT:GQ:DP:AD:VAF:PL:PS   1|0:5:17:14,0:0:3,0,17:382189
```
SV file
```
##FORMAT=<ID=GT,Number=1,Type=String,Description="Genotype">
##FORMAT=<ID=PS,Number=1,Type=Integer,Description="Phase set identifier">
#CHROM  POS     ID      REF     ALT     QUAL    FILTER  INFO    FORMAT  default
1       534057  7       N       AAATTCGCGCATATCACGGGTGCCGCCTCTGTGCAGCTCACGAAACGCCATACTACGGTGCTGCTCAGCAGCTACGGAATCGCTATACCTACGCGAGCTGCCTCAGCAGCCAC       .       PASS    IMPRECISE;SVMETHOD=Snifflesv1.0.11;CHR2=1;END=534128;STD_quant_start=25.369041;STD_quant_stop=29.191054;Kurtosis_quant_start=-0.239012;Kurtosis_quant_stop=-1.333733;SVTYPE=INS;RNAMES=0120d560-50f0-4298-8b03-7bd30f3cf139,030ac5d4-e616-4ce9-8ad3-243835335085,0cf1b0d9-2b4d-463d-a658-01b4b040dc63,1c9e982a-8af7-4ba0-8cc2-154a679c72e2,22e11f79-0067-4735-8b69-97d951ca702f,2ca8a6f4-be9d-4df5-80d2-dc1743f97a84,35dff960-22b6-4216-af69-8878b8860362,390d2fb4-9224-41a1-a9fe-6cb3bbe4273a,3e333422-12ca-4f16-afb8-ed7611dcbc2c,3e8ed78a-b857-4941-bbc1-52ca51e26c08,4191371c-49ea-466d-aadc-06f27cdf1050,4aaae789-54fe-4fa5-84b3-5524dc2b3796,581e0cfb-2491-44d7-a2e1-ba1516ba0f2f,59749531-9abf-4ff4-a4a1-31484ba3d32d,5c97b0a9-925e-4153-952d-0f437171d3dc,6067590e-956c-442f-bbb7-cae597d616ad,623804bb-e2fe-415d-96ae-3d06aec63e5d,672244ce-2d5d-45cf-beb2-ddeddae917e8,6b79aa23-7c9c-49dc-9b88-8419c88c7a36,6e60d235-6654-4ef8-9feb-70f12a397721,6fbb55c5-57fc-43bc-8a24-b0058778054c,8e10bf13-9674-489c-924e-182a42e08a34,aa6ba092-4221-4d54-8819-811448c34983,af2169b3-b308-4db5-9675-15ff5f68d8dd,b214fcbd-77de-4dd5-84db-6d2b7e1f3158,c140eaba-e0e7-44e7-9f16-c8c67fd4a2f2,c7835cf7-44c0-44da-b10e-b2468fc8caab,ca4aa84d-34d1-4639-8634-b6a5540129ca,caba4bde-cdc5-4344-9803-a3c158525b0c,e0747feb-60bb-40db-a144-a9b43dd13256,e6992c7d-c00e-40e7-b80b-562094a9b60f,e8bb376c-20e0-4bed-a61f-b82b5c37ef6f,f3242a61-deec-49e7-b99f-335a1ba13791,f87dfdf7-7b68-421b-b395-3769a5fa3ac1,f91a7627-7fdb-4f03-8f33-0ed1649d96fe;SUPTYPE=AL;SVLEN=43;STRANDS=+-;RE=35;REF_strand=44;AF=0.795455    GT:DR:DV:PS     0|1:9:35:382189
1       545892  8       N       ACACGCGGGCCGTGGCCAGCAGGCGGCGCTGCAGGAGAGGAGATGCCCAGGCCTGGCGGCC   .       PASS    IMPRECISE;SVMETHOD=Snifflesv1.0.11;CHR2=1;END=545893;STD_quant_start=28.919840;STD_quant_stop=28.543200;Kurtosis_quant_start=-0.382251;Kurtosis_quant_stop=-0.130808;SVTYPE=INS;RNAMES=0120d560-50f0-4298-8b03-7bd30f3cf139,030ac5d4-e616-4ce9-8ad3-243835335085,0cf1b0d9-2b4d-463d-a658-01b4b040dc63,22e11f79-0067-4735-8b69-97d951ca702f,2ca8a6f4-be9d-4df5-80d2-dc1743f97a84,3977c988-9901-4e5b-9f9c-b8ebfcce8e93,3e333422-12ca-4f16-afb8-ed7611dcbc2c,4191371c-49ea-466d-aadc-06f27cdf1050,4aaae789-54fe-4fa5-84b3-5524dc2b3796,5933e1b7-1aeb-4437-a875-3befbf703420,623804bb-e2fe-415d-96ae-3d06aec63e5d,672244ce-2d5d-45cf-beb2-ddeddae917e8,6b79aa23-7c9c-49dc-9b88-8419c88c7a36,7842d9f1-9a77-4c9a-ab5b-5a644ed2d355,7ba26d64-d9b0-475f-8d5f-1fa73fc42d93,8e10bf13-9674-489c-924e-182a42e08a34,a2b1b2ef-1e28-465e-8b3f-c44e15990d8b,a45514f1-4aae-40eb-94eb-2969722a7b05,b8181546-6839-49cd-b64f-b65c96369a2b,c140eaba-e0e7-44e7-9f16-c8c67fd4a2f2,c7835cf7-44c0-44da-b10e-b2468fc8caab,ca4aa84d-34d1-4639-8634-b6a5540129ca,d56f0abe-4389-4197-a151-0eb567fb99f0,e6992c7d-c00e-40e7-b80b-562094a9b60f,e8bb376c-20e0-4bed-a61f-b82b5c37ef6f,ec325153-0c55-4ece-8f3c-c432701e6750,f3242a61-deec-49e7-b99f-335a1ba13791,f91a7627-7fdb-4f03-8f33-0ed1649d96fe;SUPTYPE=AL;SVLEN=62;STRANDS=+-;RE=28;REF_strand=51;AF=0.54902        GT:DR:DV:PS     1|0:23:28:382189
```

---
### Citation

---
### Contact



