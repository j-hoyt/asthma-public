j-hoyt/asthma-public
===

Overview of a rules-based (i.e., non-AI) text mining / natural language processing NLP program that I developed to read medical papers (mostly case studies) on occupational asthma and generate concise, informative summaries. I am not able to publicly provide any source code or resources developed specifically to support this application. 

Problem:
===

An asthma researcher at [Unity Health Toronto](https://unityhealth.to/) was interested in maintaining an up-to-date list of all newly reported causes of occupational asthma. Maintaining such a list matters to workers being exposed to such asthmagens, safety regulators, employers, healthcare workers, and insurance companies.  

Staying current on relevant publications is extremely time-consuming. Last year, [PubMed](https://pubmed.ncbi.nlm.nih.gov/) alone added about 1,000,000 new publications. Nearly 10,000 of those were related to asthma. Approximately 10 of those were reports of new causes of occupational asthma. Doing a thorough job by manual review is not practical. 

A nearly complete automation of the task would involve three broad phases:
   * **Phase 1:** A regular database search of PubMed for publications relating to occupational asthma; 
      * A particular carefully designed but straightforward database search of [PubMed](https://pubmed.ncbi.nlm.nih.gov/) returned 1049 papers of **potential** interest from a five-year period
   * **Phase 2:** Narrowing down this set of papers to those that are definitely reporting on a new cause of occupational asthma
      * Labour-intensive manual review determined that 43 of these 1049 papers were definitely relevant

   * **Phase 3:** Extracting a concise summary of the paper, perhaps 3-7 sentences long, that specifies:
      * a confirmed diagnosis of occupational asthma;
      * the cause of the asthma, whether a chemical or biological agent or a particular environment;
      * the occupational context in which the patient developed asthma.
 
 **Phase 3 is the focus of this project**

Goals:
===
Based on the 43 papers identified manually, and a set of key sentences from each one, develop software that can identify and extract the relevant information automatically.

![image](https://user-images.githubusercontent.com/43970162/163232189-78b9b7d6-6894-450f-ba23-5d4e7ce84932.png)

Assess results based on:
  * Precision
    * Minimizing false positives; not including irrelevant info
  * Recall
    * Minimizing false negatives; not leaving out relevant info
  * F<sub>1</sub> score
    * average of precision and recall
  * F<sub>3</sub> score
    * weighted average of precision and recall where false negatives are 3x more costly than a false positive
    * a short summary with a little extra info is better than a short summary with too little


Approach:
===

The core functionality of reading and identifying information is provided by a [GATE (General Architecture for Text Engineering)](https://gate.ac.uk/) application. 

### Cleaning the inputs
The papers being analyzed were mostly in PDF format which can be difficult for a machine to read properly because of non-standard layouts. These were imported into GATE and automatically processed into plaintext. These would be full of irregularities that made it difficult for GATE to identify individual sentences, so further processing was done by **regular expressions** in a **Java** application to:
  * join words that were separated across a line break;
  * remove line breaks within a sentenc;
  * to separate headings (e.g., "Abstract", "Conclusion") from surrounding sentences;
  * etc.
This produced a "flatter" plaintext document that was much more machine-readable. Basic initial GATE processing includes identifying all individual words and sentences and annotating parts of speech, numbers, dates, proper nouns, locations, and orthography. 

### Assembling gazetteers: lists of words and phrases that we're looking for

Much of the power of this application comes from lists of words and phrases that are key to identifying relevant information. GATE's gazetteer format is quite simple, but date comes from a different sources in a variety of formats. Each source required extensive processing with **regular expressions** in a **Java** application to extract what was needed.

#### Lists of possible asthmagens
The difficulty in identifying asthamgens in the text is that we're looking for them because we don't already know what they are. The approach taken was to make as comprehensive a list as possible of every chemical substance. Sources include:
  * [BRENDA (The Comprehensive Enzyme Information System)](https://www.brenda-enzymes.org/)
    * 54,467 entries
  * [ChEBI (Chemical Entities of Biological Interest)](https://www.ebi.ac.uk/chebi/)
    * 141,372 entries
  * [Wikidata](https://www.wikidata.org/wiki/Wikidata:Main_Page)
    * All entries on Wikidata identified as a “chemical substance”
    * 640,878 entries

Wherever each chemical or biological agent was found in the text, it was annotated with an identifier for the database it came from (i.e., ChEBI ID, BRENDA ID, or Wikidate Entity Identifier) which could be used to generate a direct link in the output summary for more information.  

![image](https://user-images.githubusercontent.com/43970162/163240859-605beeb1-df8e-40fd-9e8e-68ffeb5bcda4.png)


#### Occupational contexts
A list of job titles was created from two major sources:
  * Statistics Canada’s [National Occupational Classification](https://noc.esdc.gc.ca/)
    * ~75,000 job titles, each annotated with a 4-digit NOC code
  * US Bureau of Labor Statistics’ [Standard Occupational Classification](https://www.bls.gov/soc/)
    * ~33,000 job titles, each annotated with a 6-digit SOC code

These required extensive processing and manual review to generate well-formatted lists, e.g., changing "foreman/woman" into separate entries for "foreman" and "forewoman", separating "chief executive officer (CEO)" into two separate entries, and other more subtle issues.

### Other relevant phrases
Important sentences were identified based on key phrases related to the specific problem domain:
* mentions of **occupational asthma** and related respiratory issues
  * 52 entries
* mentions of clinical terms related to case studies and patient examinations
  * 30 entries
* mentions of **medical terms** related to the diagnosis and treatment of asthma
  * 42 entries
* mentions of **novelty** in the reporting
  * e.g., "first case report", "not previously been identified", "to the best of our knowledge", etc
  * 54 entries
* mentions of **work** and occupational contexts
  * 19 entries
* mentions of **summary** often indicate valuable statements
  * e.g., "in conclusion", "in this study", etc.
  
Provided these gazetteers, GATE can annotate each sentence with the category of word or phrase that it contains. 

![image](https://user-images.githubusercontent.com/43970162/163240439-8043dfba-151e-4ae4-95b4-d74eb3e6fb30.png)





