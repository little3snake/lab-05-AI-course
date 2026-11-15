# Lab 05 — Conclusion

<!--
Read this before you write.

This file is graded by your instructor, by hand, and it is worth 6 of the 15 points.

- Type it yourself. Do not use a chatbot, an AI assistant, an AI autocomplete or a "rewrite my
  text" tool for any part of it, not even for grammar. English that is plain and a little
  clumsy but yours is better than polished text that is not.
- Use your own numbers. Every number you quote must be one your own notebook printed. Copy
  the number exactly as it appeared. A number your notebook did not print is marked as wrong.
- Answer in your own words, about 80-200 words per question. A short answer that uses your
  numbers beats a long answer that could have been written by anyone.
- Delete these comment lines if you like. Keep the three headings and the last line.
-->

## 1. Which layer changed your results the most?

Give the numbers before -> after for that layer (correct / wrong / no answer, out of 20) and say
whether it changed what the agent *can do* or only what it *admits*. If no layer changed the
number of correct answers, say so and explain what the layers did change instead.


In my work, 1 layer changed the results the most.

Before layer 1 it was like that:

bare           correct 16/24  wrong  4  no answer  4  95% [0.47, 0.82]  model calls  51  tokens  39536  seconds   97.0

after 1 layer:

layer 1        correct 18/24  wrong  6  no answer  0  95% [0.55, 0.88]  model calls  62  tokens  48708  seconds  104.9

The rule when we return errors to the model allows the model to try again instead of ending this action with error. But it's not garanty the correctness.
Layer 2 did not change the outcomes. In other case, layer 3 reduced the wrong answers 6 -> 4 but increased the the questions without any asnwer (0 -> 2). It did not rise the number of correct answers,however it change the asnwer accepted by system.



## 2. One failure the full harness did not fix

Pick one question that the full harness (layer 3) still gets wrong or refuses. Give its id
(q01 ... q20). Open its trace in the notebook: at which step did it go wrong, and what did
the model do instead? Name one thing you would add to the harness, and what it would cost
(extra model calls, tokens, time, or a new way to fail).


I chose question q08 (which region had the lowest average monthly wage in 2024). Despite the full harness, agent called compare_regions (correct indicator and correct year) but answered Jambyl (it's wrong answer). In comparison, the workflow answered Mangystau. The problem happened when the model received the tool output and after that selected the region. After layer 3, model change "Jambyl" to "ANSWER: Jambyl". This fixed the format but can't cope with wrong answer issue. I think I would implement the verification that find whether the chosen region has min or max of returned values, depending on the question. That requires extra code and could fail if the requested ranking is interpreted incorrectly.


## 3. Workflow or agent for the monthly ranking?

Dala Insight wants to publish, every month, "which region is highest and which is lowest on one
indicator in one year". Would you ship the fixed workflow or the agent? Use the accuracy, the
number of model calls, the tokens and the time you measured in Part 1. Then say in one sentence
what kind of question would make you choose the other one.


I would choose the fixed workflow for rankings every month because it's more predictable (the results) and the request has the clear structure like an indicator, a year, highest/lowest region to choose. Let's explain my desicion on part 1 data. The workflow answered 13/15 correctly (0 wrong and 2 unanswered), while the agent answered only 9/15 (2 wrong and 4 unanswered). As for resources, the workflow used 15 model calls, 3852 tokens and 15.7 sec, compared to agent with 26 calls, 19835 tokens and 51.6 sec.


the 15 fixed-shape questions:

workflow       correct 13/15  wrong  0  no answer  2  95% [0.62, 0.96]  model calls  15  tokens   3852  seconds   15.7

agent (bare)   correct  9/15  wrong  2  no answer  4  95% [0.36, 0.80]  model calls  26  tokens  19835  seconds   51.6


The difference is obvious. This numbers are suitable for this structured task. 
If we talk about the agent, it's better with questions that require flexible sequence of retrieval and calculation steps. The workflow just doesn't support such tasks.

---

I wrote this file myself, without any AI tool.
