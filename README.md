Data can be downloaded from this link:  
链接: https://pan.baidu.com/s/1LWuPlRPjZs8QJsHjjvjd0w  密码: kcij
--来自百度网盘超级会员V7的分享

#### 任务1. 熟悉Linux简单的命令
```
ls
top
mkdir
vim
cp
cd
mv
rm
pwd
less -S
more
cat
wc
```
- **练习题目**  
（1）在个人目录下创建空文件tmp.txt  
（2）利用vim文本编辑器，编辑字符串"AGIS"保存退出，然后查看tmp.txt的内容   
（3）查看AT.cds.fa内容（cat/less/more/tail等），提取序列ID（提示：提取文件中包含‘>’的行并将结果重定向到文件gene_id.txt，涉及到的命令包括grep/sed等 )。  
（4）统计gene_id.txt的行数。  
（5）查看当前工作路径。  
（7） 查看本目录下gene.bed文件内容并打印第1，2，3列并输出到文件gene.info中。  
（8）查看目录下number.txt文件内容, 对此列数据按照从大到小排序并删除重复项内容输出到number2.txt中。  

#### 任务2. 熟悉不同压缩格式的解压  
- **数据格式**  
```
zip/gzip/bzip2/tar.gz
gunzip AT.cds.fa.gz
unzip AT.cds.zip
bzip2 -d Os.pep.fa.bzip2
tar -zxvf ZipFiles.tar.gz
```
#### 任务3. 安装miniconda3
```
wget https://repo.anaconda.com/miniconda/Miniconda3-latest-Linux-x86_64.sh
sh Miniconda3-latest-Linux-x86_64.sh
```
利用conda安装常用生信软件
```
conda install bwa -c bioconda
conda install hisat2 -c bioconda
conda install blast -c bioconda
```

#### 任务4. 本地BLAST序列比对练习
安装BLAST
```
conda install blast -c bioconda
```
- **构建索引**  

构建核酸比对索引
```
makeblastdb -in AT.cds.fa -dbtype nucl -out nucldb
```
构建蛋白比对索引
```
makeblastdb -in Os.pep.fa -dbtype prot -out pepdb
```
- **序列比对**  
 
核酸比对
```
blastn -query AT.cds.fa -out nucl_blast.out -db nucldb -outfmt 6 -evalue 0.01 -num_threads 1

```  
蛋白比对
```
blastp -query Os.pep.fa -out pep_blast.out -db pepdb -outfmt 6 -evalue 0.01 -num_threads 1
```

#### 任务5. 利用BUSCO软件评估基因组组装质量
- **任务描述** 
 
利用BUSCO软件评估基因组组装质量
- **软件安装**

```
conda install -c bioconda -c conda-forge busco=4.1.2 augustus=3.3.3 biopython=1.76
```
- **命令行**
```
tar -zxvf enterobacterales_odb10.2020-03-06.tar.gz
busco -i ecoil.fasta -l ./enterobacterales_odb10 -o ecoil -m genome 
```

#### 任务6. 模拟二代数据  
- **任务描述**  
利用wgsim软件模拟大肠杆菌的二代数据，要求模拟2万条reads，双端150bp的数据。
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
cd PracticeData/task6/data/
ls
gunzip ecoli.contigs.fasta.gz
wgsim -N 20000 -1 150 -2 150  -S 1 ecoli.contigs.fasta reads_R1.fastq reads_R2.fastq /dev/null
```

#### 任务7. Illumina reads mapping  
- **任务描述**  
利用bwa软件mem算法将双端数据比对到参考基因组，并利用samtools将输出的sam文件转换成bam格式，最后利用samtools flagstat统计比对结果  
- **使用软件**：  
bwa (https://github.com/lh3/bwa)  
htslib (https://github.com/samtools/htslib)  
samtools (https://github.com/samtools/samtools)
- **软件安装**
```
git clone https://github.com/lh3/bwa.git
cd bwa
make
./bwa
cp bwa ~/bin/

git clone https://github.com/samtools/htslib.git
git clone https://github.com/samtools/samtools.git
autoheader
autoconf -Wno-syntax
./configure --prefix=/public/home/zhangxt/ --with-htslib=/public/home/zhangxt/Practice/software/htslib
make
make install
samtools -h
```
- **命令行**
```
bwa index ecoli.contigs.fasta
bwa mem -t 2 ecoli.contigs.fasta reads_R1.fastq reads_R2.fastq > out.sam
samtools view -bS out.sam > out.bam
samtools flagstat out.bam
```
#### 任务8. 基于参考基因组的组装
- **任务描述**  
利用ragoo软件，以日本晴的Chr1染色体作为参考，组装contig水平的水稻9311染色体Chr1  
- **使用软件**  
ragoo (https://github.com/malonge/RaGOO)  
faSize.pl (https://github.com/tangerzhang/my_script/blob/master/faSize.pl)  
miniconda3 (https://repo.anaconda.com/miniconda/Miniconda3-latest-Linux-x86_64.sh)
- **软件安装**

安装ragoo  
```
conda install intervaltree
conda install numpy
wget https://github.com/malonge/RaGOO/archive/refs/tags/v1.11.tar.gz
mv v1.11.tar.gz ./RaGOOv1.11.tar.gz
tar -zxvf RaGOOv1.11.tar.gz
cd RaGOO-1.11/
python setup.py install


```

安装faSize.pl
```
wget https://github.com/tangerzhang/my_script/blob/master/faSize.pl
chmod +x faSize.pl
cp faSize.pl ~/bin/
```

- **命令行**  
```
ragoo.py -C 9311_contig.fasta Nip_Chr1.fasta
faSize.pl ragoo.fasta
```


#### 任务9. 共线性绘制
- **任务描述**  
利用dotploty软件绘制水稻日本晴和9311之间的共线性
- **使用软件**  
minimap2 (https://github.com/lh3/minimap2)
dotploty (https://github.com/tpoorten/dotPlotly)

- **软件安装**

安装minimap2
```
git clone https://github.com/lh3/minimap2.git
cd minimap2 
make
```
安装R语言包：
```
conda install r-ggplot2 r-plotly
```
安装dotploty
```
git clone https://github.com/tpoorten/dotPlotly.git

```
- **命令行**
```
minimap2 -x asm5 Nip_Chr1_Chr2.fa 9311_Chr1_Chr2.fa > out.paf
pafCoordsDotPlotly.R -i out.paf -o out.plot -m 5000 -q 50000 -k 2 -s -t -l -p 8
```

#### 任务10. 变异检测
- **任务描述**   
利用nucmer软件计算水稻9311和日本晴的差异，并用delta-filter过滤结果，最后用show-snps获得snp信息
- **使用软件**   
MUMMER (https://github.com/mummer4/mummer/releases/download/v4.0.0.beta5/mummer-4.0.0beta5.tar.gz)  
- **软件安装**  
```
wget https://github.com/mummer4/mummer/releases/download/v4.0.0.beta5/mummer-4.0.0beta5.tar.gz
tar -zxvf mummer-4.0.0beta5.tar.gz
cd mummer-4.0.0beta5/
./configure --prefix=/public/home/zhangxt/
make && make install
```
- **命令行**  
```
nucmer Nip_Chr1.fasta 9311_Chr1.fasta --prefix out
delta-filter -r -q out.delta > out.filter.delta
show-snps -Clr out.filter.delta > out.snps
```

