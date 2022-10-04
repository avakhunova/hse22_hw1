# hse22_hw1
### 1. Выбор случайных чтений
```linux
seqtk sample -s1219 oil_R1.fastq 5000000 > sub1.fastq
seqtk sample -s1219 oil_R2.fastq 5000000 > sub2.fastq
seqtk sample -s1219 oilMP_S4_L001_R1_001.fastq 1500000 > mate1.fastq
seqtk sample -s1219 oilMP_S4_L001_R2_001.fastq 1500000 > mate2.fastq
```
### 2. Оценка и анализ чтений с использованием FastQC
```linux
mkdir fastqc
ls sub* mate* | xargs -tI{} fastqc -o fastqc {}
```
### 3. Вывод отчетов с помощью MultiQC
```linux
mkdir multiqc
multiqc -o multiqc fastqc
```
### 4. Обрезание чтений с помощью platanus![Uploading image.png…]()

```linux
platanus_trim sub*
platanus_internal_trim mate*
```
### 5. Удаление изначальных файлов
```linux
rm sub1.fastq sub2.fastq mate1.fastq mate2.fastq
```
### 6. Оценка качества обрезанных чтений с помощью FastQC
```linux
mkdir fastqc_trimmed
ls sub* matep*| xargs -tI{} fastqc -o fastqc_trimmed {}
```
### 7. Создание отчета для обрезанных чтений с помощью MultiQC
```linux
mkdir multiqc_trimmed
multiqc -o multiqc_trimmed fastqc_trimmed
```
### 8. Сбор контиг через "platanus assemble"
```linux
platanus assemble -o Pxut -f sub1.fastq.trimmed sub2.fastq.trimmed 2> assemble.log
```
### 9. Сбор скаффолдов
```linux
platanus scaffold -o Pxut -c Pxut_contig.fa -IP1 sub1.fastq.trimmed sub2.fastq.trimmed -OP2 mate1.fastq.int_trimmed mate2.fastq.int_trimmed 2> scaffold.log
```
### 10. Уменьшение числа промежутков
```linux
platanus gap_close -o Pxut -c Pxut_scaffold.fa -IP1 sub1.fastq.trimmed sub2.fastq.trimmed -OP2 mate1.fastq.int_trimmed mate2.fastq.int_trimmed 2> gapclose.log
```
### 11. Удаление подрезанных чтений
```linux
rm sub*.trimmed mate*.int_trimmed
```
### Скрины отчетов для исходных чтений
![IMAGE 2022-10-03 22:42:15](https://user-images.githubusercontent.com/114533402/193664870-25561a0f-bea1-4451-ac11-ff65ce0ec721.jpg)
<img width="1131" alt="Снимок экрана 2022-10-03 в 22 40 49" src="https://user-images.githubusercontent.com/114533402/193664914-9d4260ef-e528-43f3-ac10-0540dbbfec0b.png">
### Скрины отчетов для обрезанных чтений
<img width="1383" alt="Снимок экрана 2022-10-03 в 22 41 42" src="https://user-images.githubusercontent.com/114533402/193665048-fb23a5bd-cd13-4a14-9223-4235f97a4729.png">
<img width="1092" alt="Снимок экрана 2022-10-03 в 22 42 01" src="https://user-images.githubusercontent.com/114533402/193665054-fd3dbecd-264b-4570-b620-d06149702609.png">

### Jupyter Notebook
[bioinf.pdf](https://github.com/avakhunova/hse22_hw1/files/9709485/bioinf.pdf)
