---
title: "Why things turned out like this."
date: "2024-10-25"
description: "A reflection on PROTEUS"
tags: [life, essays, research]
---

> I really do not want to write this but will force myself to or else this information will LIVE in my head. 

# The many problems of PROTEUS

First I will try to explain why I think PROTEUS is useless purely from a results standpoint. Back when I implemented the first fixed omics analysis pipeline and called GPT4 at the very end to produce some conclusions, I thought that we were going to make so much progress starting from there, but that obviously did not happen :)

## What makes a research conclusion in omics "high-quality"?

One of my misconceptions going into this was that depth or complexity of the conclusions was what we were looking for. I later found conclusions in omics to be extremely homogenous, and a handful of common motifs and directions covered the bulk of conclusions in research papers. I now think what matters more is the level of statistical support and rigor of rationales behind the hypothesis proposal. If you discover a drug target for a cancer, no one's going to criticize how "x is a drug target for y" is too simple and ubiquitous of a format. Going from here, we see that the only fool-proof way of establishing "x is a drug target for y" is through wet lab experiments and (if we stretch this out real far) clinical trials. Bioinformatics analysis beforehand serves as a pointer for promising directions that may in the future be more concretely proved. The goal is to determine hypotheses that are amply supported by statistical trends, and therefore more promising and deserving of further investigation. *In silico* statistics alone are never sufficient proof for anything worth proving. For instance, after trying to replicate the cursed SOAT1 HCC paper, I thought that survival analysis was a solid indicator of drug target potential, since it was the most important analysis used to select SOAT1 for further experiments. But I later learned that sole survival analysis can be extremely misleading. There was a tweet that summed this up extremely well but I CAN'T FIND IT.

I would point out a caveat here: when trying to progress from empirical trends to biological mechanisms and explanations, conclusions can improve in both complexity and significance. Being able to handle more complex and intricate conclusions matters a lot in this context because it is needed for fine-grained explanation of complicated biological processes. (Been reading Molecular Biology of the Cell recently and often find myself wondering why any of us manage to stay alive given all the things that are going on.) However, I would argue that the above point still holds, namely: fixed motifs and "templates" exist and are valid guides (it's just that the total number of these motifs is larger); amount and validity of supporting evidence is the key deciding factor of conclusion quality. Using the case in the original knowledge discovery demo as an example: "NFE2L2 Promotes Pancreatic Cancer Progression via TPD52L1-EMT Signaling" follows the template of going from transcription factor (TF), to one of its target genes, then to the pathway this gene influences and its broader clinical consequences. Similar chains of reasoning can be used for analyzing countless different cases of how TFs influence cancer.

![20241024145657.png](https://apri-lllll.github.io/bell-jar/images/20241024145657.png)

When considering multiple conclusions for entire datasets, comprehensiveness is also key, but that's more self-explanatory, and easier to solve. In our experiments, I think that PROTEUS's proposed objectives and research directions are seldom unreasonable. We simply often lack the means to sufficiently address each individual objective.

I would also say that a significant portion of bioinformatics research is terrible and list surface-level statistics without more rigorous examination... but that is hardly any assurance because many things can be terrible at the same time.
WHY ARE WE AT 600 WORDS ALREADY THIS WAS NOT THE PLAN

## PROTEUS produces useless results.

In the context of PROTEUS, the previous section perfectly explains why the results were valid at first sight but meaningless from the perspective of any actual proteomics researcher. Of course the conclusions seem like realisitic proteomics results, but that's so easy to achieve it's comical. Get an open-domain LLM to hallucinate something and it may turn out ever better!
Behind this facade of validity, PROTEUS is lacking in both strength of evidence and comprehensiveness of analysis. A core problem behind both of these is the limited number of available workflows. I really did try to improve upon this but the whole framework of calling R withinn Python, and the complexity of R data structures and types, and the brittleness of many tools I was using, made the whole debugging process hard to handle. I don't think I ever got the gist of how to quickly incorporate more R tools. In the end I would rather have the system run boring stuff smoothly than technically be able to do more things but crash ever other second. Also, since we formulate the process as tool calling and not free-form code writing, which would be way too chaotic, it's not possible for the LLM to make minor adjustments to the analysis or design its own analysis steps, outside of the predefined workflows and limited amount of parameters. (This is all very hypothetical and I'm not saying that it would be able to lol.)

There are a couple of additional troubles worth mentioning. For strength of evidence, a limiting factor is that the context window of LLMs, coupled with a lack of effective memory management, makes it difficult for PROTEUS to conduct more complex reasoning. It is unable to efficiently build upon existing results and often repeats analysis instead of performing deeper investigations. For comprehensiveness, a problem behind this is the lack of flexibility interpreting analysis results. If GPT4 is given a result file and directly told to analyze it without additional cues, it will spit out the handful of molecules that have the most significant results, while perhaps hundreds of other notable results are not provided to the model in the first place. We will need to scrap this whole framework if we want PROTEUS to compare and choose between hundreds or thousands of molecules based on multi-faceted considerations, instead of just sorting according to logFC or something. We need to better integrate result interpretation with the specific research direction, ideally even incorporate more specific requirements at each analysis step, instead of merely including it as a stand-alone, fixed step.

I know I said I would only talk about results. I got carried away. To sum up how bad the results are, I'm pretty sure they would not be any better than if we simply ran a fixed pipeline. The uselessness of the whole planning idea and system is a result of both the lack of nuance of our incoporated workflows and the nature of proteomics research.

## Automation itself does not mean anything.

Nearing the end of the system's design I think we started to converge on the opinion that the main merit of PROTEUS is automation. Which makes no sense. "Automation" became a way of distinguishing our work from all the other "bioinformatics copilot" systems which required human involvement, and a way of sugarcoating the work from a pure methods pespective, without answering the harder question of how much real-life researchers would benefit from this form of automation.
Compared with copilots depending on largely fixed pipelines, our system's improvement of full automation only takes away the step of deciding the research and analysis directions. This step does not take much effort for a human researcher, and we also haven't achieved higher quality of research direction planning compared with experts, so it's hard to see the point. Compared with direct expert analysis, the speed of PROTEUS comes at the cost of more delicate parameter tuning and trial-and-error, and more importantly, general depth and quality of conclusions. Results are simply subpar and unusable.

Even if the system can produce 1 valuable result out of 100, we could easily say that this is fine, since the whole point of AI is to generate many results quickly. Extra credit if we could narrow down the range of promising results to, say, 10, before requiring human discernment. But the reality as of now is that the upper bound of result quality is almost as bleak as if we look at the average situation. If none of the conclusions are at the level of what we would expect from insightful expert analysis, there is really no saving this. A rough report generator is not what we're looking for.

I think the right path forward is not to cling to automation but figuring out how to achieve a system that is sensible to human instructions and make the best use of directions provided by researchers. And run many real-world trials.

# Why things turned out like this.

There's no sugarcoating how crap this whole thing turned out, but I've stopped mulling over it much because I did what I could at each timestep with the knowledge I had, which at times was zero, and that's fine because there's not shortcut to knowing something. Now that I know more about proteomics I have a better idea about where it went wrong. The positive feedback after I redid the whole clinical cohort system was promising. Not being able to rebuild the whole thing at demand is just another sad fact of life.

I AM DRINKING WHISKY RIGHT NOW

One sentiment that has haunted me since about a year ago has been: "it's imperative to know everything." It's easy enough to think that getting AI and bio people to just collaborate would work out, but in truth it was so difficult trying to get things going when I knew nothing and the bio people did not solve that. There were often conflicting opinions, mixed signals, flip-flops of stances, that I could not navigate. The whole thing was superalignment. How am I to decide which one of the "experts" to trust more, who (or no one) to listen to, which parts (or none) to listen to, and what questions to ask.
Essentially I think this is because in biology, the "stuff" cannot be wrapped neatly into concepts and often need to be tackled extremely specifically and solidly. This means that each expert would have different fields of expertise, and these fields are not things like "proteomics", "cancer", "immunology", which was what I thought, but much more specific things like "HCC", "T cell lymphocytes", etc. etc. BUT AT THE SAME TIME developing the system required understanding and organizing general analysis methods, and its qualitative evaluation requires intuition from experience. WHAT AM I SAYING. It takes some general insight from both the AI side and the bio side to even establish what I was looking for. I did not have that, so couldn't even have asked the right questions. Wrong use of the word "even" considering that the right questions are always the hard bit.

It did not help that I felt the most directly responsible for things turning out like this. While technically I do realize that I was not given a lot of the necessary guidance and resources needed to do this properly, after all is said and done it feels like this whole crowd of people are trying to clean up the mess that I made and make shit seem less like shit.

# Ideas

Here I will list some ideas for improving this circus that I think are promising. I am hoping that the process of writing helps me put the pieces together and figure out the whole thing.

## Domain-Specific Ontologies

Biology, not AI, was the domain that originally introduced me to "ontology" as a formal concept and research topic. I was reading about proteomics, bioinformatics, and learning about different ontology systems in biology such as GO. Biology is a field where specific "stuff" are extremely important, not just the overarching concepts, and ontologies made sense as guides to ensure that we describe these "stuff" and their relationships in a unified way.

Later on, I started becoming more aware of methods and cases in AI that can make use of ontology-adjacent solutions. These are especially prominent in rigorous and scientific domains that are not ideally handled by letting open-domain LLMs run wild. Google's AlphaProof was an indication that formal proof structures were still irreplaceable in mathematical problem-solving. Chess is another great examples. Google trained an expert-level model without search, which was cool, but pretty much set the takeaway as "better just search". More generally, a host of methods, for instance MCTS-related, for better structuring LLM reasoning during inference time have shown promise.

I saw the following summary on twitter recently and honestly it made me feel extremely unoriginal.

![8599721891dc7fb2c947bafd23e8c29.jpg](https://apri-lllll.github.io/bell-jar/images/8599721891dc7fb2c947bafd23e8c29.jpg)

The necessity of a similar scaffold in omics knowledge discovery is brought about by the presence of both fixed analysis processes and common conclusions structures, as I reflected on in the first section. In most cases, the search space of plans is finite and tractable, and entirely open-ended planning is unnecessary and troublesome.

I am planning to devise an ontology tailored for the task at hand, incoporating two types of entities (biological molecules and clinical features) and their possible relationships. Relationships within biological molecules will build upon statistical connections to elucidate biological mechanisms and regulatory networks. Relationships between biological molecules and clinical features. I have not yet done more detailed research on the ins and outs of established ontologies such as GO, which will hopefully give me inspiration on how to further refine the framework. Additionally, I am not yet certain about whether or not to enable LLMs to expand upon the human-designed ontology, but it's a worthy idea to explore for sure.

## Memory Management and Planning

(This is turning from conceptual improvement directions to a full-blown plan but I will vibe with it.)

Using the ontology, we will be able to proceed with memory management and planning in a more structured and organized manner. I plan to keep track of three data structures throughout the research process: a tool dependency graph, a progress tree, a conclusion graph. The ontology will be fixed, and stored as a small, dense graph with nodes representing entity types and directed edges representing their relationships. It is obviously possible to have multiple edges between each pair of nodes.

The tool dependency graph is self-explanatory and has been previously used in many LLM tool-use works. It's nodes will be available tools, and directed edges indicate tool dependencies. Maybe it will be possible to also include weaker edges as suggested, but not mandatory, sequential steps. The paths created by the strong and weak edge links in conbination will replace hard-coded workflows in the original framework.

The progress tree will record the analysis steps already performed by the system, as well as notable and expandable results. The inclusion of expandable results is intended to assist tool parameter selection later on. Each node in the tree will either be a tool that the system called, or a biological molecule that the system decides to focus on. After each tool is called, the tool node will expand numerous nodes consisting of molecules that the tool found notable, eg. differentially expressed molecules identified by differential expression analysis. The molecule that the system focused on at the previous step will always be added as a child node. After the system chooses the next molecule, the molecule node expands into all possible tool continuations based on the existing path of completed analysis tools. The inclusion of both molecules and tools in this graph serves to facilitate large-scale trial and error over sets of molecules, which was a major problem that we encountered in the original work.

Finally, the conclusion graph records all conclusions reached by the system. Each conclusion will take the format of a biological relationship within the defined ontology framework. Therefore, each node in the graph will correspond to an entity, and there can be multiple nodes belonging to the same entity type. Each edge will be a piece of evidence for a relationship, which will similarly belong to a defined category in the ontology graph. In addition to the possibility of there being multiple relationships between a pair of nodes, it is also possible, even desirable, to have multiple pieces of evidence, thus edges, in support of each relationship. Therefore, this conclusion graph will likely be larger and more dense than the underlying ontology. In the graph, the number of possible paths from a node to another will indicate the strength of support provided by statistical evidence that has been reached by the system. The graph will also be helpful for breaking down more complex conclusions into one-hop relationships, assisting the system's exploration of multi-step biological pathways and processes.

In summary, the memory management module is taken on by both the progress tree and the conclusion graph. The former leans more towards the short-term progress and the latter towards the long-term result. While the conclusion graph will include direct statistics that reflect on the analysis steps taken, the progress tree will be the only source of information on the most recent analysis tools executed and their precise sequence. It also informs the system of possible candidate molecules before their importances are further validated and they are added to the conclusion graph. The tool dependency graph and the progress tree will be the main sources of information supporting short-term planning of analysis steps. On the other hand, the conclusion graphs supports the long-term planning of research objectives.

## Basic Hypotheses + Further Evidence

The above formulation, supported by ontology, better elucidates the transition from hypothesis to conclusion. We've repeatedly discussed where the boundary lies between hypotheses and conclusions, and I've concluded (no wordplay intended), that it's really a spectrum a different levels of certainty, and there's no clear dividing line. All hypotheses have some reason behind its proposal, and all conclusions have some degree of uncertainty. Especially when we are talking bioinformatics, since statistics only go so far. We can discuss which conclusions are stronger, but nothing can be easily set in stone. This is also linked to the story I shamelessly weaved for one of our presentation slides. Once we have relatively certain "conclusions", we can build on top of them to proposal more volatile "hypotheses", then go forward and try to make these hypotheses become conclusions.

The conclusion graph will help us visualize this, and also eventually help the system keep track of this. Once we have formed individual, scattered links between nodes, which as elementary conclusions, we can expand upon them to propose hypotheses that include these edges but also have some gaps within the chain of reasoning. The next research objective would naturally be to fill in these gaps by returning to statistical anlaysis. We will also aim to fortify conclusions by searching for multiple paths of evidence along the conclusion graph. This may include anything from direct statistics to related literature or even cross-domain knowledge.

![e1fda9699f5ea216f8642dd384f52b6.jpg](https://apri-lllll.github.io/bell-jar/images/e1fda9699f5ea216f8642dd384f52b6.jpg)

## Self-Building Workflows

I have a dream. (nevermind)

I want to enable LLM agents to formulate, implement, and iteratively refine bioinformatics tools and even workflows. Since it can use a myriad of pre-built packages, the LLM's job would mainly be to understand the functions of different analysis libraries, deal with data format conversions, and form liasons between different tools. This all seems reasonably manageable for LLMs. To provide some extra support, we may want to build a very simple RAG system using the docs of commonly used bioinformatics libraries that we envision the system calling. We would no longer need to worry about the number of tools of libraries involved, because the LLM can produce enough information while formulating is plans to accurately located relevant document entries.

Ideally, the system would be instructed to write out a general tool class with several key adjustable parameters. This would allow the system to save and reuse any analysis methods. It would be able to iteratively adapt or expand existing workflows if they are insufficient for future situations. Essentially, the LLM would be doing what I was originally doing with BioEnricher and all that hot mess. When I previously worked to wrap up different sources of biology tools into our framework, I remember thinking that an LLM *should* be able to do this, but can't due to the complicated Python + R framework and many details and prompt engineering work within the workflows. I will make sure to avoid these problems, and since the LLMs will write the tools themselves, they will not make it too complicated.

The general process I envision is as follows: based on existing information on analysis progress, the system formulates a plan on how to proceed based on the defined ontology graph. Here the ontology will serve as a guide for narrowing down the range of workflows the system can choose from when it next plans more fine-grained analysis steps. Therefore, during each of the next steps, it will get the options to either use an existing workflow or design a new one. These more fine-grained steps will make use of the tool dependency graph and progress graph (I'm using tool and workflow interchangeably here). I also hope to give it the option to join existing workflows together to form longer ones which cover multiple hops along the tool dependency graph. Analogous to shopping malls that have esclators going from one floor to the next, but also some escalators that ascend two or three stories.

## Human Input as Feedback

This method naturally gives rise to an adjacent benefit, which is we can incorporate human input as feedback, instead of primarily as instructions. When I began this topic, I envisioned human input as taking the place of the LLM's general planning, eg. which tools to call. Now I've realized that in many cases, humans are needed to make smaller tweaks within the specifics of each workflow or tool, which was previously impossible because these were all boxed up. In comparison, "do DEA before you do GSEA" is something that LLMs can handle themselves.

In this new formulation, a feedback + refinement loop will certainly be needed for smooth automatic workflow development. In this case, human feedback can be optionally combined with the individual analysis execution results (or error messages lol) and fed to the LLM for it to improve the workflow designs. This then introduces a new issue of aligning the granularity of human input and workflow execution steps. Obviously we will not get human feedback after each single tool execution, so perhaps I will need to design a method of linking more general feedback to the specific workflow it concerns.
In addition, using human guidance to steer planning will likely remain as an optional feature. For this part I believe that the original simple implementation is good enough. It's just that we no longer need to emphasize full automation. HOPEFULLY!

## Multi-Omics & Data Dependencies

The overly complex and brittle data structures in the original system have been the bane of my existence for so long, and I am way too ready to scrap the whole thing and start again. This simplification is absolutely necessary under the new design, or else automatic workflow development will have no chance of working out.

For now, my plan is to scrap the integration of R altogether, since I found bio-related Python packages to be plentiful. For input data, the most direct plan would be to unify everything into two csv files containing molecule expression data and sample metadata respectively, which would be simple enough to work with.

In addition, we would directly operate on file inputs and data outputs, without using a data object that would need to be passed around and carefully managed. This would facilitate the incorporation of multi-omics data - just adding more input files to choose from wouldn't burden the system as much as keeping track of multiple data objects. I already started doing this for my revamp of the clinical cohort analysis system, so this *should* work.

## Types of Tools

I am planning on keeping the existing tool types. For external cohorts, since we are DITCHING DataChat (cheers), I think we can directly provide the relevant cohorts as data files, in the same format as our own source data, and these different datasets can share their sets of workflows.

Other forms of external knowledge will also be incorporated through automatically coding workflows. API information and usage can be provided to the LLM during its development process. A benefit of the new framework is that with the ontology, the system will be able to provide the names of entities with greater accuracy when calling these workflows. I want to find a way to query existing literature within the confines of the ontology entirely. Giving it text chunks will not do. I imagine that the existing graph structures and node definitions will make plugging in knowledge graphs much easier, but what do I know about knowledge graphs lol.

Now that I'm thinking about it, within this more flexible framework, it might be possible for the LLM to develop simple machine learning methods to do more sophisticated analysis. I still need to do more research on these methods, including the classified papers I have YET to go over, and related methods used in ChemCrow. But if this works it would be quite a glow-up for the whole thing.

![20241025191410.png](https://apri-lllll.github.io/bell-jar/images/20241025191410.png)

That being said, even though these are promising directions for improvement, I am still unsure whether I want to go down these paths. Currently the gap is so huge that even significant improvements may be meaningless.

I originally had more to write but we are at FOUR THOUSAND WORDS so I will stop.

![1729854641285.png](https://apri-lllll.github.io/bell-jar/images/1729854641285.png)
