---
weight: 1
bookFlatSection: true
title: "TREC 2024 Lateral Reading Track"
---

# TREC 2024 Lateral Reading Track Guidelines

**This 2024 track has concluded, and its successor, the [TREC 2025 DRAGUN Track](https://trec-dragun.github.io/), will continue in 2025. DRAGUN stands for "Detection Retrieval-Augmented Generation for Understanding News".**

## Overview

Welcome to the TREC 2024 Lateral Reading Track.
This track is for researchers interested in addressing the problems of misinformation and trust in search and online content.
The current web landscape requires the ability to make judgments about the trustworthiness of information, which is a difficult task for most people.
Meanwhile, automated detection of misinformation is likely to remain limited to well-defined domains or be limited to simple fact-checking.

[Wineburg and McGrew (2019)](https://journals.sagepub.com/doi/abs/10.1177/016146811912101102) discovered that professional fact-checkers follow a process of **Lateral Reading** that involves asking questions about a document's source and evidence and seeking answers to these questions via search engines in order to establish the document's trustworthiness.
In the first year of this track, our goal is to explore NLP and IR technologies to support the tested practice of lateral reading during people's trustworthiness evaluation of **online news**, given the civic importance of trustworthy news and a decline in trust in news over the years [(Brenan, 2022)](https://news.gallup.com/poll/403166/americans-trust-media-remains-near-record-low.aspx).
As such, it will not require a definition of what is true and what is misinformation, and thus the track can address trustworthiness beyond the relatively narrow focus of traditional fact-checking and claim verification.

While teaching people how to do lateral reading can be an effective means of helping people better evaluate the trustworthiness of information [(McGrew et al., 2019)](https://bpspsychub.onlinelibrary.wiley.com/doi/abs/10.1111/bjep.12279), this training cannot easily reach the millions of people who use the Internet and are finished with their schooling.
As such, an opportunity exists for systems to assist users with lateral reading by helping users understand what they should question about a document and helping them find other documents that can answer those questions.
For example, imagine a "Lateral Reading Copilot" that could assist or nudge people towards lateral reading behaviors when reading web pages.

In the first year, this track has **two tasks with separate deadlines**. 
For [Task 1](./#task-1-question-generation), participants need to suggest questions that a reader of an online target news article should ask to determine its trustworthiness.
After the deadline for Task 1, we will pool submitted questions as input to [Task 2](.#task-2-document-retrieval), where participants need to retrieve documents from the specified web collection to answer those questions.
<!--At the same time, we will have NIST assessors manually perform this task to produce the questions they believe are most important.
After the deadline for Task 1, we will release the NIST assessors' Task 1 questions as input to [Task 2](#task-2-document-retrieval), where participants need to retrieve documents from the specified web collection to answer those questions.-->

<!--<img src="/lr_questions_example.png" alt="Example Questions for Lateral Reading" title="picture_lr_questions_example" style="border-radius: 0"/>-->

## Data

- **Web Collection**: This track will use the English subset `ClueWeb22-B-English` of the new [ClueWeb22](https://www.lemurproject.org/clueweb22.php/) Category B dataset as the document collection, which contains about 87 million popular web documents of roughly 200 GB of data (size of plaintext).
This web collection was collected around February 2022. Please refer to their [website](https://www.lemurproject.org/clueweb22/obtain.php) for how to obtain the dataset.
The `ClueWeb22-B-English` subset is found in `cw22-b/txt/en` for plaintext and `cw22-b/html/en` for WARC format, etc.
Considering the size of `ClueWeb22-B-English`, we suggest obtain the collection as soon as possible.
- **News Articles**: [trec-2024-lateral-reading-task1-articles.txt](/trec-2024-lateral-reading-task1-articles.txt) contains the the ClueWeb22-IDs of 50 selected target news articles (or "topics"), each about a different event, published in 2021 and 2022 from various sources.
If participants have not yet obtained the collection, they can obtain the 50 documents (plaintext version) from CMU for free once they have signed a licensing agreement with them.
To obtain this 50-document subset, please refer to the [How to Get It](https://www.lemurproject.org/clueweb22/obtain.php) page and follow the instructions to request the `TREC-LR-2024-T1` subset.

## Tasks

Assume there is a (general public) reader who is looking through online news. This track has the following two tasks with separate submission due dates.

### Task 1: Question Generation

For each of the 50 topics (i.e., target news articles), participants need to produce **10** questions that the reader should ask to evaluate its trustworthiness, ranked from the most important to the least important to ask. Those questions should meet the following requirements.
- Should be self-contained and explain the full context, i.e., one can understand this question without reference to the article.
- Should be at most 120 characters long.
- Should be reasonably expected to be answered by a single web page.
- Compound questions should be avoided, e.g. who is X and when did Y happen? In general, each question should focus on a single topic.

Participants should put all the questions for those 50 articles into a single file, using the format below, and submit it to NIST via [Evalbase](https://ir.nist.gov/evalbase/).
- It should be a tab-separated file.
- It should be encoded in UTF-8.
- Each line consists of the following tab-separated fields in this order: `topic_id`, `run_tag`, `rank`, `question`.
    - `topic_id`: ClueWeb22-ID of the target news article.
    - `run_tag`: A tag that uniquely identifies your group and the method you used to produce the run. Each run should have a different tag. Run tags for runs submitted by one group **must** all share a common prefix to identify the group across runs.
    - `rank`: Your rank for the question, starting from 1.
    - `question`: Question in plaintext, with no tabs or newlines.

Submissions can be either **manual** (involving human intervention to generate questions, e.g., hiring people to produce questions or manually selecting questions from a candidate list of questions produced by algorithms or other human involvement in the question generation process) or **automatic** (automatic systems that produce questions without the need of human input beyond the construction of the systems). 
Teams submitting automatic runs should make a good faith effort to not read or study the 50 articles.
<!--After the submission due date of Task 1, we will make available the questions that NIST assessors raise during their evaluation of each news article.-->

**Example article and questions**: On February 21, 2023, the New York Times published an opinion article by Bret Stephens entitled ["The Mask Mandates Did Nothing. Will Any Lessons Be Learned?"](https://www.nytimes.com/2023/02/21/opinion/do-mask-mandates-work.html?unlocked_article_code=gFPkTMW10NLmmWAaYVT8kUk5IJGdtqOT4oPTIljn-eqha-dQGMt5LbDNkcSGc-lPWwq88xHQztwlyXkwSvjA42AWwawLMaAd0GruPyysGxIHa4izksvdo7Rzs08-EuiXyFTG01aeEWnRqUzneqq92uwtQH8FPvptgSBG2nc2u5i7JZ-Q5yMFli4VgmS1-2XMEPxw4ZX_-FXhpdOse85-TnFMOHW1Oc1r0347aFhJ73iOcuIs6nBJu8GERP8f9dqxnFtJ_km19GyZsJCtPv7Q9I3RNo4ozPwIhlV0nqJfDGiOwP3GTfFyFh_OuqglmGDh3UAmSRtWsP0IhiGu&smid=url-share).
(Note that this document is not part of the ClueWeb22 collection.)
In the article, Stephens makes an argument that mask mandates during the COVID pandemic did not work. Given the importance of this issue, the reader would be advised to examine the trustworthiness of the information.
As suggested by lateral reading, we want to ask about sources, evidence, and what others say about the issue. This example file [trec-2024-lateral-reading-example-questions.txt](/trec-2024-lateral-reading-example-questions.txt) shows the 10 questions we manually created to evaluate the trustworthiness of this article, based on the plaintext content of this article. In working to answer these questions, the reader would likely learn that Stephens is a conservative [(Wikipedia, accessed in 2024)](https://en.wikipedia.org/w/index.php?title=Bret_Stephens&oldid=1256439085), that Tom Jefferson had previously published articles using other studies as evidence against masks, which received criticism from other scientists [(Gorski, 2023)](https://sciencebasedmedicine.org/the-cochrane-mask-fiasco-how-the-evidence-based-medicine-paradigm-can-produce-misleading-results/), that Maryanne Demasi is a journalist who has faced criticism for reports that go against scientific consensus, e.g. Wi-Fi is dangerous [(Wikipedia, accessed in 2024)](https://en.wikipedia.org/w/index.php?title=Maryanne_Demasi&oldid=1175116938), and that the Cochrane study was misinterpreted as it was inconclusive about the question of if interventions to encourage mask wearing worked or not [(McDonald, 2023)](https://www.factcheck.org/2023/03/scicheck-what-the-cochrane-review-says-about-masks-for-covid-19-and-what-it-doesnt/).
A file in the format of this example is what we expect participants to return, containing questions for all the 50 articles.

### Task 2: Document Retrieval

We have pooled 12 questions for each topic (i.e., target news article) from those submitted by participants in Task 1: [trec-2024-lateral-reading-task2-questions.txt](/trec-2024-lateral-reading-task2-questions.txt).
In Task 2, for each question, participants need to retrieve **10** documents from the web collection `ClueWeb22-B-English` that are useful for answering those questions in the context of the corresponding news article, ranked by their usefulness.
This task is similar to a traditional ad-hoc retrieval task.
As we provide questions, to participate in Task 2 does not require your participation in Task 1.
The question file is a tab-separated file with the following fields:
- `question_id`: Unique identifier for the question. No two questions will have the same identifier, even across different topics.
- `topic_id`: ClueWeb22-ID of the target news article, associated with the question.
- `question`: Question in plaintext, with no tabs or newlines. 

Note that the pooled questions may not satisfy the requirements for Task 1, e.g., some questions may not be self-contained or understandable without the context of the news article.
Those questions might be discarded during the evaluation of Task 2 submissions.
Meanwhile, participants are free to use the target news articles in addition to the pooled questions during their retrieval processes.

Runs can be either **full rank** or **rerank**.
We have prepared a BM25-RM3 baseline run ([Organizers-Baseline-BM25RM3](/Organizers-Baseline-BM25RM3)) and participants can rerank the top 100 retrieved results using BM25 (`k1=0.9, b=0.4`) with RM3 (`fb_terms=10, fb_docs=10, original_query_weight=0.5`) as implemented in [Pyserini](https://github.com/castorini/pyserini), without the hassle to index the full collection.
Participants who want to rerank the baseline run can request the plaintext version of those retrieved documents (about 1 GB in JSONL format) from CMU after signing a licensing agreement with them.
Please refer to the [How to Get It](https://www.lemurproject.org/clueweb22/obtain.php) page and follow the instructions to request the `TREC-LR-2024-T2` subset.

Similar to Task 1, runs may be either **automatic** or **manual**.
Participants should follow the standard TREC run format below and submit their runs to NIST via [Evalbase](https://ir.nist.gov/evalbase/).
- It should be a space-separated file.
- It should be encoded in ASCII.
- Each line consists of the following space-separated fields in this order: `question_id`, `Q0`, `doc_id`, `rank`, `score`, `run_tag`.
    - `question_id`: Unique question id from the question file (above).
    - `Q0`: Unused column, whose value should always be Q0.
    - `doc_id`: ClueWeb22-ID of the retrieved document answering the question in the context of the target news article.
    - `rank`: Rank of the document, starting from 1.
    - `score`: Score (integer or floating point) of the document, in non-increasing order. [trec_eval](https://trec.nist.gov/trec_eval/) sorts documents by the scores instead of the ranks.
    - `run_tag`: A tag that uniquely identifies your group and the method you used to produce the run. Each run should have a different tag. Run tags for runs submitted by one group **must** all share a common prefix to identify the group across runs.

## Schedule

- **Task 1 Article Released**: Early May
- **Task 1 Question Generation <u>Submission Due</u>**: ~~June 30~~ July 26
- **Task 2 Question Released**: August 7
- **Task 2 Document Ranking <u>Submission Due</u>**: ~~August 30~~ September 8
- **Result Release**: ~~Late September~~ November 5
- **Notebook <u>Paper Due</u>**: ~~Late October~~ November 10
- **TREC 2024 Conference**: November 18-22 at NIST in Gaithersburg, MD, USA

## Evaluation

NIST assessors judged the helpfulness of generated questions from Task 1 and the usefulness of retrieved documents to answer the corresponding question in the context of the news article from Task 2.
Here are the assessing instructions: [instructions.pdf](/assessing_instructions.pdf).
Please refer to our track overview paper for detailed result analysis.
<!--Evaluation details are to be determined.-->
<!--As part of the Task 1 evaluation, we will consider the overlap between the questions created by the assessors and the questions created by the participants, but the primary measure will be based on the usefulness of the questions, independent of the assessor created questions.-->

## Q&A

1. How do we register to participate in this track? \
*Please follow the TREC registration guidelines from their [Call for Participation](https://trec.nist.gov/pubs/call2024.html).*
2. Why can't we join the Slack channel? \
*[#lateral-reading-2024](https://trectalk.slack.com/archives/C065QFMRNBY) is in the TREC workspace. Please join the workspace first by following the instructions in the TREC 2024 welcome email after registration.*
3. Is there a limit on how many runs each group can submit?\
*Participating groups will be allowed to submit as many runs as they like, but they need authorization from the track organizers before submitting more than 10 runs per task. Not all runs are likely to be used for pooling and groups will need to specify a preference ordering for pooling purposes.*

## Organizers

<style>
    table {
        width: 100%;
        background-color: white!important;
        border-collapse: collapse; /* Ensures there are no spaces between cell borders */
    }
    th, td {
        padding: 8px; /* Add some padding for content inside cells */
        text-align: left; /* Align text to the left */
    }
    th:first-child, td:first-child {
        max-width: 21%; /* Set minimum width to 21% of the table/page width */
        width: 150px;
    }
    /* Remove borders */
    td, th {
       border: none!important;
    }
    img {
        border-radius: 20%;
    }
</style>

<table>
    <tr>
        <th><img src="https://scholar.googleusercontent.com/citations?view_op=medium_photo&user=Hg46RfsAAAAJ&citpid=7" alt="Dake Zhang" title="picture_dake_zhang"/></th>
        <td><b>Dake Zhang</b> <br> University of Waterloo <br> Waterloo, Ontario, Canada <br> <a href="https://zhangdake.tech/" target="_blank">Website</a> &nbsp; <a href="https://www.linkedin.com/in/zhangdake/" target="_blank">LinkedIn</a> &nbsp; <a href="https://twitter.com/ZhangDake1998" target="_blank">X</a></td>
    </tr>
    <tr></tr>
    <tr>
        <td><img src="https://scholar.googleusercontent.com/citations?view_op=medium_photo&user=BgiGGQQAAAAJ&citpid=4" alt="Mark D. Smucker" title="picture_mark_smucker" /></td>
        <td><b>Mark D. Smucker</b> <br> University of Waterloo <br> Waterloo, Ontario, Canada <br> <a href="https://uwaterloo.ca/management-science-engineering/profile/msmucker" target="_blank">Website</a> &nbsp; <a href="https://www.linkedin.com/in/mark-smucker-168144134/" target="_blank">LinkedIn</a> </td>
    </tr>
    <tr></tr>
    <tr>
        <td><img src="/charlie_photo.jpeg" /></td>
        <td><b>Charles L. A. Clarke</b> <br> University of Waterloo <br> Waterloo, Ontario, Canada <br> <a href="https://plg.uwaterloo.ca/~claclark/" target="_blank">Website</a> &nbsp; <a href="https://www.linkedin.com/in/charlie-clarke-7714a82/" target="_blank">LinkedIn</a> &nbsp; <a href="https://twitter.com/claclarke" target="_blank">X</a></td>
    </tr>
</table>

## Resources
- <a href="https://cor.inquirygroup.org/curriculum/collections/teaching-lateral-reading">Civic Online Reasoning - Teaching Lateral Reading</a>
