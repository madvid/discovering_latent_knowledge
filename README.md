# Discovering Latent Knowledge Without Supervision

This repository contains the essential code for Discovering Latent Knowledge in Language Models Without Supervision.

<p align="center">
<img src="figure.png" width="750">
</p>

We introduce a method for discovering truth-like features directly from model activations in a purely unsupervised way.

## Abstract
> Existing techniques for training language models can be misaligned with the truth: if we train models with imitation learning, they may reproduce errors that humans make; if we train them to generate text that humans rate highly, they may output errors that human evaluators can't detect. We propose circumventing this issue by directly finding latent knowledge inside the internal activations of a language model in a purely unsupervised way. Specifically, we introduce a method for accurately answering yes-no questions given only unlabeled model activations. It works by finding a direction in activation space that satisfies logical consistency properties, such as that a statement and its negation have opposite truth values. We show that despite using no supervision and no model outputs, our method can recover diverse knowledge represented in large language models: across 6 models and 10 question-answering datasets, it outperforms zero-shot accuracy by 4\% on average. We also find that it cuts prompt sensitivity in half and continues to maintain high accuracy even when models are prompted to generate incorrect answers. Our results provide an initial step toward discovering what language models know, distinct from what they say, even when we don't have access to explicit ground truth labels.


## Datasets prompts
Accordind to the paper, several dataset were used to evaluate the CCS method. For each of the dataset, several variety of prompts were generated via the use of the library `promptsource`.

Here some details about all the prompt of IMDB dataset to get a better understanding (more about the other dataset in [dataset documentation](./docs/dataset.md))

### IMDB dataset
There is 11 prompts available, for each prompt, the name from promptsource is given plus the Jinja template and an example:
1. **name:** Movie Expressed Sentiment
    * **Jinja template:**:
      * input: `{{text}} The sentiment expressed for the movie is`
      * output: `{{ answer_choices [label] }}` (where label is either `positive` or `negative`)
    * **example:**
      * Input
        ```
        I rented I AM CURIOUS-YELLOW from my video store because of all the controversy that surrounded it when it was first released in 1967. I also heard [...] Swedish cinema. But really, this film doesn't have much of a plot.
        The sentiment expressed for the movie is
        ```
      * Target
        ```
        negative
        ```
2. **name:** Movie Expressed Sentiment 2
    * **Jinja template:**:
      * input: `The following movie review expresses what sentiment? {{text}}`
      * output: `{{ answer_choices [label] }}` (where label is either `positive` or `negative`)
    * **example:**
      * Input
        ```
        The following movie review expresses what sentiment? OK its not the best film I've ever seen but at the same time [...] if your after cheap laughs and you see it in pound land go by it.
        ```
      * Target
        ```
        negative
        ```
3. **name:** Negation template for positive and negative
    * **Jinja template:**:
      * input: `{{text}} This is definitely not a`
      * output: `{{ answer_choices [1-label]}} review.` (where label is either `positive` or `negative`)
    * **example:**
      * Input
        ```
        If only to avoid making this type of film in the future. This film is interesting as [...] spend one's time staring out a window at a tree growing.<br /><br /> This is definitely not a
        ```
      * Target
        ```
        positive review.
        ```
4. **name:** Reviewer Enjoyment
    * **Jinja template:**:
      * input: `{{text}} How does the reviewer feel about the movie?`
      * output: `{{ answer_choices [label] }}` (where label is either `They didn't like it!` or `They loved it`)
    * **example:**
      * Input
        ```
        "I Am Curious: Yellow" is a risible and pretentious steaming pile. It doesn't matter what one's political views are because this film can [...] culturally with the insides of women's bodies. How does the reviewer feel about the movie?
        ```
      * Target
        ```
        They didn't like it!
        ```
5. **name:** Reviewer Enjoyment Yes No
    * **Jinja template:**:
      * input: `{{text}} Did the reviewer enjoy the movie?`
      * output: `{{ answer_choices [label] }}` (where label is either `No` or `Yes`)
    * **example:**
      * Input
        ```
        I rented I AM CURIOUS-YELLOW from my video store because of all the controversy that surrounded it when it was first released in 1967. I also [...] to study the meat and potatoes (no pun intended) of Swedish cinema. But really, this film doesn't have much of a plot. Did the reviewer enjoy the movie?
        ```
      * Target
        ```
        No
        ```
6. **name:** Reviewer Expressed Sentiment
    * **Jinja template:**:
      * input: `{{text}} What is the sentiment expressed by the reviewer for the movie?`
      * output: `{{ answer_choices [label] }}` (where label is either `negative` or `positive`)
    * **example:**
      * Input
        ```
        This film was probably inspired by Godard's Masculin, féminin and I urge you to see that film instead.<br /><br />The film has two strong [...] A movie of its time, and place. 2/10. What is the sentiment expressed by the reviewer for the movie?
        ```
      * Target
        ```
        ...
        ```
7. **name:** Reviewer Opinion bad good choices
    * **Jinja template:**:
      * input: `{{text}} Did the reviewer find this movie {{"good or bad"}}?`
      * output: `{{ answer_choices [label] }}` (where label is either `bad` or `good`)
    * **example:**
      * Input
        ```
        "I Am Curious: Yellow" is a risible and pretentious steaming pile. It doesn't matter what one's political views are because this film can [...] culturally with the insides of women's bodies. Did the reviewer find this movie good or bad?
        ```
      * Target
        ```
        bad
        ```
8. **name:** Reviewer Sentiment Feeling
    * **Jinja template:**:
      * input: ``
      * output: `...` (where label is either `...` or `...`)
    * **example:**
      * Input
        ```
        ...
        ```
      * Target
        ```
        ...
        ```
9. **name:** Sentiment with choices
    * **Jinja template:**:
      * input: ``
      * output: `...` (where label is either `...` or `...`)
    * **example:**
      * Input
        ```
        ...
        ```
      * Target
        ```
        ...
        ```
10. **name:** Text Expressed Sentiment
    * **Jinja template:**:
      * input: ``
      * output: `...` (where label is either `...` or `...`)
    * **example:**
      * Input
        ```
        ...
        ```
      * Target
        ```
        ...
        ```
11. **name:** Writer Expressed Sentiment
    * **Jinja template:**:
      * input: ``
      * output: `...` (where label is either `...` or `...`)
    * **example:**
      * Input
        ```
        ...
        ```
      * Target
        ```
        ...
        ```


## Code

We provide three options for code:
1. A notebook walking through our main method in a simple way: `CCS.ipynb`. This may be the best place to start if you want to understand the method better and play around with it a bit.
2. More flexible and efficient scripts for using our method in different settings: `generate.py` and `evaluate.py` (both of which rely heavily on `utils.py`). This code is a polished and simplified version of the code used for the paper. This may be the best place to build on if you want to build on our work.
3. You can also download the original (more comprehensive, but also more complicated and less polished) code [here](https://openreview.net/attachment?id=ETKGuby0hcs&name=supplementary_material).

Below we provide usage details for our main python scripts (`generate.py` and `evaluate.py`).

### Generation
First, use `generate.py` for (1) creating contrast pairs, and (2) generating hidden states from a model. For example, you can run:
```
python generate.py --model_name deberta  --num_examples 400 --batch_size 40
```
or
```
python generate.py --model_name gpt-j --num_examples 100 --batch_size 20
```
or
```
CUDA_VISIBLE_DEVICES=0,1 python generate.py --parallelize --model_name t5-11b --num_examples 100 
```

To use the decoder of an encoder-decoder model (which we found is worse than the encoder for T5 and UnifiedQA, but better than the encoder for T0), specify `--use_decoder`.

There are also many optional flags for specifying the dataset (`--dataset`; the default is `imdb`), the cache directory for model weights (`--cache_dir`; the default is `None`), which prompt for the dataset to use (`--prompt_idx`; the default is `0`), where to save the hidden states for all layers in the model (`--all_layers`), and so on.

### Evaluation 
After generating hidden states, you can use `evaluate.py` for running our main method, CCS, on those hidden states. Simply run it with the same flags as you used when running `generate.py`, and it will load the correct hidden states for you. For example, if you ran `generate.py` with DeBERTa, generating 400 examples with a batch size of 40 (the first example in the Generation section), then you can run:
```
python evaluate.py --model_name deberta  --num_examples 400 --batch_size 40
```

In addition to evaluating the performance of CCS, `evaluate.py` also verifies that logistic regression (LR) accuracy is reasonable. This can diagnose why CCS performance may be low; if LR accuracy is low, that suggestions that the model's representations aren't good enough for CCS to work well.

### Requirements

This code base was tested on Python 3.7.5 and PyTorch 1.12. It also uses the [datasets](https://pypi.org/project/datasets/) and [promptsource](https://github.com/bigscience-workshop/promptsource) packages for loading and formatting datasets. 

## Citation

If you find this work helpful, please consider citing our paper:

    @article{burns2022dl,
      title={Discovering Latent Knowledge in Language Models Without Supervision},
      author={Burns, Collin and Ye, Haotian and Klein, Dan and Steinhardt, Jacob},
      journal={ArXiV},
      year={2022}
    }

