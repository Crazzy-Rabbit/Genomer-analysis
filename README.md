# Genomer-analysis
## 动物驯化及群体历史研究：
* 与驯化相关的研究热点主要集中在：家养动物何时何地被驯化？驯化途径是什么？驯化表型是什么？与驯化表型相关的基因有哪些？家养动物驯化地点大致主要集中在三个地方：近东地区、中国中部和安第斯山脉。几乎所有家养动物都是在近几千年或几万年内被驯化。
* 驯化的途径主要分为三种：*共生、捕食和直接驯化*。共生：指人类与被驯化物种的野外物种长期处在相同环境下，逐渐与人和谐共处达到驯化的目的，猫和狗是通过该方式驯化的；捕食：指人类相对被驯化动物是捕食者，当人类狩猎捕食的个体太多的时候，会将一些个体圈养起来，进而达到驯化的目的，牛与猪是通过该方式驯化：直接驯化：指人类为了某项具体的目的直接改造物种的方式，例如人类养蚕是为了丝绸织布，驯化马和骆能是为了运输。
* 人工驯化的家养动物通常需要经历两次群体瓶颈时期：*驯化事件和品种形成事件*。驯化事件指的是从野生状态到家养状态的过程，一般发生在一万年左右，例如灰狼驯化成狗的过程。品种形成事件则是指现代品种的形成，一般发生在最近几百年左右，例如各个不同品种狗的形成。驯化会导致一系列表型的显著改变，在家养动物中，这种表型的选择通常包括温顺程度、行为、体型大小与形态、皮肤和毛发颜色以及与繁殖相关性状等方面的变化。
                       ![image](https://user-images.githubusercontent.com/111029483/222344636-6f245dc3-50ea-4222-a117-f535649d8ac8.png)
### 等位基因频谱（SFS）：
基于 SFS 重构群体历史的常用软件有∂a∂i与 Fastsimcoal2
* *∂a∂i*：使用扩散近似法（diffusion approximation）来推断最多 3 个群体的种群大小变化、种群分化及迁移率等历史事件；
* *Fastsimcoal2*：使用复合似然值（composite likelihood）来推断群体演化历程。
* 两者对于少于 4 个群体的历史推测有着相似的运算效率，但当群体数超过 3 个时，只能利用 Fastsimcoal2 进行计算。
    计算过程中，∂a∂i 与 Fastsimcoal2都需要从高质量且独立的位点获得群体之间的频谱信息：即在计算 SFS 之前，必须要去掉变异位点之间的连锁不平衡，且尽可能利用高覆盖度高质量的全基因组，测序数据去生成 SFS。

* 相比 PSMC、MSMC，SFS 方法可以获得更为近期的群体历史动态变化，而且可以利用更大的群体信息得到更为精确的结果，同时不会增加计算负担。SFS 方法需要在计算时提供先验模型，然后从预先已知的先验分布中重复抽样来估计未知群体历史参数的后验分布。基于 SFS 的方法的另一个优点是可以重塑非常复杂的多群体历史动态，而且可以推断群体之间的迁移事件。

* SFS 的方法需要用户提供可靠的先验模型才能得到具有生物学意义的结果。但在很多情况下，研究者并没有这样一个可靠的先验模型，所以大多数用户都会结合群体遗传结构分析的结果选择多个可能的模型分别进行计算，而且每个模型需要进行多次的bootstrap 并通过比较似然值来提高结果的准确性（似然值越大，所得到的参数越贴近实际数据）。不同模型之间，可以通过赤池信息量准则（Akaike Information Criterion, AIC）和贝叶斯信息准则（Bayesian Information Criterion, BIC）综合考虑自由参数与似然值进而选择最佳的模型。

* 针对每一个分离位点，如果我们不知道哪一个碱基是祖系状态，哪一个是后天得到的，那么这个时候 我们可以用minor allele frequency(MAF)来描述，也就是“少数等位基因”频率。MAF的范围在1/n - 0.5之间。它也被称为site frequency spectrum(SFS)。因为不能确定祖系状态，只能在“少数等位基因”，因而也被称为folded spectrum.
* 如果我们通过其他物种的基因组推测出了每一个位点的祖系状态，这个时候就可以用derived allele frequency(DAF)来表示位点变异情况，DAF的范围在1/n 到 (n-1)/n之间，相对应的，也被称为unfolded spectrum

* 在基因组数据分析中，我们在不知道祖系状态的情况下，通常将MAF<0.05的位点舍弃，因为这可能是测序错误或者有害突变，并不一定代表基因的多态性。但是这也有可能包含了DAF>0.95的位点，也就是会丢失掉很多进化上很重要的位点。

### 群体历史模拟中最优模型选择准则：
* 赤池信息量准则：
  1. Akaike information criterion、简称AIC，是衡量统计模型拟合优良性的一种标准，是由日本统计学家赤池弘次创立和发展的。赤池信息量准则建立在熵的概念基础上，可以权衡所估计模型的复杂度和此模型拟合数据的优良性。
  2. 增加自由参数的数目提高了拟合的优良性，AIC鼓励数据拟合的优良性但尽量避免出现过度拟合（Overfitting）的情况。所以优先考虑的模型应是AIC值最小的那一个。赤池信息量准则的方法是寻找可以最好地解释数据但包含最少自由参数的模型

* 贝叶斯信息准则：
  1. 也称为Bayesian Information Criterion（BIC）。贝叶斯决策理论是主观贝叶斯派归纳理论的重要组成部分。是在不完全情报下，对部分未知的状态用主观概率估计，然后用贝叶斯公式对发生概率进行修正，最后再利用期望值和修正概率做出最优决策。
  2. 与AIC相似，训练模型时，增加参数数量，也就是增加模型复杂度，会增大似然函数，但是也会导致过拟合现象，针对该问题，AIC和BIC均引入了与模型参数个数相关的惩罚项，BIC的惩罚项比AIC的大，考虑了样本数量，样本数量过多时，可有效防止模型精度过高造成的模型复杂度过高。

* AIC和BIC的原理是不同的，AIC是从预测角度，选择一个好的模型用来预测，BIC是从拟合角度，选择一个对现有数据拟合最好的模型，从贝叶斯因子的解释来讲，就是边际似然最大的那个模型。

  1. 共性:
    * 构造这些统计量所遵循的统计思想是一致的，就是在考虑拟合残差的同时，依自变量个数施加“惩罚”。
  2. 不同点
    * BIC的惩罚项比AIC大，考虑了样本个数，样本数量多，可以防止模型精度过高造成的模型复杂度过高。
    * AIC和BIC前半部分是一样的，BIC考虑了样本数量，样本数量过多时，可有效防止模型精度过高造成的模型复杂度过高。

* 今天我们所观察到的基因组遗传变异是一系列复杂演化过程的产物，不仅受突变、随机漂变、选择（自然选择、人工选择）、重组等的影响，也与参考基因组的组装质量及遗传变异的质量有关。在诸多的影响因素下，为了能够更精确的完成群体之间的溯祖，我们需要结合多方面的分析结果来确认一个最稳健的进化模型，包括群体遗传分析（系统发育树分析，主成分分析，聚类分析，连锁不平衡分析及群体遗传参数统计等）、不依赖先验模型的分析（PSMC、SMC++和 MSMC）及模拟分析（ms、Ma CS）等。

## 基因组渗透：

* 适应性基因渗透包含两个过程：基因渗透和适应。基因渗透是指遗传成分从一个群体通过有性生殖的方式进入到另一个群体。产生杂交的前提是两个群体之间没有生殖隔离或只有不完全生殖隔离。只有这样才能通过与可育Ｆ１代回交达到遗传成份垂直传递的目的。通过基因渗透得到的区域，如果在该群体内没有受到选择，那么会因为随机遗传漂变的影响，逐渐在基因组中消失。只有当基因渗透得到的区域受到自然或人工选择，它才能在基因组中保留一定的频率。


## 变异检测：
* 一般来说我们使用GATK HaplotypeCaller模块进行变异检测。
  * 由于古 DNA 中存在的 5’端 C 到 T 和 3’端 G到 A 的损伤模式会造成大量假阳性 SNP，因此只保留在现代样本中能够检测到的变异，排除了古代样本中的特有变异。由于古代样本中内源性 DNA 含量通常较低，最终得到的测序深度也很低 ，因此针对古代样本还增加了--minPruning 1 和--minDanglingBranchLength 1 的设置以提高低深度位点变异检测的灵敏度。

* 变异检测还可以使用bcftools和ANGSD等软件。
  * 若进行的是那种top期刊的研究，推荐使用两种变异检测软件进行变异检测以降低检测到的SNP的假阳性。

* *性别鉴定*，在动物中可以由性染色体的测序深度来确定，由于雄性为XY，雌性为XX，因此雌性的性染色体测序深度应为雄性的二倍。分别统计每个样本在常染色体、X 染色体和 Y 染色体上 SNP 位点的平均 reads 覆盖深度，以常染色体的深度为分母，分别计算了  X 和  Y  染色体的相对覆盖深度。预期观察到雄性个体中的 X 染色体相对覆盖深度为 0.5 且 Y 染色体也为 0.5，而雌性个体中 X 染色体相对覆盖率为 1 且Y 染色体为 0。

## 群体遗传结构及渗透分析
### 计算群特异性位点
```
## 1. 提取群体vcf文件
bcftools提取和牛与其他群体vcf文件(大的vcf文件用的是未过滤maf 和geno的)，然后用Maf和geno过滤后的SNP即这个群体获得的SNP数。

## 2. 取特异性位点
再把bim文件进行sort a.txt b.txt b.txt | uniq -u操作得到a群体特异性位点。

sort QC.01.10.JPBC-geno005-maf003.bim QC.01.10.sample115-geno005-maf003.bim QC.01.10.sample115-geno005-maf003.bim | uniq -u > JPBC.only.txt

uniq参数说明：
-d     仅显示重复出现的行列;
-u     仅显示出一次的行列。
```
### 群体遗传结构
#### PCA
纯数学的运算方法，可以将多个相关变量经过线形转换选出较少个数的重要变量。
##### GCTA软件
```
## make germ
/home/software/gcta_1.92.3beta3/gcta64 --bfile QC.common_89_cattle_851_ASIA-geno005-maf003 \
                                       --make-grm --autosome-num 29 \
                                       --out QC.common_89_cattle_851_ASIA-geno005-maf003.gcta
## PCA
/home/software/gcta_1.92.3beta3/gcta64 --grm QC.common_89_cattle_851_ASIA-geno005-maf003.gcta \
                                       --pca 4 \
                                       --out QC.common_89_cattle_851_ASIA-geno005-maf003.gcta.out
```
##### EIGENSOFT软件
1. 转换格式：
EIGENSOFT中内置的convertf 文件转化为smartpca的输入文件
```
convertf -p transfer.conf

需要一个config file，将文件的输入输出写进去。
##config file
genotypename:    myplink_test.ped
snpname:         myplink_test.map # or example.map, either works
indivname:       myplink_test.ped # or example.ped, either works
outputformat:    EIGENSTRAT
genotypeoutname: example.eigenstratgeno
snpoutname:      example.snp
indivoutname:    example.ind
familynames:    NO
```
产生三个pca所需的输入文件 example.eigenstrat example.snp example.ind

2. smartpca做PCA

smartpca -p runningpca.conf
```
##config file
genotypename: example.geno
snpname: example.snp
indivname: example.ind
evecoutname: example.pca.evec
evaloutname: example.eval
altnormstyle: NO
numoutevec: 10
numoutlieriter: 5
outliersigmathresh: 6.0
```
##### Admixture
群体结构分析的算法模型为：假设所有群体拥有K个祖先，每个群体的特征由其每个位点的等位基因频率决定，在哈代-温伯格平衡下，使用最大似然估计算法将实际群体中的每个个体以概率的形式，计算每个个体的基因组变异来源，用Q值表示，Q值越大，表明该位点来自这个祖先群体的可能性越大，从而将每个位点归类到不同的祖先成分。
```
# admixture
for K in 2 3 4 5 6 7 8 9 10 11 12 13 14 15; do /home/software/admixture_linux-1.3.0/admixture --cv QC.sample-northeast_Asia-geno005-maf003.bed $K | tee log${K}.out; done
# 提取CV值
CV error最小的为最佳K值
grep -h CV log*.out


Admixture.sh
touch admixture.sh
nohup bash admixture,sh &
## 提取：
bcftools view -S id.txt  common_89_cattle_851_ASIA.vcf  -Ov > sample-select.vcf
## 转格式：
plink --allow-extra-chr --chr-set 29 -vcf sample-select.vcf --make-bed --double-id --out sample-select
## 过滤： 
plink --allow-extra-chr --chr-set 29 -bfile sample-select --geno 0.05 --maf 0.03 --make-bed --out QC.sample-select-geno005-maf003
## admixture
for K in 2 3 4 5 6 7 8 9 10 11 12 13 14 15; do admixture --cv QC.sample-select-geno005-maf003.bed $K | tee log${K}.out; done
## 提取CV值
## CV error最小的为最佳K值
grep -h CV log*.out 
```
##### 系统发育树
###### MEGA













