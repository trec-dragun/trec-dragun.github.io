---
weight: 1
title: 2025
type: docs
---

# TREC 2025 DRAGUN Track Guidelines

***D**etection, **R**etrieval, and **A**ugmented **G**eneration for **U**nderstanding **N**ews*

<u>**Important announcement: The submission deadline has been extended by one week to August 22nd.**</u>

<!--**Below are draft guidelines. Please join our [Slack channel](https://nistgov.slack.com/archives/C08ED5JBXHT) for updates and discussions.**-->

<!-- <img src="/dragun.png" alt="drawing" width="50%"/> -->

## Overview

Welcome to the TREC 2025 DRAGUN Track, the successor to the [TREC 2024 Lateral Reading Track](./docs/TREC_2024_LR/). This track aims to **support readers in assessing the trustworthiness of online news**. There are two tasks: (1) **Question Generation** and (2) **Report Generation**. 

**Question Generation** focuses on <ins>identifying critical questions that readers should consider</ins> when evaluating news trustworthiness. These questions should guide further investigation, such as examining source bias, credentials, and cross-referencing with other sources, much like how LLMs generate queries in the "Deep Research" mode.

**Report Generation** involves <ins>creating a well-attributed and comprehensive report</ins> that provides readers with background and context for a more informed trustworthiness evaluation. This report is expected to address the most important questions from Task 1. 

Both tasks run in parallel, sharing the same submission deadline. Unlike traditional fact-checking, this track is designed to help readers form their own judgments by offering multi-source context from a neutral perspective, rather than establishing an "absolute truth" and delivering a verdict.

## Participation and Communication

Please follow the TREC 2025 registration guidelines from their <a href="https://trec.nist.gov/cfp.html" target="_blank">Call for Participation</a>. After completing the registration process, you should receive an invitation to NIST’s workspace and be granted access to our primary communication channel: <a href="https://nistgov.slack.com/archives/C08ED5JBXHT" target="_blank">#trec-dragun-2025</a>. This channel will serve as the main hub for discussions and important announcements.

## Data

- **Web Collection**: This track will use <a href="https://trec-rag.github.io/annoucements/2024-corpus-finalization//#ms-marco-v21-document-corpus---segmented-version" target="_blank">MS MARCO V2.1 (Segmented)</a> as the web collection for the report generation task, the same corpus used by the <a href="https://trec-rag.github.io/" target="_blank">TREC Retrieval-Augmented Generation Track</a>. This segmented collection consists of approximately 114 million segments derived from around 11 million web documents. The collection is available for download <a href="https://trec-rag.github.io/annoucements/2024-corpus-finalization/#where-can-i-find-the-corpus" target="_blank">here</a>.

- **News Articles**: <a href="/trec-2025-dragun-topics.jsonl" target="_blank">trec-2025-dragun-topics.jsonl</a> provides **30** selected target news articles (i.e., topics), each on a different subject and from a range of sources. These articles were extracted from the `MS MARCO V2.1 Document` collection. Each line in this JSONL file includes the following fields for a selected news article: `docid`, `url`, `title`, `headings`, and `body`.

## Tasks

Assume there is a (general public) reader who is viewing each of these 30 news articles. This track has the following two parallel tasks with the same submission due date. Participants may choose to engage in either one or both tasks. As organizers, we have provided a <a href="https://github.com/trec-dragun/2025-starter-kit" target="_blank">starter kit</a> for participants to modify or build upon. Participants are also welcome to develop their own systems from scratch.
<!--Unlike traditional fact-checking tasks, which often aim to establish an "absolute truth" and deliver a verdict, DRAGUN emphasizes offering readers multi-source context to help them form their own informed judgments. -->
Note: If any article includes reader comments at the bottom of the page (which should be few, if any), disregard these comments for both tasks, i.e., reader comments following the articles are out of scope. 

### Task 1: Question Generation

For each topic (i.e., target news article), participants need to generate **10** critical and investigative questions that a thoughtful reader should look into when assessing its trustworthiness, on aspects such as source bias, motivation, or breadth of viewpoints, including centrist and right wing viewpoints. The generated questions should be <ins>ranked from most to least important</ins>, similar to the "Deep Research" mode, where LLMs (search agents) proactively generate queries to guide the investigation of an article's trustworthiness. Participants may <ins>use any resources</ins> (online or offline) beyond the specified web collection to develop their systems for this task.

Questions should meet the following requirements:

- They should be at most 300 characters long.
- Avoid compound questions (e.g., *Who is X and when did Y happen?* ). Each question should focus on a single topic.
- Avoid overly general or ambiguous questions, in the context of the article (e.g., *Are there other sources corroborating the details presented in this article?* ).

All questions for the articles should be compiled into a single file, formatted as follows, and submitted to NIST via <a href="https://ir.nist.gov/evalbase/" target="_blank">Evalbase</a>:

- Tab-separated file, encoded in UTF-8.
- Each line contains the following fields, in this order: `topic_id`, `team_id`, `run_id`, `rank`, `question`.
    - `topic_id`: `docid` of the target news article.
    - `team_id`: Unique identifier for the team.
    - `run_id`: Unique identifier for the run, specifying both the team and the method used. Each run must have a different `run_id`, and all `run_id`s submitted by the same team should share a common prefix to associate them with the team. 
    - `rank`: Rank for the question, from 1 to 10.
    - `question`: Question in plaintext, with no tabs or newlines.

Submissions can be either **manual** (involving human intervention to generate questions, such as hiring people to produce questions or manually selecting questions from candidate questions generated by algorithms, or other human involvement) or **automatic** (produced entirely by automatic systems without human input beyond the construction of the systems). Teams submitting automatic runs should make a good-faith effort not to read or study the articles.

### Task 2: Report Generation

This is the **core task** of this track: for each topic (i.e., target news article), generate a well-attributed report to provide more background and context, such as the bias and motivation of the source, details on the cited support in the article, and viewpoints from other sources, to help readers form their own trustworthiness assessments. These reports are expected to thoughtfully address the most important questions from Task 1. It can be viewed as a RAG-style task, with a fixed query: *tell me what I should know about this article to better assess its trustworthiness*, but a varying context: *the news article*.

Each sentence should have **at most three references** (i.e., segment's `docid` from `MS MARCO V2.1 (Segmented)`). The total length of the generated reports should be **within 250 words**. Note that it is possible that no answer exists in `MS MARCO V2.1 (Segmented)` for some generated questions from Task 1. In such cases, it is suggested to skip those questions in the generated reports, as answers without citations (not grounded on the web collection) are not desired for RAG systems.

Similar to Task 1, runs may be either **automatic** or **manual**. We use a subset of the required fields from the shared submission format among RAG-related tracks in TREC 2025. One submission (run) should be a UTF-8-encoded JSONL file, with each line being a JSON object for a topic. Participants should follow this format and submit their runs to NIST via <a href="https://ir.nist.gov/evalbase/" target="_blank">Evalbase</a>.

```json
{
	"metadata": {
		"team_id": "organizers",
		"run_id": "organizers-run-example", 
		"topic_id": "msmarco_v2.1_doc_xx_xxxxx0",
		"type": "automatic",
		"use_starter_kit": 0
	},
	"responses": [
		{
			"text": "This is the first sentence.",
			"citations": [
				"msmarco_v2.1_doc_xx_xxxxxx1#x_xxxxxx3",
				"msmarco_v2.1_doc_xx_xxxxxx2#x_xxxxxx4",
			]
		},
		{
			"text": "This is the second sentence.",
			"citations": []
		}
	]
}
```

Above is an example line from the final JSONL run file, with the following fields:
- `metadata`: Metadata for this run. It should be the same across all 30 lines (topics).
- `team_id`: Unique identifier for the team.
- `run_id`: Unique identifier for the run, specifying both the team and the method used. Each run must have a different `run_id`, and all `run_id`s submitted by the same team should share a common prefix to associate them with the team.
- `topic_id`: The `docid` of the target news article (the topic).
- `type`: Indicates whether the run is "automatic" or "manual".
- `use_starter_kit`: Set to 1 if the run is based on the starter kit, or 0 if not.
- `responses`: A list of objects, each representing a generated sentence with:
	- `text`: One generated sentence in plaintext. All `text` fields in the list must together not exceed a total of 250 words.
	- `citations`: A list of up to 3 `docid`s of segments that this sentence is based on. The citations for a sentence may be empty, and their order does not matter.

## Schedule

- **News Articles (Topics) Released**:  May 7
- -> **Task 1 and Task 2 <u>Submission Due</u>**: August 22 (extended)
- **Evaluation Results Release**: Late September
- **Notebook <u>Paper Due</u>**: Late October
- **TREC 2025 Conference**: November 17-21 at NIST in Gaithersburg, MD, USA

## Evaluation

NIST assessors will evaluate the trustworthiness of each of the 30 news articles, generate questions (similar to those in Task 1), and identify candidate answers through online searches. These will serve as rubrics for evaluating submissions to both tasks. For Task 1, participant questions will be automatically graded based on their alignment with the assessors’ questions. For Task 2, participant reports will be manually evaluated by NIST assessors, focusing on how many assessor questions each report answers. If no answer can be found in `MS MARCO V2.1 (Segmented)` for an assessor question, that question will be dropped from the rubrics during report evaluation. Further details are to be determined.

## Q&A

1. Is there a limit on how many runs each team can submit?\
*Participating teams are allowed to submit as many runs as they like, but they need authorization from the track organizers before submitting more than 10 runs per task. Since not all runs are guaranteed to be evaluated, teams also need to specify a priority ordering during submission.*

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
        <td><b>Dake Zhang</b> <br> University of Waterloo <br> Waterloo, Ontario, Canada <br> <a href="https://zhangdake.tech/" target="_blank">Website</a> &nbsp; <a href="https://www.linkedin.com/in/zhangdake/" target="_blank">LinkedIn</a> &nbsp; <a href="https://twitter.com/ZhangDake1998" target="_blank">Twitter</a></td>
    </tr>
    <tr></tr>
    <tr>
        <td><img src="https://scholar.googleusercontent.com/citations?view_op=medium_photo&user=BgiGGQQAAAAJ&citpid=4" alt="Mark D. Smucker" title="picture_mark_smucker" /></td>
        <td><b>Mark D. Smucker</b> <br> University of Waterloo <br> Waterloo, Ontario, Canada <br> <a href="https://uwaterloo.ca/management-science-engineering/profile/msmucker" target="_blank">Website</a> &nbsp; <a href="https://www.linkedin.com/in/mark-smucker-168144134/" target="_blank">LinkedIn</a> </td>
    </tr>
    <tr></tr>
    <tr>
        <td><img src="/charlie_photo.jpeg" /></td>
        <td><b>Charles L. A. Clarke</b> <br> University of Waterloo <br> Waterloo, Ontario, Canada <br> <a href="https://plg.uwaterloo.ca/~claclark/" target="_blank">Website</a> &nbsp; <a href="https://www.linkedin.com/in/charlie-clarke-7714a82/" target="_blank">LinkedIn</a> &nbsp; <a href="https://twitter.com/claclarke" target="_blank">Twitter</a></td>
    </tr>
</table>

## Other RAG-related Tracks in TREC 2025

There are other tracks in TREC 2025 that feature RAG tasks, with a similar submission format to encourage cross-participation. Participants are welcome to adapt their RAG systems for those tracks to explore their performance across diverse scenarios.

- <a href="https://trec-rag.github.io/" target="_blank">TREC 2025 Retrieval-Augmented Generation Track</a>
- <a href="https://trec-ragtime.github.io/" target="_blank">TREC 2025 RAGTIME Track</a>
- <a href="https://www.trecikat.com/" target="_blank">TREC 2025 Interactive Knowledge Assistance Track (iKAT)</a>
- <a href="https://trec-biogen.github.io/docs/" target="_blank">TREC 2025 Biomedical Generative Retrieval Track (BioGen)</a>