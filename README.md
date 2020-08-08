# UAM IR team at RecSys 2020

This repository contains the code and data needed to reproduce the experiments of the paper:

> R. Cañamares and [P. Castells](http://ir.ii.uam.es/castells). [On Target Item Sampling in Offline Recommender System Evaluation](http://ir.ii.uam.es/pubs/recsys2020.pdf). 14th ACM Conference on Recommender Systems (RecSys 2020). Virtual Event, Brazil, September 2020.

The software produces the results displayed in the figures of the paper (1 to 5).

## Authors

Information Retrieval Group at Universidad Autónoma de Madrid
- [Rocío Cañamares](mailto:rcanamares@gmail.com)
- [Pablo Castells](http://ir.ii.uam.es/castells)

## Software description

The repository contains the Java classes for reproducing the experiments reported in the paper. The software is organized in the following packages:
- `es.uam.ir.crossvalidation`: class for creating a 5-fold rating data partition for cross validation.
- `es.uam.ir.datagenerator`: class for handling the data and generating the training set by mapping negative ratings to zero.
- `es.uam.ir.filler`: class for filling in the missing coverage in recommendation rankings when an algorithm is not able to rank sufficient items for the metric cutoff.
- `es.uam.ir.targetsampling`: top-level main classes for generating the figures of the paper and sampling different target set sizes.
- `es.uam.ir.util`: additional utility classes for the rest of the program.

The software uses the [RankSys](http://ranksys.org/) library, and extends some of its classes. Our extensions are located in the following packages:
- `es.uam.ir.ranksys.metrics.basic`: extension of RankSys basic metrics to include the Coverage metric.
- `es.uam.ir.ranksys.nn.user`: extension of RankSys implementations of kNN collaborative filtering, adding normalized user-based variants.
- `es.uam.ir.ranksys.rec.fast.basic`: extension of RankSys implementations of non-personalized recommendation, adding average rating and random recommendation.
- `es.uam.ir.ranksys.rec.runner.fast`: extension of RankSys to include target samplers.

System Requirements
-------------------

- Java JDK: 1.8 or above (the software was tested using the version 1.8.0_40).

- Maven: tested with version 3.6.3.

## Installation

  Download all the files and unzip them into any root folder.

  From the root folder run the command: 
  
    mvn compile assembly::single

## Execution

In order to reproduce the results displayed in the figures of the paper, the datasets should be placed in the following locations:
- [MovieLens 1M](http://files.grouplens.org/datasets/movielens/ml-1m.zip). Uncompress the dataset file and place `ratings.dat` in path `datasets/ml1m/`. 
- [Yahoo R3!](https://webscope.sandbox.yahoo.com/catalog.php?datatype=r). Uncompress the dataset file and place `ydata-ymusic-rating-study-v1_0-train.txt` and `ydata-ymusic-rating-study-v1_0-test.txt` in path `datasets/yahoo/`.

Before producing any of the figures of the paper, first run the command:

  	java -cp .\target\TargetSampling-0.1-jar-with-dependencies.jar es.uam.ir.targetsampling.Initialize
	
This program generates the folders and files described in section [Output files generated upon initialization](#output-files-generated-upon-initialization) below. 

Once the previous command has finished, the results for the figures are generated with the following command:

  	java -cp .\target\TargetSampling-0.1-jar-with-dependencies.jar es.uam.ir.targetsampling.GenerateFigure figureNumber

Where `figureNumber` is the number of the figure you want to generate. For instance, the following command generates figure 1.

  	java -cp .\target\TargetSampling-0.1-jar-with-dependencies.jar es.uam.ir.targetsampling.GenerateFigure 1

A file `figure1.txt` is produced in the 'results/' folder when runnning the previous command. Similarly, `figure2.txt`, `figure3.txt`, etc., are produced by passing 2, 3, etc. as argument to the above command.

Note: running Figure 4 and Figure 5 generates additional auxiliary files, that you do not need to be concerned with unless you are interested in the intermediate results. See sections [Additional output files generated when running Figure 4](#output-files-generated-when-running-figure-4) and [Additional output files generated when running Figure 5](#output-files-generated-when-running-figure-5) for more details.

Warning: Figure 4 requires heavy computation and may take over one week to execute.

## Example of output file for figures
 
Note that given to the random sampling involved, the exact values slightly change from one execution to another:


- `figure1.txt`

		====================
		Dataset: ml1m
		====================

		P@10
		Recommender	Full	Test
		Average Rating	0.043632450331125824	0.6654437086092715
		Normalized kNN (full)	0.20870529801324503	0.6430397350993378
		Normalized kNN (test)	0.08596688741721856	0.6713145695364238
		Popularity	0.19433774834437084	0.6247715231788079
		Random	0.0101158940397351	0.550748344370861
		iMF (full)	0.35446688741721855	0.6591854304635761
		iMF (test)	0.1779072847682119	0.6846622516556291
		kNN (full/test)	0.31014238410596023	0.6591854304635761
	
- `figure2.txt`

		====================
		Dataset: yahoo
		====================

		nDCG@10
		Recommender	Full	Unbiased	Test
		Average Rating	0.001650734160221513	0.003829033307459144	0.5742325330205329
		Normalized kNN (full)	0.11147538266721554	0.018741243171895478	0.5592266654113534
		Normalized kNN (test)	0.003882970919506258	0.005036532205423697	0.5784354711675748
		Popularity	0.09399462311473945	0.014028366168191336	0.5517415219579382
		Random	0.004522436887264159	0.0025867995786283625	0.5339080440397328
		iMF (full)	0.16771377618299926	0.0251217955870769	0.5785765223432282
		iMF (test)	0.15340480662962952	0.02539973458928866	0.5834810525959093
		kNN (full/test)	0.16122921629314352	0.024345937551992086	0.574813835832783

		P@10
		Recommender	Full	Unbiased	Test
		Average Rating	7.703703703703704E-4	0.0015333333333333334	0.16937407407407407
		Normalized kNN (full)	0.04318888888888889	0.005303703703703704	0.16796296296296295
		Normalized kNN (test)	0.0017888888888888891	0.0016518518518518516	0.16967407407407406
		Popularity	0.035033333333333326	0.0042703703703703706	0.16799629629629628
		Random	0.0021222222222222224	9.407407407407409E-4	0.16673333333333334
		iMF (full)	0.05665925925925926	0.006592592592592593	0.1703222222222222
		iMF (test)	0.052974074074074076	0.006714814814814815	0.17070000000000002
		kNN (full/test)	0.05455185185185185	0.006485185185185186	0.16973703703703708

		Recall@10
		Recommender	Full	Unbiased	Test
		Average Rating	0.0031147841375079714	0.00860790711346267	0.6653173687495764
		Normalized kNN (full)	0.18541561377791896	0.03399792768959435	0.6616775832736017
		Normalized kNN (test)	0.007466027366265543	0.00951521164021164	0.6657179038022737
		Popularity	0.14840804400335653	0.026248706643151088	0.6626026429117486
		Random	0.007396483442576396	0.0049428277483833044	0.6583673440131952
		iMF (full)	0.23595273981035292	0.04412942386831276	0.6666139854632609
		iMF (test)	0.22593296451697437	0.04448390652557319	0.6672541822530319
		kNN (full/test)	0.23017855552221628	0.04199854497354497	0.6657941512861026


- `figure3.txt`

		====================
		Dataset: ml1m
		====================

		nDCG@10
		Target size	Average Rating	Normalized kNN (full)	Normalized kNN (test)	Popularity	Random	iMF (full)	iMF (test)	kNN (full/test)
		0	0.8474269175543897	0.8160138700628359	0.8560640445080618	0.7951016786057409	0.6938587830461577	0.8392894798788486	0.869845745259709	0.8402899757543999	
		1	0.8302687837271744	0.8033453158795792	0.844188068586804	0.7891364087628846	0.6589570611321536	0.8369285700866003	0.8656968700802136	0.8375852780862305	
		2	0.8143541633613524	0.7918560977120843	0.8331517909208177	0.7836486460751496	0.6290034817629173	0.834294875388436	0.8613803562039527	0.8349628501892955	
		5	0.7716195653249869	0.7606541618683871	0.8024665655458516	0.7671531680471257	0.5514022140874031	0.8265624491088086	0.8500156447880881	0.8272583396772968	
		10	0.7115228297491643	0.7231782361276808	0.7580025686825451	0.7421841077767024	0.451393507313454	0.8160834629568547	0.8325220433073339	0.8159683234042072	
		20	0.6224581091214122	0.6846044112170457	0.6878526962510374	0.7043594018471365	0.34219222114152026	0.8029565281647397	0.8021165420135411	0.7985826853895938	
		50	0.47540343032996824	0.6395027803761985	0.5634765275617488	0.6311444097925709	0.21141626058123855	0.7734366918184182	0.7333844000974973	0.7605659055545602	
		100	0.3601509831505314	0.6010048745345035	0.4573200753566578	0.561901280304331	0.13581288686070778	0.7403486896384539	0.6562736877339788	0.7180289289924343	
		200	0.25054601483033484	0.548047332735325	0.3542439027820496	0.48515268635867353	0.08181231155654889	0.6940097603546296	0.559012166024246	0.6609757666441688	
		500	0.13604002767006168	0.45002096900407346	0.22579365338449736	0.3798399964291014	0.03855122095950807	0.6090404043379937	0.40904141206097944	0.5639088475252959	
		1000	0.08304354038509996	0.3538668473399171	0.14695604536921053	0.30039476050747166	0.02008151422858673	0.5288886674282263	0.30113176785135976	0.4787546288169864	
		2000	0.03840049908595009	0.25147058646045484	0.08419500249292108	0.22797850770469935	0.010741474025524275	0.44031707177429336	0.20736314921530333	0.3914066084476547	
		3706	0.01583207677836752	0.17211749136937893	0.04461966601611872	0.1710266231206597	0.005681942151269359	0.3604854428871518	0.14314765027482065	0.31756171221269086	

		P@10
		Target size	Average Rating	Normalized kNN (full)	Normalized kNN (test)	Popularity	Random	iMF (full)	iMF (test)	kNN (full/test)
		0	0.6654437086092715	0.6430397350993378	0.6713145695364238	0.6247715231788079	0.550748344370861	0.6591854304635761	0.6846622516556291	0.6591854304635761	
		1	0.6566655629139073	0.6358576158940397	0.6649801324503312	0.6213112582781457	0.5324834437086092	0.6577218543046357	0.6820860927152317	0.6575066225165563	
		2	0.64808940397351	0.6284801324503311	0.6586920529801324	0.6179701986754967	0.5142350993377484	0.6560794701986754	0.679341059602649	0.6557980132450332	
		5	0.6211092715231789	0.6055728476821193	0.6395629139072848	0.6071854304635762	0.46288079470198673	0.6504105960264901	0.672566225165563	0.650476821192053	
		10	0.5804900662251655	0.5764602649006623	0.6096026490066225	0.5900960264900662	0.3879172185430464	0.6434470198675497	0.6610596026490067	0.6424271523178808	
		20	0.5168079470198675	0.546135761589404	0.5597980132450331	0.5632847682119205	0.30299668874172186	0.6340364238410595	0.6405894039735098	0.6295364238410596	
		50	0.4048112582781457	0.5104602649006622	0.4653112582781457	0.5088874172185431	0.1935364238410596	0.6126092715231788	0.5920397350993378	0.600907284768212	
		100	0.3136953642384106	0.4804602649006622	0.38298344370860926	0.45632450331125823	0.1260728476821192	0.5881059602649006	0.5358013245033113	0.5685298013245033	
		200	0.22138741721854305	0.44023509933774835	0.30250662251655636	0.3975894039735099	0.07653311258278146	0.5527913907284768	0.46229470198675493	0.5240662251655628	
		500	0.1246158940397351	0.3641092715231788	0.2003609271523179	0.31595695364238413	0.036599337748344375	0.4879801324503311	0.3438774834437086	0.4479105960264901	
		1000	0.08494370860927153	0.2889966887417218	0.13827483443708607	0.2529635761589404	0.01920198675496689	0.4247582781456954	0.25557947019867555	0.3796291390728477	
		2000	0.043632450331125824	0.20870529801324503	0.08596688741721856	0.19433774834437084	0.0101158940397351	0.35446688741721855	0.1779072847682119	0.31014238410596023	
		3706	0.020043046357615897	0.1461291390728477	0.04975496688741722	0.14565231788079472	0.005483443708609271	0.29138410596026487	0.12379470198675498	0.25129470198675496	

		Recall@10
		Target size	Average Rating	Normalized kNN (full)	Normalized kNN (test)	Popularity	Random	iMF (full)	iMF (test)	kNN (full/test)
		0	0.6266384740171216	0.6057585546067139	0.6298012679586169	0.6035505555604445	0.5668521678154991	0.6185188116404958	0.6351619213246099	0.6197158653371649	
		1	0.6190606492092695	0.5973632648437812	0.6241920501186813	0.5998816591365844	0.549759689150824	0.6167758841753055	0.6329032437880394	0.6178145242553763	
		2	0.6112286386245669	0.5881240171301719	0.6182942063116744	0.5962010222613425	0.5321458333086291	0.6145064959856493	0.6303289606144594	0.6158051205197526	
		5	0.5845558785226554	0.5539962693460395	0.5993711007114246	0.5830231449347161	0.47340461756487323	0.606242841954809	0.6226741659872465	0.6089089520997709	
		10	0.5368418923697125	0.4984121177058955	0.5654675439014706	0.5597819358803439	0.3656306617655322	0.5924069295942282	0.609127067184265	0.5972596006153738	
		20	0.45767836248098126	0.4446157559996761	0.5023685556747448	0.5206562496423088	0.25150789952604147	0.5784918091268105	0.5847771947730389	0.5788841670892299	
		50	0.32941978616735984	0.39578894620579025	0.3871322846458475	0.4457150123487965	0.13681051410331022	0.5496870351740643	0.5279607120685339	0.5406373764453507	
		100	0.24024514664762225	0.3680536956903485	0.2983028557432813	0.37924423629567416	0.08036444477318788	0.5183385568717808	0.4654884472713551	0.501493430475634	
		200	0.16194415792192027	0.3408351376470171	0.22245163861964484	0.31277379497866786	0.04412588158296539	0.47680325288307046	0.3875097608967993	0.45112966440795194	
		500	0.08364040874416587	0.2953741112511602	0.13781373747250264	0.23056094431144197	0.01910051523761007	0.4046683682547935	0.2735007759915167	0.37001109237643587	
		1000	0.054985689759310674	0.24662913238304768	0.09023117951939769	0.17426742789745023	0.010160200286924322	0.3407678286600797	0.1956516651659124	0.3031272314394585	
		2000	0.02669183114174914	0.18623823276557688	0.05097023817536843	0.12428011914972496	0.005318348557865324	0.2748393593348874	0.13057869461907687	0.23932348941317536	
		3706	0.011581141610150123	0.13213431866161526	0.02671609637234366	0.08607540702474145	0.002663733141711325	0.21908105494838245	0.08812157048829075	0.1877777494051907	

		====================
		Dataset: yahoo
		====================

		nDCG@10
		Target size	Average Rating	Normalized kNN (full)	Normalized kNN (test)	Popularity	Random	iMF (full)	iMF (test)	kNN (full/test)
		0	0.5742325330205329	0.5592266654113534	0.5784354711675748	0.5517415219579382	0.5339080440397328	0.5785765223432282	0.5834810525959093	0.574813835832783	
		1	0.5365223395523299	0.533381398067417	0.5577103381674886	0.5381587679775924	0.48721273606274973	0.5716202049576984	0.5769819915441677	0.5667436815586787	
		2	0.5065004594548841	0.5149382164952903	0.5394561986193269	0.5266287395967524	0.4529588409839313	0.5653724240496371	0.5708560671931644	0.5597318536225979	
		5	0.4371013560817773	0.47191819528621065	0.49231898223084036	0.499164891159612	0.37532121276763053	0.5488585259348573	0.5548844848918116	0.5421542211161171	
		10	0.3500652501852782	0.40596247333930585	0.4295489171453067	0.45744496136496826	0.26850993789554695	0.5240732451847766	0.5308669996799528	0.5164075500022591	
		20	0.23626883167014473	0.34169575627496124	0.34277236121287363	0.4038523669335924	0.16056518193316432	0.4894957076849617	0.49577377325726796	0.47810484108840284	
		50	0.10363088355433618	0.28050225663591427	0.19858525994405068	0.3242102016880833	0.07432212593860779	0.4243534579013374	0.4284027642686272	0.4152514351898467	
		100	0.045167330624334055	0.2469195395197883	0.10146854731738539	0.2632562461338456	0.03941996050450971	0.3684665611348721	0.36768623142348034	0.36009210640342787	
		200	0.01746669288334444	0.2123651566745623	0.040498766705399335	0.20742276541880372	0.020081898952859906	0.305358855298148	0.2981245050276884	0.29798496834545424	
		500	0.006155715486787669	0.1593375290726513	0.009986524634572787	0.13604938091945107	0.008362299048176161	0.22394473930175346	0.2107313482931291	0.21682422980321184	
		1000	0.001650734160221513	0.11147538266721554	0.003882970919506258	0.09399462311473945	0.004522436887264159	0.16771377618299926	0.15340480662962952	0.16122921629314352	

		P@10
		Target size	Average Rating	Normalized kNN (full)	Normalized kNN (test)	Popularity	Random	iMF (full)	iMF (test)	kNN (full/test)
		0	0.16937407407407407	0.16796296296296295	0.16967407407407406	0.16799629629629628	0.16673333333333334	0.1703222222222222	0.17070000000000002	0.16973703703703708	
		1	0.1680222222222222	0.16632592592592593	0.1686	0.16688148148148146	0.1648037037037037	0.16973333333333335	0.1701814814814815	0.16904444444444447	
		2	0.16644074074074072	0.16431851851851853	0.16742962962962965	0.16547037037037038	0.16180000000000003	0.1690888888888889	0.1694962962962963	0.16824074074074075	
		5	0.15975925925925927	0.15539629629629628	0.16330740740740743	0.1603814814814815	0.1494925925925926	0.16661851851851855	0.16723333333333334	0.16547407407407408	
		10	0.1392851851851852	0.12951851851851853	0.15331851851851852	0.1480185185185185	0.11185185185185184	0.16061851851851852	0.16213703703703702	0.15826296296296297	
		20	0.10261851851851853	0.10263703703703704	0.13268518518518518	0.13061851851851852	0.0685111111111111	0.15132592592592592	0.15287777777777778	0.14733703703703704	
		50	0.049044444444444446	0.08027037037037038	0.08568148148148148	0.10572962962962962	0.032596296296296304	0.13359259259259262	0.13494074074074072	0.1295962962962963	
		100	0.022222222222222223	0.07117407407407407	0.046848148148148146	0.08692222222222222	0.01767037037037037	0.11665555555555555	0.11746296296296296	0.11368888888888888	
		200	0.00864814814814815	0.06534444444444445	0.01905185185185185	0.07062592592592594	0.009274074074074075	0.09830000000000001	0.09737777777777779	0.09587407407407408	
		500	0.0033148148148148147	0.05666296296296297	0.004670370370370372	0.04869629629629629	0.003755555555555556	0.0738111111111111	0.07089259259259259	0.07154444444444444	
		1000	7.703703703703704E-4	0.04318888888888889	0.0017888888888888891	0.035033333333333326	0.0021222222222222224	0.05665925925925926	0.052974074074074076	0.05455185185185185	

		Recall@10
		Target size	Average Rating	Normalized kNN (full)	Normalized kNN (test)	Popularity	Random	iMF (full)	iMF (test)	kNN (full/test)
		0	0.6653173687495764	0.6616775832736017	0.6657179038022737	0.6626026429117486	0.6583673440131952	0.6666139854632609	0.6672541822530319	0.6657941512861026	
		1	0.6631092046841357	0.6588448328317067	0.6640906629913332	0.6605526690270815	0.655436903484848	0.6656026460328872	0.6664370633481147	0.6645789655459169	
		2	0.6602973462466161	0.6541095939288305	0.6622634263737319	0.6578997164621916	0.6484848741989341	0.6644937984478888	0.6654770214582334	0.66303331714897	
		5	0.6466695002027228	0.6318635373800514	0.6539510625392687	0.6458774549530795	0.617485146154882	0.6587197119216528	0.6602361050535368	0.6568922213182439	
		10	0.5785399760930137	0.533267499970085	0.6262934932577813	0.6042726687593085	0.4705891360950785	0.6388265645332727	0.6454945269450066	0.6337854373718891	
		20	0.4265127437547829	0.4138212335994947	0.55666648036744	0.5369265686755089	0.27574855569904616	0.6064113377085293	0.6134390405160806	0.5928935712744722	
		50	0.1984615481016997	0.3204132750654596	0.3728994758186742	0.43784530873052957	0.1260559893153989	0.5411678646381539	0.5491234099699354	0.5273387193669155	
		100	0.08591957828562953	0.28480764669516406	0.20444164090040146	0.36229415644457363	0.06479477268979329	0.4767716402635365	0.48226907704877575	0.46756283471278903	
		200	0.0316897656011111	0.2653354237299926	0.08333364480997613	0.29631237043439934	0.0334766907707463	0.4059476250859837	0.40584406698029785	0.3988081384502701	
		500	0.01257530953079793	0.23738827484271438	0.019557891010077745	0.20409188871826042	0.013483647673339084	0.30696740890273067	0.3012037117942265	0.30086250498426736	
		1000	0.0031147841375079714	0.18541561377791896	0.007466027366265543	0.14840804400335653	0.007396483442576396	0.23595273981035292	0.22593296451697437	0.23017855552221628	


		
- `figure4.txt`

		====================					
		Dataset: ml1m					
		====================					
							
		nDCG@10					
		Target size	Expected intersection ratio in top n	Ratio of ties	Ratio of ties at zero	Sum of p-values	
		0	0.574528681	0.185368969	0.008739357	71.28555491	
		1	0.558366078	0.138043992	0.008746452	77.55132457	
		2	0.540983483	0.118148061	0.008751183	79.15633664	
		5	0.480445039	0.091112819	0.008874172	67.26120313	
		10	0.372798668	0.071394276	0.009263245	51.30954202	
		20	0.25623712	0.056795175	0.010980369	35.51160652	
		50	0.138890495	0.046014664	0.017030511	21.13541875	
		100	0.08103185	0.047068354	0.026712394	41.60353423	
		200	0.044910018	0.056396641	0.042233917	22.69381847	
		500	0.019445149	0.08657403	0.076604778	27.7242271	
		1000	0.010035044	0.124270341	0.116047777	22.77760853	
		2000	0.005105328	0.187986045	0.180604305	29.10533677	
		3706	0.002804388	0.269395695	0.262850047	16.96667005	
							
		P@10					
		Target size	Expected intersection ratio in top n	Ratio of ties	Ratio of ties at zero	Sum of p-values	
		0	0.574528681	0.555457663	0.008739357	64.97746512	
		1	0.558366078	0.528716887	0.008746452	88.37805274	
		2	0.540983483	0.504722091	0.008751183	88.05789623	
		5	0.480445039	0.431420293	0.008874172	74.06320958	
		10	0.372798668	0.344123699	0.009263245	41.19384225	
		20	0.25623712	0.281005203	0.010980369	41.55735873	
		50	0.138890495	0.22593543	0.017030511	28.50290115	
		100	0.08103185	0.202827578	0.026712394	30.55372366	
		200	0.044910018	0.193336093	0.042233917	37.70691166	
		500	0.019445149	0.203304163	0.076604778	22.90575647	
		1000	0.010035044	0.232816935	0.116047777	15.35931483	
		2000	0.005105328	0.281673368	0.180604305	21.74596902	
		3706	0.002804388	0.34489948	0.262850047	36.40395814	
							
		Recall@10					
		Target size	Expected intersection ratio in top n	Ratio of ties	Ratio of ties at zero	Sum of p-values	
		0	0.574528681	0.555457663	0.008739357	91.89411298	
		1	0.558366078	0.528716887	0.008746452	93.25529831	
		2	0.540983483	0.504722091	0.008751183	112.1142271	
		5	0.480445039	0.431420293	0.008874172	89.79130929	
		10	0.372798668	0.344123699	0.009263245	60.32143589	
		20	0.25623712	0.281005203	0.010980369	48.01642512	
		50	0.138890495	0.22593543	0.017030511	35.52516904	
		100	0.08103185	0.202827578	0.026712394	29.37267018	
		200	0.044910018	0.193336093	0.042233917	29.48777395	
		500	0.019445149	0.203304163	0.076604778	25.76555899	
		1000	0.010035044	0.232816935	0.116047777	33.57200604	
		2000	0.005105328	0.281673368	0.180604305	24.41926729	
		3706	0.002804388	0.34489948	0.262850047	37.65538846	
							
		====================					
		Dataset: yahoo					
		====================					
							
		nDCG@10					
		Target size	Correlation with unbiased evaluation	Expected intersection ratio in top n	Ratio of ties	Ratio of ties at zero	Sum of p-values
		0	0.642857143	0.978749634	0.634497354	0.322525132	175.8397479
		1	0.714285714	0.973822498	0.528431217	0.322547619	97.78268079
		2	0.785714286	0.967250713	0.486854497	0.322649471	78.56412382
		5	0.857142857	0.929165893	0.43537037	0.323156085	59.6964845
		10	0.857142857	0.725526751	0.407256614	0.325641534	34.77367448
		20	0.857142857	0.419060186	0.400887566	0.339746032	37.00487599
		50	0.928571429	0.186671985	0.426284392	0.383628307	30.74563587
		100	0.857142857	0.097347174	0.470984127	0.437825397	46.56217236
		200	0.857142857	0.049778853	0.523746032	0.499484127	34.24317516
		500	0.785714286	0.020199182	0.599140212	0.583867725	41.52063557
		1000	0.714285714	0.010199519	0.664207672	0.654013228	56.5984932
							
		P@10					
		Target size	Correlation with unbiased evaluation	Expected intersection ratio in top n	Ratio of ties	Ratio of ties at zero	Sum of p-values
		0	0.642857143	0.978749634	0.973275132	0.322525132	364.5719429
		1	0.642857143	0.973822498	0.96564418	0.322547619	187.0699151
		2	0.642857143	0.967250713	0.956312169	0.322649471	134.4884984
		5	0.714285714	0.929165893	0.91365873	0.323156085	103.0997591
		10	0.714285714	0.725526751	0.778624339	0.325641534	52.75302968
		20	0.785714286	0.419060186	0.668384921	0.339746032	38.82973256
		50	0.857142857	0.186671985	0.604529101	0.383628307	44.28920657
		100	0.928571429	0.097347174	0.601115079	0.437825397	29.29375845
		200	0.785714286	0.049778853	0.626005291	0.499484127	50.15748309
		500	0.785714286	0.020199182	0.678030423	0.583867725	34.39498621
		1000	0.714285714	0.010199519	0.724080688	0.654013228	39.97958074
							
		Recall@10					
		Target size	Correlation with unbiased evaluation	Expected intersection ratio in top n	Ratio of ties	Ratio of ties at zero	Sum of p-values
		0	0.642857143	0.978749634	0.973275132	0.322525132	336.1208641
		1	0.642857143	0.973822498	0.96564418	0.322547619	225.2082675
		2	0.642857143	0.967250713	0.956312169	0.322649471	181.0026774
		5	0.642857143	0.929165893	0.91365873	0.323156085	120.1367147
		10	0.714285714	0.725526751	0.778624339	0.325641534	66.76248884
		20	0.714285714	0.419060186	0.668384921	0.339746032	56.77973938
		50	0.857142857	0.186671985	0.604529101	0.383628307	45.97959657
		100	0.928571429	0.097347174	0.601115079	0.437825397	38.2936459
		200	0.785714286	0.049778853	0.626005291	0.499484127	42.59841558
		500	0.857142857	0.020199182	0.678030423	0.583867725	42.87905009
		1000	0.785714286	0.010199519	0.724080688	0.654013228	51.86857291


		
- `figure5.txt`

		====================
		Dataset: ml1m-nofill
		====================

		Coverage@10
		Target size	Average Rating	Normalized kNN (full)	Normalized kNN (test)	Popularity	Random	iMF (full)	iMF (test)	kNN (full/test)
		0	0.8865860927152319	0.6780331125827814	0.8858245033112583	0.8865860927152319	0.8866688741721853	0.8866688741721853	0.8866688741721853	0.876913907284768	
		1	0.9137649006622517	0.6785728476821192	0.907360927152318	0.9137649006622517	0.9140463576158939	0.9140463576158939	0.9140463576158939	0.8887847682119204	
		2	0.937566225165563	0.6792218543046357	0.9268841059602648	0.937566225165563	0.9379403973509934	0.9379403973509934	0.9379403973509934	0.8997019867549669	
		5	0.9852549668874173	0.6809039735099337	0.9702019867549669	0.9852549668874173	0.9856688741721854	0.9856688741721854	0.9856688741721854	0.9285066225165564	
		10	0.9999801324503311	0.6835364238410596	0.9972814569536425	0.9999801324503311	0.9999900662251655	0.9999900662251655	0.9999900662251655	0.9620331125827815	
		20	1.0	0.6896357615894039	0.9999933774834437	1.0	1.0	1.0	1.0	0.9908079470198675	
		50	1.0	0.7047350993377484	1.0	1.0	1.0	1.0	1.0	0.9998841059602649	
		100	1.0	0.7279403973509934	1.0	1.0	1.0	1.0	1.0	1.0	
		200	1.0	0.7668211920529802	1.0	1.0	1.0	1.0	1.0	1.0	
		500	1.0	0.8481390728476821	1.0	1.0	1.0	1.0	1.0	1.0	
		1000	1.0	0.9180761589403973	1.0	1.0	1.0	1.0	1.0	1.0	
		2000	1.0	0.9737384105960265	1.0	1.0	1.0	1.0	1.0	1.0	
		3706	1.0	0.9958013245033112	1.0	1.0	1.0	1.0	1.0	1.0	

		====================
		Dataset: yahoo-nofill
		====================

		Coverage@10
		Target size	Average Rating	Normalized kNN (full)	Normalized kNN (test)	Popularity	Random	iMF (full)	iMF (test)	kNN (full/test)
		0	0.4307148148148148	0.13495925925925928	0.41503703703703704	0.4307148148148148	0.4307148148148148	0.4307148148148148	0.4307148148148148	0.4140407407407408	
		1	0.5203777777777778	0.13655185185185187	0.47208518518518516	0.5203777777777778	0.5203777777777778	0.5203777777777778	0.5203777777777778	0.47368148148148154	
		2	0.6074074074074074	0.13802592592592594	0.5273888888888889	0.6074074074074074	0.6074074074074074	0.6074074074074074	0.6074074074074074	0.5324037037037037	
		5	0.8395518518518518	0.14304074074074075	0.6844555555555555	0.8395518518518518	0.8395518518518518	0.8395518518518518	0.8395518518518518	0.6968925925925926	
		10	0.9995259259259258	0.15131111111111112	0.8852592592592593	0.9995259259259258	0.9995259259259258	0.9995259259259258	0.9995259259259258	0.9031555555555556	
		20	1.0	0.16712222222222223	0.9960629629629629	1.0	1.0	1.0	1.0	0.9981259259259259	
		50	1.0	0.21698518518518517	1.0	1.0	1.0	1.0	1.0	1.0	
		100	1.0	0.2950074074074074	1.0	1.0	1.0	1.0	1.0	1.0	
		200	1.0	0.44641481481481476	1.0	1.0	1.0	1.0	1.0	1.0	
		500	1.0	0.7909444444444444	1.0	1.0	1.0	1.0	1.0	1.0	
		1000	1.0	0.9804592592592594	1.0	1.0	1.0	1.0	1.0	1.0	


## Configuration

The software takes configuration parameters defined in the following files:
- `conf/ml1m-biased.properties`: configuration for running the experiments using the ratings of the MovieLens 1M dataset. The parameters of the algorithms along with other properties are set here.
- `conf/yahoo-biased.properties`: configuration for running the experiments using the biased ratings of the Yahoo R3 dataset. The parameters of the algorithms along with other properties are set here.
- `conf/yahoo-unbiased.properties`: configuration for running the experiments using the unbiased ratings of the Yahoo R3 dataset. The parameters of the algorithms along with other properties are set here.

## Output files generated upon initialization

In case you are interested in the detail and intermediate results that are elaborated and aggregated into the figure files, we provide here a description of the corresponding files containing such detail.

The following files are generated in the folder results/biased.

For Yahoo! R3:
- `yahoo-target-sampling.txt`: Contains the performance (in terms of different metrics) achieved by each algorithm for each target size and each fold.
The format of the file is : `fold\ttarget size\trecommender system\tCoverage@10\tnDCG@10\tP@10\tRecall@10`.

- `yahoo-ties.txt`: For each pair of algorithms, the file contains the ratio of users for which the two algorithms obtain the same metric value (in terms of different metrics and target size).
The format of the file is : `target size\trecommender system 1\trecommender system 2\tCoverage@10\tnDCG@10\tP@10\tRecall@10`.

- `yahoo-tiesAtZero.txt`: For each pair of algorithms, the file contains the ratio of users for which the two algorithms obtain a metric value of 0 (in terms of different metrics and target size).
The format of the file is : `target size\trecommender system 1\trecommender system 2\tCoverage@10\tnDCG@10\tP@10\tRecall@10`.

- `yahoo-pvalues.txt`: For each pair of algorithms, contains the t-test p-value for a metric comparison of both algorithms (in terms of different metrics and target size).
The format of the file is : `target size\trecommender system 1\trecommender system 2\tCoverage@10\tnDCG@10\tP@10\tRecall@10`.

- `yahoo-expected-intersection-ratio.txt`: Contains the expected intersection ratio for each target size and each fold.
The format of the file is : `fold\ttarget size\texpected intersection ratio`.

For MovieLens 1M, equivalent output files to the previous ones for Yahoo! R3.
- `ml1m-target-sampling.txt`
- `ml1m-ties.txt`
- `ml1m-tiesAtZero.txt`
- `ml1m-pvalues.txt`
- `ml1m-expected-intersection-ratio.txt`

The following files are generated in the folder results/unbiased. These files are equivalent to the ones generated in results/biased but using the unbiased test from yahoo, and without considering different target sizes. Only one target size (full = 1000) appears in the files.
- `yahoo-target-selection.txt`
- `yahoo-ties.txt`
- `yahoo-tiesAtZero.txt`
- `yahoo-pvalues.txt`
- `yahoo-expected-intersection-ratio.txt`


## Output files generated when running Figure 4

In case you are interested in the detail and intermediate results that are elaborated and aggregated into the figure files, we provide here a description of the corresponding files containing such detail.

The curve `sum of p-values` of Figure 4 requires additional files to be generated. They are equivalent to the [ones generated upon initialization](#output-files-generated-upon-initialization) but using more parameter configurations for kNN and iMF.

These files are generated when running the command to generate Figure 4 (see section [Execution](#execution)).

For Yahoo! R3:
- `yahoo-allrecs-target-sampling.txt`
- `yahoo-allrecs-ties.txt`
- `yahoo-allrecs-tiesAtZero.txt`
- `yahoo-allrecs-pvalues.txt`
- `yahoo-allrecs-expected-intersection-ratio.txt`

For MovieLens 1M:
- `ml1m-allrecs-target-sampling.txt`
- `ml1m-allrecs-ties.txt`
- `ml1m-allrecs-tiesAtZero.txt`
- `ml1m-allrecs-pvalues.txt`
- `ml1m-allrecs-expected-intersection-ratio.txt`

## Output files generated when running Figure 5

In case you are interested in the detail and intermediate results that are elaborated and aggregated into the figure files, we provide here a description of the corresponding files containing such detail.

Figure 5 requires running algorithms without filling in the rankings when they are short of coverage. Therefore, running the command to generate Figure 5 (see section [Execution](#execution)) generates equivalent files to the [ones generated upon initialization](#output-files-generated-upon-initialization) but without randomly filling in the rankings. 

For Yahoo! R3:
- `yahoo-nofill-target-sampling.txt`
- `yahoo-nofill-ties.txt`
- `yahoo-nofill-tiesAtZero.txt`
- `yahoo-nofill-pvalues.txt`
- `yahoo-nofill-expected-intersection-ratio.txt`

For MovieLens 1M:
- `ml1m-nofill-target-sampling.txt`
- `ml1m-nofill-ties.txt`
- `ml1m-nofill-tiesAtZero.txt`
- `ml1m-nofill-pvalues.txt`
- `ml1m-nofill-expected-intersection-ratio.txt`
