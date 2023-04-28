# wolfram-toolformer

A exploratory project to test out GPT's math ability when a) fine-tuned and b) augmented with the Wolfram Alpha API.

Project by Richard Ren & Clara Marty.

# File structure explanation

## Prompts and keys

**keys.json** -- if you'd like to replicate our results, get your OpenAI & Wolfram API Keys and place them in keys.json.

**prompts.json** -- prompts used. The **prompts.txt** file shows you what our prompts look like, in conjunction with other inputs, when fed into GPT models.

## Testing and training dataset

**/data**
- original MATH dataset by Hendrycks et al. (2021).

**/test** 
- modified "test set" of 210 data points
- ten were randomly selected per category (across all seven categories) and levels (out of levels 1-3) to create the test set.
- create-test.ipynb creates this folder.

**/train-finetuned-davinci**
- modified "train set" of 420 data points
- twenty were randomly selected per category (across all seven categories) and levels (out of levels 1-3) to create the train set.
- create-train.ipynb creates this folder.

**train-finetuned-davinci.jsonl**
- train set compiled into a .jsonl file with OpenAI token requirements; used to fine-tune the OpenAI model
- create-jsonl.ipynb creates this file.

## Creating, training, & testing individual models

No hyperparameter tuning was used, as these are OpenAI API models.

The ```results-{model_name}.ipynb``` notebooks run the model on the ```/test``` set and save modified results into ```/results-{model_name}```.

**baseline-davinci:**
- model: GPT-3-DaVinci
- prompt: "no-wolfram" field in prompts.json
- results-baseline-davinci.ipynb: runs on \test &rarr; save \results-baseline-davinci

**baseline-turbo:**
- model: ChatGPT-3.5-Turbo
- prompt: "no-wolfram" field in prompts.json
- results-baseline-turbo.ipynb: runs on \test &rarr; save \results-baseline-turbo

**finetuned-davinci:**
- GPT-3-DaVinci, fine-tuned
- prompt: "no-wolfram" field in prompts.json
- results-finetuned-davinci.ipynb:
    - runs fine-tuning process using train-finetuned-davinci.jsonl file (will have to manually write command prompt lines and OpenAI key, if replicating)
    - runs on \test &rarr; save \results-finetuned-davinci

**wolfram-davinci:**
- GPT-3-DaVinci, augmented with Wolfram's API in a two-step prompting process
- prompts: "wolframpt1" and "wolframpt2" fields in prompts.json
- results-wolfram-davinci.ipynb: runs on \test &rarr; save \results-wolfram-davinci

**wolfram-turbo:**
- ChatGPT-3.5-Turbo, augmented with Wolfram's API in a two-step prompting process
- prompts: "wolframpt1" and "wolframpt2" fields in prompts.json
- results-wolfram-turbo.ipynb: runs on \test &rarr; save \results-wolfram-turbo

## Analyzing Results

**analyze-results.ipynb** provides functions for:
- String parser that compares boxed LaTeX answers and uses SymPy to check whether they are the same.
- Proportion of answers boxed checker, to see how well the model followed instructions.
- Proportion of results which had a successful Wolfram Alpha call checker.
- Performance comparisons between baseline-davinci, baseline-turbo, fine-tuned davinci, wolfram-davinci, wolfram-turbo, wolfram-finetuned-davinci -- broken down by both difficulty levels and problem categories.

# Citation for MATH Dataset

[Hendrycks et al., 2021] Dan Hendrycks, Collin Burns, Saurav Kadavath, Akul Arora, Steven Basart, Eric Tang, Dawn Song, and Jacob Steinhardt. "Measuring Mathematical Problem Solving With the MATH Dataset." arXiv preprint arXiv:2103.03874 (2021).
