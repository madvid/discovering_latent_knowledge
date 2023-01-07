# Detailled on datasets

Here we go in detailled in the processes about datasets used in the paper *Discovering Latent Knowledge* [[1]](#1).


## Datasets used
Burns *et al.* used 10 datasets to perform tests on the caption of model belief of the truthness concept:
1. [**IMDB**](http://ai.stanford.edu/~amaas/data/sentiment/) [dataset](https://huggingface.co/datasets/imdb),
2. [**Amazon polarity**](https://docs.fast.ai/data.external.html#datasets) [dataset](https://huggingface.co/datasets/amazon_polarity),
3. [**AG-News**](link) [dataset](https://huggingface.co/datasets/ag_news),
4. [**DBpedia-14**](link) [dataset](https://huggingface.co/datasets/dbpedia_14),
5. **NLI-RTE** (subset of [**GLUE**](https://gluebenchmark.com/)) dataset ([RTE subset](https://huggingface.co/datasets/glue/viewer/rte/test)),
    > *when looking for the homage, we are reaching this [page](https://aclweb.org/aclwiki/Recognizing_Textual_Entailment) a page on ACLweb (Association for Computational Linguistics)*
6. **QNLI** (subset of [**GLUE**](https://gluebenchmark.com/)) dataset ([QNLI subset](https://huggingface.co/datasets/glue/viewer/qnli/test))
    > *when looking for the homepage, we are reaching [SQuAD2.0](https://rajpurkar.github.io/SQuAD-explorer/) page*,
7. [**COPA**](https://people.ict.usc.edu/~gordon/copa.html) dataset (curated versions can be found on huggingface: [COPA_nli](https://huggingface.co/datasets/pietrolesci/copa_nli)),
8. [**Story-Cloze**](https://cs.rochester.edu/nlp/rocstories/) [dataset](https://huggingface.co/datasets/story_cloze),
9. [**BoolQ**](https://github.com/google-research-datasets/boolean-questions) [dataset](https://huggingface.co/datasets/boolq),
10. [**PIQA**](https://yonatanbisk.com/piqa/) [dataset](https://huggingface.co/datasets/piqa).

Here some detailled about each datasets:


## Multiple shade of prompt
In this section we detailled all the different prompt of each dataset used by Burns *et. al.* during their experiments.
### IMDB dataset
At the time these lines are written, there is 11 prompts available, for each prompt, the name from promptsource is given plus the Jinja template and an example:
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
      * input: `{{text}} How does the viewer feel about the movie?`
      * output: `{{ answer_choices [label] }}` (where label is either `negqtive` or `positive`)
    * **example:**
      * Input
        ```
        I rented I AM CURIOUS-YELLOW from my video store because of all the controversy that surrounded it when it was first released in 1967. I
        [...] to study the meat and potatoes (no pun intended) of Swedish cinema. But really, this film doesn't have much of a plot. How does the viewer feel about the movie?
        ```
      * Target
        ```
        negative
        ```
9. **name:** Sentiment with choices
    * **Jinja template:**:
      * input:
        ```
        {{text}}
        Is this review {{"positive or negative"}}?
        ```
      * output: `{{answer_choices [label] }}` (where label is either `negative` or `positive`)
    * **example:**
      * Input
        ```
        I can't believe that those praising this movie herein aren't thinking of some other film. I was prepared for the possibility that this would be awful, but the script (or lack thereof) makes [...] the expense of those who seem to be asking for it should stick to Peter B's amazingly awful book, Killing of the Unicorn.
        Is this review positive or negative?
        ```
      * Target
        ```
        negative
        ```
10. **name:** Text Expressed Sentiment
    * **Jinja template:**:
      * input: `{{text}} What is the sentiment expressed in this text?`
      * output: `{{ answer_choices [label] }}` (where label is either `negative` or `positive`)
    * **example:**
      * Input
        ```
        This film was probably inspired by Godard's Masculin, féminin and I urge you to see that film instead. The film has two strong elements and those are, (1) the realistic acting (2) the impressive, [...] difference in ideals between the French and the Swedish society. A movie of its time, and place. 2/10. What is the sentiment expressed in this text?
        ```
      * Target
        ```
        negative
        ```
11. **name:** Writer Expressed Sentiment
    * **Jinja template:**:
      * input: `{{text}} What sentiment does the writer express for the movie?`
      * output: `{{ answer_choices [label] }}` (where label is either `negative` or `positive`)
    * **example:**
      * Input
        ```
        I rented this movie about 3 years ago, and it still stands out in my mind as the worst movie ever made. I don't think I ever finished it. It is worse than [...] to the video store and asked them why they even carry the movie and if I could get the hour of my life back. To this day it is the worst movie I have ever seen, and I have seen some pretty bad ones. What sentiment does the writer express for the movie?
        ```
      * Target
        ```
        negative
        ```

### Amazon polarity dataset
At the time these lines are written, there is 9 prompts available, for each prompt, the name from promptsource is given plus the Jinja template and an example:
1. **name:**: Is_this_review
  * **Jinja template:**:
    * input:
      ```
      Title: {{title}}
      Review: {{content}}
      Is the review positive or negative?
      ```
    * output: `{{answer_choices[label]}}` (where label is either `Negative` or `Positive`)
  * **example:**
    * Input
      ```
      Title: A FIVE STAR BOOK
      Review: I just finished reading Whisper of the Wicked saints. I fell in love with the caracters. I expected an average romance read, but
      instead I found one of my favorite books of all time. Just when I thought I could predict the outcome I was shocked ! The writting was so
      descriptive that my heart broke when Julia's did and I felt as if I was there with them instead of just a distant reader. If you are a lover
      of romance novels then this is a must read. Don't let the cover fool you this book is spectacular!
      Is the review positive or negative?
      ```
    * Target
      ```
      Positive
      ```
2. **name:**: User_recommend_this_product
  * **Jinja template:**:
    * input:
    ```
    Based on this review, would the user recommend this product?
    ===
    Review: {{content}}
    Answer:
    ```
    * output: `{{answer_choices[label]}}` (where label is either `No` or `Yes`)
  * **example:**
    * Input
      ```
      Based on this review, would the user recommend this product?
      ===
      Review: If you've played the game, you know how divine the music is! Every single song tells a story of the game, it's that good! The
      greatest songs are without a doubt, Chrono Cross: Time's Scar, Magical Dreamers: The Wind, The Stars, and the Sea and Radical Dreamers:
      Unstolen Jewel. (Translation varies) This music is perfect if you ask me, the best it can be. Yasunori Mitsuda just poured his heart on and
      wrote it down on paper.
      Answer:
      ```
    * Target
      ```
      Yes
      ```
3. **name:**: Is_this_product_review_positive
  * **Jinja template:**:
    * input:
    ```
    Is this product review positive?
    Title: {{title}}
    Review: {{content}}
    Answer:
    ```
    * output: `{{answer_choices[label]}}` (where label is either `No` or `Yes`)
  * **example:**
    * Input
      ```
      Is this product review positive?
      Title: Mind numbing
      Review: This game makes you do the same things over and over, it never holds my son's attention long enough to get to the next level. There
      is no choice of games you have to do every thing in order. We have another Rescue Heroes game and both my boys 3 and 6 love it, but this one
      is a dog!
      Answer:
      ```
    * Target
      ```
      No
      ```
4. **name:**: Is_this_review_negative
  * **Jinja template:**:
    * input:
    ```
    Title: {{title}}
    Review: {{content}}
    Is this product review negative?
    ```
    * output: `{{answer_choices[label]}}` (where label is either `Yes` or `No`)
  * **example:**
    * Input
      ```
      Title: Alaska sourdough
      Review: REad most of the book while visiting my brother in Alaska. Loved it. I am going to be making my sourdough starter soon. Book is full
      of great stories and recipes.
      Is this product review negative?
      ```
    * Target
      ```
      No
      ```
5. **name:**: convey_negative_or_positive_sentiment
  * **Jinja template:**:
    * input:
    ```
    Title: {{title}}
    Review: {{conten
    Does this product review convey a negative or positive sentiment?
    ```
    * output: `{{answer_choices[label]}}` (where label is either `Negative` or `Positive`)
  * **example:**
    * Input
      ```
      Title: Problem with charging smaller AAAs
      Review: I have had the charger for more than two years. It charges AA batteries just fine, but has a huge problem securing smaller AAA
      batteries. To charge the smaller batteries you need to flip down the little button at the positive end. In the beginning one of the four AAA
      batteries would pop up, and now three out of the four won't hold. The problem is the flip mechanism became loose, and any horizontal
      pressure would push the buttons back up. What I have to do now is using duct tape and a segment of crayon, apply the crayon on the buttons,
      and wrap the tape around. You know how painful that is.
      Does this product review convey a negative or positive sentiment?
      ```
    * Target
      ```
      Negative
      ```
6. **name:**: negative_or_positive_tone
  * **Jinja template:**:
    * input:
    ```
    Is there a negative or positive tone to this product review?
    ===
    Title: {{title}}
    Review: {{content}}
    Answer: 
    ```
    * output: `{{answer_choices[label]}}` (where label is either `Negative` or `Positive`)
  * **example:**
    * Input
      ```
      Is there a negative or positive tone to this product review?
      ===
      Title: Whispers of the Wicked Saints
      Review: This was a easy to read book that made me want to keep reading on and on, not easy to put down.It left me wanting to read the follow
      on, which I hope is coming soon. I used to read a lot but have gotten away from it. This book made me want to read again. Very enjoyable.
      Answer:
      ```
    * Target
      ```
      Positive
      ```
7. **name:**: user_satisfied
  * **Jinja template:**:
    * input:
    ```
    Here is a review left by a customer on a product. Would you say he was
    {{answer_choices[1]}} or {{answer_choices[0]}}?
    Title: {{title}}
    Review: {{content}}
    ```
    * output: `{{answer_choices[label]}}` (where label is either `dissatisfied` or `satisfied`)
  * **example:**
    * Input
      ```
      Here is a review left by a customer on a product. Would you say he was satisfied or dissatisfied?
      Title: Mind numbing
      Review: This game makes you do the same things over and over, it never holds my son's attention long enough to get to the next level. There
      is no choice of games you have to do every thing in order. We have another Rescue Heroes game and both my boys 3 and 6 love it, but this one
      is a dog!
      ```
    * Target
      ```
      dissatisfied
      ```
8. **name:**: would_you_buy
  * **Jinja template:**:
    * input:
    ```
    You are considering whether to buy a product. You look at the reviews.
    Would the following review {{answer_choices[0]}} or {{answer_choices[1]}} the
    chances of you buying the product?
    Review title: {{title}}
    Product review: {{content}}
    ```
    * output: `{{answer_choices[label]}}` (where label is either `decrease` or `increase`)
  * **example:**
    * Input
      ```
      You are considering whether to buy a product. You look at the reviews. Would the following review decrease or increase the chances of you
      buying the product?
      Review title: i liked this album more then i thought i would
      Product review: I heard a song or two and thought same o same o,but when i listened to songs like "blue angel","lanna" and 'mama" the hair
      just rose off my neck.Roy is trully an amazing singer with a talent you don't find much now days.
      ```
    * Target
      ```
      increase
      ```
9. **name:**: flattering_or_not
  * **Jinja template:**:
    * input:
    ```
    Title: {{title}}
    Product review: {{content}}
    Would you say this review depicts the product in a {{answer_choices[1]}} or
    {{answer_choices[0]}} light?
    ```
    * output: `{{answer_choices[label]}}` (where label is either `unflattering` or `flattering`)
  * **example:**
    * Input
    ```
    Title: Very Frustrating
    Product review: My three year old son was very excited to get this but after two attempts at playing it hasn't been touched since. You are
    not able to use the mouse through any of the games except for the "power-up segments" instead you are using the up-down arrows on the
    keyboard - too hard for him and not much fun. Very disapointed with this game and wish I could return it.
    Would you say this review depicts the product in a flattering or unflattering light?
    ```
    * Target
    ```
    unflattering
    ```

### AG-News dataset
At the time these lines are written, there is 7 prompts available, for each prompt, the name from promptsource is given plus the Jinja template and an example:
1. **name:**: ...
  * **Jinja template:**:
    * input:
      ```
      ...
      ```
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

### DBpedia-14 dataset
At the time these lines are written, there is 4 prompts available, for each prompt, the name from promptsource is given plus the Jinja template and an example:
1. **name:**: ...
  * **Jinja template:**:
    * input:
      ```
      ...
      ```
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

### NLI dataset - subset RTE
At the time these lines are written, there is x prompts available, for each prompt, the name from promptsource is given plus the Jinja template and an example:
1. **name:**: ...
  * **Jinja template:**:
    * input:
      ```
      ...
      ```
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

### QNLI dataset 
At the time these lines are written, there is x prompts available, for each prompt, the name from promptsource is given plus the Jinja template and an example:
1. **name:**: ...
  * **Jinja template:**:
    * input:
      ```
      ...
      ```
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

### COPA dataset
COPA dataset is not available in `promptsource`. The curation was made by the author of DLK himself. To keep a consistency in the prompt variety presentation the prompt is presented in an identical format that one would find it in promptsource:
1. **name:**: ...
  * **Jinja template:**:
    * input:
      ```
      ...
      ```
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

### Story-Cloze dataset
Also not in promptesource library, Bruns *et. al.* used 9 prompts (6 from Sanh *et. al.* [[3]](#3) plus 3 handcrafted). Each prompt is given following the same template as we did until now:
1. **name:**: ...
  * **Jinja template:**:
    * input:
      ```
      ...
      ```
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

### BoolQ dataset
At the time these lines are written, there is x prompts available, for each prompt, the name from promptsource is given plus the Jinja template and an example:
1. **name:**: ...
  * **Jinja template:**:
    * input:
      ```
      ...
      ```
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

### PIQA dataset
At the time these lines are written, there is x prompts available, for each prompt, the name from promptsource is given plus the Jinja template and an example:
1. **name:**: ...
  * **Jinja template:**:
    * input:
      ```
      ...
      ```
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



## Multiple shade of prompt
### PromptSource library
The authors use [promptsource library](https://github.com/bigscience-workshop/promptsource) to generate multiple versions of prompt forged from the examples in the dataset. 



## References
* <a id="1">[1]</a> 
C. Burns, H. Ye, D. Klein and  J. Steinhardt, *Discovering Latent Knowledge in Language Models Without Supervision*, [ArXiV](https://arxiv.org/abs/2212.03827), **2022**.
* <a id="2">[2]</a> 
A. Wang, A. Singh and J. Michael, F. Hill and O. Levy and S. R. Bowman, *GLUE: A Multi-Task Benchmark and Analysis Platform for Natural Language Understanding*, [ArXiV](https://arxiv.org/abs/1804.07461), **2019**.
*  <a id="3">[3]</a> 
V. Sanh, A. Webson, C. Raffel, S. H. Bach, L. Sutawika, Z. Alyafeai, A. Chaffin, A. Stiegler, T. L. Scao, A. Raja, M. Dey, M. S. Bari, C. Xu, U. Thakker, S. S. Sharma, E. Szczechla, T. Kim, G. Chhablani, N. Nayak, D. Datta, J. Chang, M. T-J Jiang, H. Wang, M. Manica, S. Shen, Z. X. Yong, H. Pandey, R. Bawden,  T. Wang, T. Neeraj, J. Rozen, A. Sharma, A. Santilli, T. Fevry, J. A. Fries, R. Teehan, T. Bers, S. Biderman, L. Gao, T. Wolf and A. M. Rush, *Multitask Prompted Training Enables Zero-Shot Task Generalization*, [ArXiV](https://arxiv.org/abs/2110.08207), **2021**.
<!---
*Discovering Latent Knowledge in Language Models Without Supervision*
@article{burns2022dl,
  title={Discovering Latent Knowledge in Language Models Without Supervision},
  author={Burns, Collin and Ye, Haotian and Klein, Dan and Steinhardt, Jacob},
  journal={ArXiV},
  year={2022}
}
/======================================================================/
*GLUE: A Multi-Task Benchmark and Analysis Platform for Natural Language Understanding*
@misc{wang2019glue,
      title={GLUE: A Multi-Task Benchmark and Analysis Platform for Natural Language Understanding}, 
      author={Alex Wang and Amanpreet Singh and Julian Michael and Felix Hill and Omer Levy and Samuel R. Bowman},
      year={2019},
      eprint={1804.07461},
      archivePrefix={arXiv},
      primaryClass={cs.CL}
}
/======================================================================/
*Multitask Prompted Training Enables Zero-Shot Task Generalization*
@misc{https://doi.org/10.48550/arxiv.2110.08207,
  doi = {10.48550/ARXIV.2110.08207},
  
  url = {https://arxiv.org/abs/2110.08207},
  
  author = {Sanh, Victor and Webson, Albert and Raffel, Colin and Bach, Stephen H. and Sutawika, Lintang and Alyafeai, Zaid and Chaffin, Antoine and Stiegler, Arnaud and Scao, Teven Le and Raja, Arun and Dey, Manan and Bari, M Saiful and Xu, Canwen and Thakker, Urmish and Sharma, Shanya Sharma and Szczechla, Eliza and Kim, Taewoon and Chhablani, Gunjan and Nayak, Nihal and Datta, Debajyoti and Chang, Jonathan and Jiang, Mike Tian-Jian and Wang, Han and Manica, Matteo and Shen, Sheng and Yong, Zheng Xin and Pandey, Harshit and Bawden, Rachel and Wang, Thomas and Neeraj, Trishala and Rozen, Jos and Sharma, Abheesht and Santilli, Andrea and Fevry, Thibault and Fries, Jason Alan and Teehan, Ryan and Bers, Tali and Biderman, Stella and Gao, Leo and Wolf, Thomas and Rush, Alexander M.},
  
  keywords = {Machine Learning (cs.LG), Computation and Language (cs.CL), FOS: Computer and information sciences, FOS: Computer and information sciences},
  
  title = {Multitask Prompted Training Enables Zero-Shot Task Generalization},
  
  publisher = {arXiv},
  
  year = {2021},
  
  copyright = {Creative Commons Attribution 4.0 International}
}
/======================================================================/

/======================================================================/

/======================================================================/

/======================================================================/

/======================================================================/

-->