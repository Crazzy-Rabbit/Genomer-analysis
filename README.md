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

相比 PSMC、MSMC，SFS 方法可以获得更为近期的群体历史动态变化，而且可以利用更大的群体信息得到更为精确的结果，同时不会增加计算负担。SFS 方法需要在计算时提供先验模型，然后从预先已知的先验分布中重复抽样来估计未知群体历史参数的后验分布。基于 SFS 的方法的另一个优点是可以重塑非常复杂的多群体历史动态，而且可以推断群体之间的迁移事件。

SFS 的方法需要用户提供可靠的先验模型才能得到具有生物学意义的结果。但在很多情况下，研究者并没有这样一个可靠的先验模型，所以大多数用户都会结合群体遗传结构分析的结果选择多个可能的模型分别进行计算，而且每个模型需要进行多次的bootstrap 并通过比较似然值来提高结果的准确性（似然值越大，所得到的参数越贴近实际数据）。不同模型之间，可以通过赤池信息量准则（Akaike Information Criterion, AIC）和贝叶斯信息准则（Bayesian Information Criterion, BIC）综合考虑自由参数与似然值进而选择最佳的模型。

针对每一个分离位点，如果我们不知道哪一个碱基是祖系状态，哪一个是后天得到的，那么这个时候 我们可以用minor allele frequency(MAF)来描述，也就是“少数等位基因”频率。MAF的范围在1/n - 0.5之间。它也被称为site frequency spectrum(SFS)。因为不能确定祖系状态，只能在“少数等位基因”，因而也被称为folded spectrum.
如果我们通过其他物种的基因组推测出了每一个位点的祖系状态，这个时候就可以用derived allele frequency(DAF)来表示位点变异情况，DAF的范围在1/n 到 (n-1)/n之间，相对应的，也被称为unfolded spectrum

在基因组数据分析中，我们在不知道祖系状态的情况下，通常将MAF<0.05的位点舍弃，因为这可能是测序错误或者有害突变，并不一定代表基因的多态性。但是这也有可能包含了DAF>0.95的位点，也就是会丢失掉很多进化上很重要的位点。


















