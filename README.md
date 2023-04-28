# wolfram-toolformer

A exploratory project to test out GPT's math ability when a) fine-tuned and b) augmented with the Wolfram Alpha API.

Project by Richard Ren & Clara Marty.

# File structure explanation

## Prompts and Keys

**keys.json** -- if you'd like to replicate our results, get your OpenAI & Wolfram API Keys and place them in keys.json.

**prompts.json** -- you can modify the prompts used for baseline/finetuned and wolfram here. The **prompts.txt** file shows you what our prompts look like, in conjunction with other inputs, when fed into GPT models.

## Testing and training dataset
- We limited the number of testing and training datasets because of financial constraints (i.e. OpenAI credits are really expensive 💸💵💰).

**/data** -- original MATH dataset by Hendrycks et al. (2021). Each problem corresponds to an individual .json file.

**/test** -- modified "test set", a subset of the original MATH test dataset. Contains 210 data points; ten were randomly selected per category (across all seven categories) and levels (out of levels 1-3) to create the test set. **create-test.ipynb** creates this folder. Each problem corresponds to an individual .json file.

**/train-finetuned-davinci** -- modified "train set", a subset of the original MATH train dataset. Used to finetune davinci. Contains 420 data points; twenty were randomly selected per category (across all seven categories) and levels (out of levels 1-3) to create the train set. **create-train.ipynb** creates this folder. Each problem corresponds to an individual .json file.

**train-finetuned-davinci.jsonl** -- train set, compiled into a .jsonl file with OpenAI token requirements so it can be used to fine-tune the OpenAI model. **create-jsonl.ipynb** creates this folder, referencing files in /train-finetuned-davinci.

## Creating, training, & testing individual models

No hyperparameter tuning was used, as these are OpenAI API models.

### baseline-davinci: 
- GPT-3-DaVinci
- prompt: "no-wolfram" field in prompts.json
- **results-baseline-davinci.ipynb** runs the davinci model on the /test set --> saves modified results into **\results-baseline-davinci**.

### baseline-turbo: 
- ChatGPT-3.5-Turbo
- prompt: "no-wolfram" field in prompts.json
- **results-baseline-turbo.ipynb** runs the turbo model on the /test set --> saves modified results into **\results-baseline-turbo**.

### finetuned-davinci: 
- GPT-3-DaVinci, fine-tuned
- prompt: "no-wolfram" field in prompts.json
- **results-finetuned-davinci.ipynb** does two things:
    - runs the fine-tuning process via the  **train-finetuned-davinci.jsonl** file (will have to manually put in command prompt lines and OpenAI key)
    - runs the turbo model on the /test set --> saves modified results into the **\results-finetuned-davinci** folder.

### wolfram-davinci: 
- GPT-3-DaVinci, augmented with Wolfram's API in a two-step prompting process
- prompts: "wolframpt1" and "wolframpt2" fields in prompts.json
- **results-wolfram-davinci.ipynb** runs the davinci model on the /test set --> saves modified results into **\results-wolfram-davinci**.

### wolfram-turbo: 
- ChatGPT-3.5-Turbo, augmented with Wolfram's API in a two-step prompting process
- prompts: "wolframpt1" and "wolframpt2" fields in prompts.json
- **results-wolfram-turbo.ipynb** runs the davinci model on the /test set --> saves modified results into **\results-wolfram-turbo**.

## Analyzing Results

**analyze-results.ipynb** provides functions for:
- String parser that compares boxed LaTeX answers and uses SymPy to check whether they are the same.
- Proportion of answers boxed checker, to see how well the model followed instructions.
- Proportion of results which had a successful Wolfram Alpha call checker.
- Performance comparisons between baseline-davinci, baseline-turbo, fine-tuned davinci, wolfram-davinci, wolfram-turbo, wolfram-finetuned-davinci -- broken down by both difficulty levels and problem categories.

# Citation for MATH Dataset

[Hendrycks et al., 2021] Dan Hendrycks, Collin Burns, Saurav Kadavath, Akul Arora, Steven Basart, Eric Tang, Dawn Song, and Jacob Steinhardt. "Measuring Mathematical Problem Solving With the MATH Dataset." arXiv preprint arXiv:2103.03874 (2021).
