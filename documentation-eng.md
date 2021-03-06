# Mythologiae Datamodel 

- [Introduction](#introduction)
- [Layered Approach](#layered-approach)
  * [Layers 0 and 1: factual data and assertions](#layers-0-and-1--factual-data-and-assertions)
    + [WHAT](#what)
      - [Items](#items)
      - [Citations](#citations)
      - [Works](#works)
      - [Conceptual Categories](#conceptual-categories)
    + [WHERE](#where)
    + [WHEN](#when)
    + [WHO](#who)
  * [Layer 2: Provenance](#layer-2--provenance)
  * [Layer 3: Publication Info](#layer-3--publication-info)

## Introduction
In this section is briefly presented how <a href="https://mima-data-model.github.io/mima-documentation/">MIMA (Documentation)</a> has been reused and integrated to satisfy Mythologiae dataset representational requirements and peculiarities.
In the following sections is presented a brief overview of the 4 layered approach used to store and enhance Mythologiae data. 
For more information about this approach, please see "Daquino, M., Pasqual, V., Tomasi, F. “Knowledge Representation of digital Hermeneutics of archival and literary Sources.”

## Layered Approach
The so called "layered approach" cosists in reusing <a href="http://nanopub.org/wordpress/">Nanopublication</a> structure to encode sets of information descibing each cultural object in Mythologiae collection. Nanopublication datamodel, as shown in the figure below is organised in 3 interconnected graphs: the assertion, provenance and publication information graphs. A fourth layer (layer 0 or factual data graph) has been added to cover the entire domain.
Specifically, each layer (graph) stores a particular type of metadata.
* Layer 0. Factual data that are part of scholars’ background knowledge. In Mythologiae case we talk about descriptional metadata of cultural objects and literary works.
* Layer 1. The scope of scholars’ questionable statements. In Mythologiae specific case we talk about the hermeneutic analysis and cultural object-literary source association made by the experts.
* Layer 2. Context information for hypotheses assessment. In Mythologiae case we refer to the interpretative acts that lay behind each expert assertion.
* Layer 3. Provenance information of the mining processes. In Mythologiae case we encoded a few information about the RDF-dataset publication.

![image info](datamodel-imgs/struttura_mima_np.svg)  

Approach to datamodelling activity:
* Design of the domain
* input csv cleaning into computable python format to generate rdf
* Competency questions for each of the modules (WHAT, WHERE, WHO, WHEN) and submodules (WHAT: items, works, citations, conceptual categories)
* refactorization of concepts in triple-fashion using existing ontologies
* data alignement with external sources (e.g. Perseus, VIAF)
* conversion of input csv in python format into rdf (with rdf lib) following the chosen ontologies structures
* manual checking of entities (e.g. URIs consistency, doubles elimination)
* perfomation of queries to find errors in data production (e.g. missing properties between individuals)
* performation of CQs to test datamodel and dataset representational requirements
* again, automatic, semiautomatic and manual adjustment of data

This process has been performed sequentially and on each module and submodule.

### Layers 0 and 1: factual data and assertions 

The figure below represents layers 0 (factual data) and 1 (assertions) of the datamodel. All class and properties in the gray zone represent the assertion graph (layer 1) and all the other class and properties in white background represent factual data (layer 0). In particular, in level 0 are stored information about cultural objects and works descriptive metadata. In level 1 are stored the connections betweeen cultural objects representation (e.g. depicted scenes, conceptual value of the item), categories to which the cultural object is referred (e.g. the scene or theme depicted in the item), works and citations. For example, for what concerns the following scenario "The cultural object X represents the category Y, which is mentioned in the citation Z and the work W", descriptive metadata about works and cultural object can be stored in factual data graph (level 1), while the association between X, Y and Z is stored in assertion A. 

![image info](datamodel-imgs/layer0_layer1.svg)  

For the sake of simplicity, Mythologiae Datamodel has been tested in modules. Those modules are named 'What', 'Where', 'When', 'Where' - the four dimensions created by the reuse of an event-centric backbone ontology (FRBRoo). It is worth to underline that this research main interest is to investigate the 'What' dimension, taking into consideration how it interacts with the other three modules. 
In particular, the 'What' dimension represents how citations mentioning works and items are linked together through the use of conceptual categories. 'When', 'Where' and 'Who' can be seen as three modules of addition information aiming to investigate in toto the 'What' dimension. 

#### WHAT
##### Items
Items have been modelled with FRBRoo and dcterms. 
Example: The figure below expresses the the item 775 and its metadata, along with its conceptual aspect which represents Teseus.    
![image info](datamodel-imgs/what-items.svg)

URIs identifying items (efrbroo:F4_Manifestation_Singleton) are are incrementally numbered. 

##### Citations
Citations (Canonical Citations) has been modelled through the use of hucit.  

Example in natural language: "The passage "Eneide, IV, 362-392" refers to the abandon of Dido By Eneas". 
![image info](datamodel-imgs/what-citations.svg)

URIs identifying citations (hucit:Citation) are incrementally numbered. URIs identifying textual elements of the canonical work (hucit:TextElement) follow the structure "book number, line-line". URIs identifying works (hucit:Work) follows the strucuture "author name, work name" reconciled against viaf when possible.  


##### Works
Works have been modelled with FRBRoo. 
Example in natural language: "In "Rime" by Francesco Petrarca is take up the figure oF Arianna, princess of Cnosso"
![image info](datamodel-imgs/what-works.svg)

URIs identifying works (hucit:Work) follows the strucuture "author name, work name" reconciled against viaf when possible.  
Reconliciation against VIAF, why is this so important? 
Considering the messy data input, most of the cited works has been slightly differently recorded as strings in the input csv (e.g. Boccaccio Giovanni --> "Della Genealogia degli dei" oppure "Genalogie Deorum Gentilium Libri" oppure "Genalogie Deorum Gentilium" oppure "Genalogie Deorum"). VIAF reconciliation guarantees to reconduct all this different forms of the same work to a single controlled label (e.g. "Boccaccio, Giovanni, 1313-1375. | Genealogia deorum") and identifier (e.g. 182235138). The controlled label has been reused to create the rdf-dataset internal corresponding URIs (e.g. myth-work:boccaccio-giovanni-genealogie-deorum).

##### Conceptual Categories
Conceptial categories (or simply categories) are the connection between each cultural object representation and the citation or works they refer to. In Mythologiae data, a 2 layers taxonomy has been populated. For example the category "Enea fugge da Troia" refers to the supercategory "Enea". 

#### WHERE 

Example in natural language: "Item 81 is currently stored in Altes Museum (Berlin, Germany)". 

![image info](datamodel-imgs/where-museo-citta-nazione.svg)

#### WHEN 
Example in natural language: "Item 81 has been created between 320 and 340 b.C. (IV century b.C.) and belongs to the Classical era - Greek art".
![image info](datamodel-imgs/when-items.svg)

#### WHO 
Example in natural language: "Item 102 has been created by Giogio de Chirico (1913 - contemporary art piece)" and "La sfida al labiritinto, literary source by Italo Calvino"
![image info](datamodel-imgs/who.svg)

### Layer 2: Provenance
Starting from the abovementioned scenario "The cultural object X represents the category Y, which is mentioned in the citation Z and the work W", which has been said, it is stored into the graph assertion A, we use provenance graph to define the contextual information about A. Assuming that each assertion is generated by an interpretation, in provenance graph we define who performed the interpretation, when, with which criterion and its type. 
The following figures describes classes and properties reused in provenance graph. Such elements are mostly reused by HiCo and PROV. 

![image info](datamodel-imgs/layer2.svg)

### Layer 3: Publication Info
Finally, in publication information graph are stored the information about the publication of each nanopublication. Information in this layer answers the questions such as: Who is responsible for the machine-readable version of the statement? When was it extracted? It represents the meta-context of a statement that has been automatically or semi-automatically generated.

![image info](datamodel-imgs/layer3.svg)


