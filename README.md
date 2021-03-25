# GenomicsPracticeData

任务1. 模拟二代数据  
- **任务描述**：利用wgsim软件模拟大肠杆菌的二代数据，要求模拟10万条reads，双端150bp的数据。
- **使用软件**：wgsim （https://github.com/lh3/wgsim）
- **软件安装**  
```
git clone https://github.com/lh3/wgsim.git
cd wgsim/
gcc -g -O2 -Wall -o wgsim wgsim.c -lz -lm
cp wgsim ~/bin/
wgsim
```
- **命令行**  
```
cd PracticeData/task1/data/
ls
gunzip ecoli.contigs.fasta.gz
wgsim -N 100000 -1 150 -2 150  -S 1 ecoli.contigs.fasta reads_R1.fastq reads_R2.fastq /dev/null
```

任务2. Illumina reads mapping  
- **任务描述**：利用bwa软件mem算法将双端数据比对到参考基因组，并利用samtools将输出的sam文件转换成bam格式，最后利用samtools flagstat统计比对结果  
- **使用软件**：  
bwa (https://github.com/lh3/bwa)  
htslib ()  
samtools (https://github.com/samtools/samtools)
- **软件安装**
```
git clone https://github.com/lh3/bwa.git
git clone https://github.com/samtools/htslib.git

git clone https://github.com/samtools/samtools.git

```
