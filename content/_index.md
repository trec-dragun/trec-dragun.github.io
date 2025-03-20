  ---
weight: 1
title: 2025
type: docs
---

# TREC 2025 DRAGUN Track Guidelines

***D**etection, **R**etrieval, and **A**ugmented **G**eneration for **U**nderstanding **N**ews*

**Below are draft guidelines. Please join our [Slack channel](https://nistgov.slack.com/archives/C08ED5JBXHT) for updates and discussions.**

<!-- <img src="/dragun.png" alt="drawing" width="50%"/> -->

## Overview

Welcome to the TREC 2025 DRAGUN Track, a continuation of the previous [TREC 2024 Lateral Reading Track](./docs/TREC_2024_LR/). The goal of this track is to **support people's trustworthiness assessment of online news**. There are two tasks: (1) **Question Generation** and (2) **Report Generation**. The Question Generation task focuses on <ins>detecting critical questions readers should consider</ins> during their trustworthiness assessment. Those questions should guide readers' investigation, such as the bias or motivations of the source and narratives from other sources. This is similar to LLMs generating search queries in the “Deep Research” mode. Meanwhile, the Report Generation task involves <ins>creating a well-attributed and comprehensive report</ins> that provides readers with the background and context they need for a more informed trustworthiness evaluation. Both tasks run in parallel, with the same submission due date. This track differs from traditional fact-checking by aiming to assist readers in making their trustworthiness assessments from a neutral perspective, helping them to form their own judgments rather than dictating conclusions.

## Participation and Communication

Please follow the TREC 2025 registration guidelines from their [Call for Participation](https://trec.nist.gov/cfp.html). After completing the registration process, you should receive an invitation to NIST’s workspace and be granted access to our primary communication channel, [#trec-dragun-2025]((https://nistgov.slack.com/archives/C08ED5JBXHT)). This channel will serve as the main hub for discussions and important announcements.

## Data

- Web Collection: This track will use [MS MARCO V2.1](https://trec-rag.github.io/annoucements/2024-corpus-finalization/) (Segmented) as the document collection, the same one used by the [RAG track](https://trec-rag.github.io/), which contains about 114 million segments from around 11 million web documents. This collection can be downloaded from [here](https://trec-rag.github.io/annoucements/2024-corpus-finalization/#where-can-i-find-the-corpus).

- News Articles: We will release some selected target news articles (or "topics"), published from various sources.

## Tasks

Assume there is a (general public) reader who is looking through online news. This track has the following two parallel tasks with the same submission due date. Participants can choose to engage in either one or both tasks.

### Task 1: Question Generation

For each of the topics (i.e., target news articles), participants need to produce some important questions that the reader should ask to evaluate its trustworthiness, ranked from the most important to the least important. Those questions should meet the following requirements.

- Should be at most 120 characters long.
- Compound questions should be avoided, e.g. who is X and when did Y happen? In general, each question should focus on a single topic.
- Should be reasonably expected to work as a self-contained search query without reference to the article.

Participants should put all the questions for those articles into a single file, using the format below, and submit it to NIST via [Evalbase](https://ir.nist.gov/evalbase/).

- It should be a tab-separated file.
- It should be encoded in UTF-8.
- Each line consists of the following tab-separated fields in this order: `topic_id`, `run_id`, `rank`, `question`.
    - `topic_id`: docid of the target news article.
    - `run_id`: Run ID that uniquely identifies the team and the method used to produce the run. Each run should have a different ID. IDs for runs submitted by the same team must all share a common prefix to identify the team across runs. 
    - `rank`: Your rank for the question, starting from 1.
    - `question`: Question in plaintext, with no tabs or newlines.

Submissions can be either **manual** (involving human intervention to generate questions, e.g., hiring people to produce questions or manually selecting questions from a candidate list of questions produced by algorithms or other human involvement in the question generation process) or **automatic** (automatic systems that produce questions without the need of human input beyond the construction of the systems). Teams submitting automatic runs should make a good-faith effort not to read or study the articles.

## Task 2: Report Generation

This is the core task of this track: generate a well-attributed report to provide more background and context (e.g., the bias and motivation of the source, narratives from other sources, etc.) to help readers better judge the trustworthiness of given news articles. It is a RAG-style task, with a fixed query: tell me what I should know about this article to better assess its trustworthiness, but a varying context: the news article. Each sentence should have at most three references. The total length of the generated reports should be within 250 words.

As organizers, we will provide a starter kit for participants not interested in the retrieval part in the pipeline of report generation.

Similar to Task 1, runs may be either **automatic** or **manual**. We use a subset of required fields from the shared submission format among RAG-related tracks in TREC 2025. One submission (run) would be a UTF-8-encoded JSONL file, each line being a JSON object for a topic. Participants should follow this format and submit their runs to NIST via [Evalbase](https://ir.nist.gov/evalbase/).

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

Above is an example of one JSONL object, a line from the final JSONL run file, with the following fields:
- `metadata`: Metadata for this run, should be the same across lines (topics).
- `team_id`: Unique team ID.
- `run_id`: Run ID that uniquely identifies the team and the method used to produce the run. Each run should have a different ID. IDs for runs submitted by the same team must all share a common prefix to identify the team across runs.
- `topic_id`: docid of the target news article (topic).
- `type`: Either “automatic” or “manual”.
- `use_starter_kit`: 1 or 0 to indicate whether the run is based on the starter kit or not.
- `responses`: Each of its objects is a generated sentence. Each object contains:
- `text`: One generated sentence in plaintext. 250-word limit is on this field across all objects.
- `citations`: A list of docids of segments that this sentence is based on, with at most 3 citations. A sentence can have no citation. Their order does not matter.


 
## Schedule

- **News Article Release**: Early May
- **Task 1 and Task 2 <u>Submission Due</u>**: Middle August
- **Result Release**: Late September
- **Notebook <u>Paper Due</u>**: Late October
- **TREC 2025 Conference**: November 17-21 at NIST in Gaithersburg, MD, USA

## Evaluation

NIST assessors will examine the trustworthiness of the news articles, produce questions (similar to Task 1), and find expected answers by searching online, which will serve as rubrics for evaluating submissions for both tasks. Participant questions for Task 1 will be automatically graded based on the extent to which they align with assessors’ questions. Participant reports for Task 2 will be evaluated manually by NIST assessors, focusing on the number of assessor questions each report answers. Details are to be determined.

## Q&A

1. Is there a limit on how many runs each group can submit?\
*Participating groups will be allowed to submit as many runs as they like, but they need authorization from the track organizers before submitting more than 10 runs per task. Not all runs are evaluated and groups need to specify a preference ordering during submission.*

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

