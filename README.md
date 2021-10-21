# hse21_hw2
Аннотация собранного генома
1. Создание ссылок в личной папке:
```
 ls /usr/share/data-minor-bioinf/assembly/* | xargs -tI{} ln -s {}
```
 2. Выбираем случайные 5 млн. paired-end чтений и 1.5 млн. mate-pairs чтений
 ```
seqtk sample -s620 oil_R1.fastq 5000000 > R1_paired_end.fastq
seqtk sample -s620 oil_R2.fastq 5000000 > R2_paired_end.fastq
seqtk sample -s620 oilMP_S4_L001_R1_001.fastq 1500000 > R1_mate_pairs.fastq
seqtk sample -s620 oilMP_S4_L001_R2_001.fastq 1500000 > R2_mate_pairs.fastq
 ```
3. Оцениваем качество исходных чтений с помощью fastQC
```
ls R*.fastq | xargs -P 4 -tI{} fastqc -o fastqc {}
```
4. Собираем отчёт с помощью multiQC
```
 multiqc -o multiqc fastqc
```
5. Подрезаем чтения по качеству и удалям праймеры
```
platanus_trim R1_paired_end.fastq R2_paired_end.fastq
platanus_internal_trim R1_mate_pairs.fastq R2_mate_pairs.fastq
```
6. Оцениваем качество подрезанных данных с помощью fastQC
```
ls trimmed_fastq/* | xargs -P 4 -tI{} fastqc -o trimmed_fastqc {}
```
7. Собираем отчёт с помощью multiQC
```
multiqc -o trimmed_multiqc trimmed_fastqc
```
8. Собираем контиги из подрезанных чтений с помощью “platanus assemble”
```
time platanus assemble -o Poil -t 1 -m 8 -f trimmed_fastq/R1_paired_end.fastq.trimmed  trimmed_fastq/R2_paired_end.fastq.trimmed 2>assemble.log
```
9. Собираем скаффолды из контигов, а также из подрезанных чтений
```
time platanus scaffold -o Poil -t 1 -c Poil_contig.fa -IP1 trimmed_fastq/R1_paired_end.fastq.trimmed trimmed_fastq/R2_paired_end.fastq.trimmed -OP2 trimmed_fastq/R1_mate_pairs.fastq.int_trimmed trimmed_fastq/R2_mate_pairs.fastq.int_trimmed 2> scaffold.log
```
10. Выделим в отдельный файл самый длинный скаффолд
```
echo scaffold1_len3832152_cov231 > _tmp.txt
seqtk subseq Poil_scaffold.fa _tmp.txt > scaffold1_len3832152_cov231.fasta
```
