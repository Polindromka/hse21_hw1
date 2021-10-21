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
