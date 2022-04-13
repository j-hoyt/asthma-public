# asthma-public
Overview of a rules-based (i.e., non-AI) text mining / natural language processing NLP program that I developed to read medical papers (mostly case studies) on occupational asthma and generate concise, informative summaries. I am not able to publicly provide any source code or resources developed specifically to support this application. 

## Problem:
An asthma researcher at [Unity Health Toronto](https://unityhealth.to/) was interested in maintaining an up-to-date list of all newly reported causes of occupational asthma. A carefully designed but straightforward database search of [PubMed](https://pubmed.ncbi.nlm.nih.gov/) might yield up to 1000 papers of **potential** interest from a one-year period. These would need to be manually reviewed by the researcher or a fellow expert to determine:
1. which ones were in fact reporting a confirmed case of occupational asthma, 
2. and of those, which ones were reporting a causal agent that had not previously been linked to occupational asthma, 
3. and of those:
   * what the asthmagenic causal agent was, and
   * what the occupational context was, whether a job title, job description, or general industry.

This is tedious and time-consuming work that could be automated to a large degree. 


## Task:
A nearly complete automation of the task would involve three broad phases:
   * **Phase 1:** A regular database search of PubMed for publications relating to occupational asthma. Such a search might return up to 1000 over a 1-year period;
   * **Phase 2:** Narrowing down this set of papers to those that are definitely reporting on a new cause of occupational asthma;
   * **Phase 3:** Extracting a concise summary of the paper, perhaps 3-7 sentences long, that specifies:
      * a confirmed diagnosis of occupational asthma;
      * the cause of the asthma, whether a chemical or biological agent or a particular environment;
      * the occupational context in which the patient developed asthma.

The primary aim of this application was to address Phase 3, but much of the work was able to to be used to address Phase 2 as well. 

## Approach


