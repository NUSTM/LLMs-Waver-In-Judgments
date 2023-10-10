# Ask Again, Then Fail: Large Language Models' Vacillations in Judgement

<i>Qiming Xie, Zengzhi Wang, Yi Feng, Rui Xia</i>

<i>Nanjing University of Science and Technology, China</i>


 📄 [[Paper]](https://arxiv.org/abs/2310.02174) &nbsp; 🖥️ [[Homepage on PaperWithCode]](https://paperswithcode.com/paper/ask-again-then-fail-large-language-models)


 ## Quick links

  - [Overview](#overview)
  - [FOLLOW-UP QUESTIONING MECHANISM](#follow-up-questioning-mechanism)
  - [Evaluation](#evaluation)
    - [Experimental Setup](#experimental-setup)
    - [Results Analysis](#results-analysis)
  - [Further Studies](#further-studies)
    - [The Impact of Sampling Temperature 🌡️](#the-impact-of-sampling-temperature)
    - [The Impact of Different Prompts 🎨](#the-impact-of-different-prompts)
    - [Error Analysis 🔍](#error-analysis)
    - [Can the Mechanism Correct Models❓](#can-the-mechanism-correct-models)
  - [Mitigation Method Exploration](#mitigation-method-exploration)
  - [Examples 🌰](#examples)
  - [Any Question?](#any-questions)
  - [Citation](#citation)



## Overview
❗️ With the emergence of generative conversational large language models (LLMs) like ChatGPT, serving as virtual assistants in various fields, the stability and reliability of their responses have become crucial. **However, during usage, it has been observed that these models tend to waver in their judgements when confronted with follow-up questions from users expressing skepticism or disagreement.** [🌰 Like these examples 🌰](#examples)

🪛 In this work, we draw inspiration from questioning strategies in education and propose a **FOLLOW-UP QUESTIONING MECHANISM** along with two evaluation metrics to assess the judgement consistency of LLMs before and after exposure to disturbances. We evaluate the judgement consistency of ChatGPT, PaLM2-Bison, and Vicuna-13B under this mechanism across eight reasoning benchmarks. Empirical results show that even when the initial answers are correct, judgement consistency sharply decreases when LLMs face disturbances such as questioning, negation, or misleading. 

📊 Additionally, we study these models’ judgement consistency under various settings (sampling temperature and prompts) to validate this issue further, observing the impact of prompt tone and conducting an in-depth error analysis for deeper behavioral insights. Furthermore, we also explore several prompting methods to mitigate this issue and demonstrate their effectiveness.

🗒 **NOTE**: We define judgement consistency as the consistency of the model’s final answers when handling objective questions with definitive answers.



## FOLLOW-UP QUESTIONING MECHANISM
To evaluate this consistency of large language models, we design a **FOLLOW-UP QUESTIONING MECHANISM**. This mechanism consists of three types of follow-up questions, organized in two different forms. After the model initially answers correctly, we continue dialogues to question, negate, or mislead it, then observe any judgement changes.
<div align=center> <img alt="method" src="https://github.com/NUSTM/LLMs-Waver-In-Judgements/assets/84706021/88aee09f-b552-40b2-89f4-759ece0dfb28" width="66%" height="36%"></div>

The prompts we used in the experiment. C, O, and L represent closed-ended questions, open-ended questions, leading questions, respectively. {M_A} denotes the misleading answers.
<div align=center> <img alt="prompts-a" src="https://github.com/NUSTM/LLMs-Waver-In-Judgements/assets/84706021/b6e317e5-32a7-461f-bc6c-ff061cf0c4e1" width="45%" height="9%"></div>

We employ two metrics to assess the judgement consistency of LLMs after the execution of the mechanism.
- **Modification (M.)** measures the difference in model performance before and after the mechanism execution.
- **Modification Rate (M. Rate)** represents the occurrence rate of Modifications, defined as the ratio of Modification to the initial model performance.
<div align=center> <img alt="metrics" src="https://github.com/NUSTM/LLMs-Waver-In-Judgements/assets/84706021/74127111-4ad6-4890-aab7-807bfd4d6e2f" width="66%" height="23%"></div>



## Evaluation

### Experimental Setup
- Models
  - ChatGPT (gpt-3.5-turbo-0301) with temperature at 0.5.
  - PaLM2-Bison (chat-bison-001) with temperature at 0.4.
  - Vicuna-13b (Vicuna-13B-v1.3) with temperature at 0.7.
- Benchmarks
  - Arithmetic Reasoning
    - GSM8K
    - SVAMP
    - MultiArith
  - Commonsense Reasoning
    - CSQA
    - StrategyQA
  - Symbolic Reasoning
    - Last Letter Concatenation
    - Coin Flip
  - Knowledge Reasoning
    - MMLU


### Results
The results of ChatGPT in Direct Form.
<div align=center> <img alt="results-chatgpt-d" src="https://github.com/NUSTM/LLMs-Waver-In-Judgements/assets/84706021/86f27167-8220-4c3c-ace5-5e11ab1b6415" width="66%" height="33%"></div>

The results of ChatGPT in Progressive Form.
<div align=center> <img alt="results-chatgpt-p" src="https://github.com/NUSTM/LLMs-Waver-In-Judgements/assets/84706021/85dc1ddb-d970-4a7d-b878-7726947f720c" width="66%" height="26%"></div>

The results of the mechanism in Direct Form (Left) and Progressive Form (Right) on PaLM2-Bison and Vicuna-13B.
<div align=center> <img alt="results-palm-vicuna-d-p" src="https://github.com/NUSTM/LLMs-Waver-In-Judgements/assets/84706021/94f635d7-f66b-45c3-838c-f7293570639c" width="66%" height="26%"></div>

🗒 **NOTE**: ↓ implies a decline in accuracy after the mechanism execution. The results represent the average metrics across all datasets in the respective type (cf. Benchmarks). Bold denotes the poorest judgement consistency. 



## Further Studies

### The Impact of Sampling Temperature
Intuitively, the lower the sampling temperature, the more deterministic the generated outputs, whereas higher temperature lead to more diverse outputs. Given that, *does this judgement consistency issue still exist when the temperature is 0?* 

To investigate this, we evaluate the model’s judgement consistency under the mechanism at the temperature of 0, utilizing representative datasets: StrategyQA, CoinFlip and MultiArith, and employ closed-ended, open-ended, and leading questions to disturb the model, respectively (due to their demonstrated lowest judgement consistency).
<div align=center> <img alt="results-temperature" src="https://github.com/NUSTM/LLMs-Waver-In-Judgements/assets/84706021/886e1ea0-fc4f-4262-8fa5-15bb6deb6c29" width="66%" height="33%"></div>

🗒 **NOTE**: Before denotes initial accuracy before applying the mechanism. Bold denotes the poorest judgement consistency.


### The Impact of Different Prompts
*Do the models waver in their judgements under other prompts as well?* To investigate this, we employ prompts written by annotators A, B, and C across these models.
<div align=center> <img width="780" alt="prompts-all" src="https://github.com/NUSTM/LLMs-Waver-In-Judgements/assets/84706021/c02bf33b-558a-4949-a791-793ffa7dd771" width="56%" height="12%"></div>

The impact of different prompts on Modification (Direct Form).
<div align=center> <img alt="results-prompts" src="https://github.com/NUSTM/LLMs-Waver-In-Judgements/assets/84706021/19b4133f-c7d1-450b-b172-95d9501d39b7" width="56%" height="15%"></div>


### Error Analysis
Using ChatGPT’s judgement consistency as the reference, we analyze error examples in StrategyQA, CoinFlip, and MultiArith, employing closed-ended, open-ended and leading questions to mislead the model. These datasets represent commonsense, symbolic, and arithmetic reasoning tasks, respectively. Specifically, we conduct an error analysis on randomly sampled 50 error examples from each model on each dataset.

We find a common pattern in these errors, where the initial response typically begins with an acknowledge of a mistake, e.g., “*I apologize for my mistake.*”. Based on the subsequent responses, these errors can be classified into following four types:
- **Error#1 Unable to answer**
  - The model, realizing its error, claims inability to answer or maintains neutrality.
- **Error#2 Modify the question**
  - The model, having admitted its previous mistake, tries to justify its initial incorrect response by altering the question and introducing new conditions to make the initial answer seem reasonable. 
- **Error#3 Direct answer modification**
  - The model, upon acknowledging its mistake, directly corrects the answer without providing additional explanation.
- **Error#4 Correct process, wrong answer**
  - The model’s original reasoning steps are correct, but having previously admitted to an error, it is compelled to concoct an incorrect answer to maintain consistency.
<div align=center> <img alt="results-error-analysis" src="https://github.com/NUSTM/LLMs-Waver-In-Judgements/assets/84706021/3bfc1165-0e3c-4ef7-8b94-fb517964d6a8" width="44%" height="10%"></div>


### Can the Mechanism Correct Models?
Students may gradually arrive at the correct answer under the teacher’s follow-up questioning. So, *can the mechanism provide an opportunity for initially incorrect answers to become correct?* In the previous setup, the mechanism only considers to follow-up question samples with initially correct answers. To investigate this, we conduct experiments on samples with initially incorrect answers using this mechanism.
<div align=center> <img alt="results-error-to-right" src="https://github.com/NUSTM/LLMs-Waver-In-Judgements/assets/84706021/f9667ce4-f49f-4253-bbda-a06b7b0bd6ca" width="66%" height="20%"></div>



## Mitigation Method Exploration



## Examples
Here are examples of ChatGPT, Bard, Vicuna-13b, and some other Chinese large language models.

<details>
  <summary>ChatGPT</summary>
   &nbsp; &nbsp; &nbsp;🌰
  <div align=center> <img alt="chatgpt-csqa" src="https://github.com/NUSTM/LLMs-Waver-In-Judgements/assets/84706021/aa07ab8f-b613-46c0-831a-23be6d5e9676" width="44%" height="10%"></div>
   &nbsp; &nbsp; &nbsp;🌰🌰
  <div align=center> <img alt="chatgpt-coin" src="https://github.com/NUSTM/LLMs-Waver-In-Judgements/assets/84706021/070d8944-58b3-483a-8cdb-9e3ab99d7cfd" width="44%" height="10%"></div>
</details>


<details>
  <summary>Bard</summary>
  &nbsp; &nbsp; &nbsp;🌰
  <div align=center> <img alt="bard-math" src="https://github.com/NUSTM/LLMs-Waver-In-Judgements/assets/84706021/f1a6fa13-9061-4e7d-a147-9d368f5b6126" width="44%" height="10%"></div>
  &nbsp; &nbsp; &nbsp;🌰🌰
  <div align=center> <img alt="bard-coin" src="https://github.com/NUSTM/LLMs-Waver-In-Judgements/assets/84706021/ea0522e4-fd4b-411f-a659-0bfa7b073d5d" width="44%" height="10%"></div>
</details>


<details>
  <summary>Vicuna-13b</summary>
  &nbsp; &nbsp; &nbsp;🌰
  <div align=center> <img alt="vicuna13b-math" src="https://github.com/NUSTM/LLMs-Waver-In-Judgements/assets/84706021/99e024f5-eba4-4d95-8d81-7234eccf95cd" width="55%" height="10%"></div>
  &nbsp; &nbsp; &nbsp;🌰🌰
  <div align=center> <img alt="vicuna13b-csqa" src="https://github.com/NUSTM/LLMs-Waver-In-Judgements/assets/84706021/568f3f0d-326b-4457-a83d-76e264913c95" width="55%" height="10%"></div>
</details>


<details>
  <summary>文心一言</summary>
  &nbsp; &nbsp; &nbsp;🌰
  <div align=center> <img alt="文心一言-math" src="https://github.com/NUSTM/LLMs-Waver-In-Judgements/assets/84706021/27779432-d75c-40ae-b4c2-3ac859358399" width="50%" height="10%"></div>
  &nbsp; &nbsp; &nbsp;🌰🌰
  <div align=center> <img alt="文心一言-coin" src="https://github.com/NUSTM/LLMs-Waver-In-Judgements/assets/84706021/bd9f14ea-b327-440b-92e0-25deb861c384" width="50%" height="10%"></div>
</details>


<details>
  <summary>讯飞星火</summary>
  &nbsp; &nbsp; &nbsp;🌰
  <div align=center> <img alt="讯飞星火-math" src="https://github.com/NUSTM/LLMs-Waver-In-Judgements/assets/84706021/a5746e70-0843-4cd1-8cd0-2be8939d0b1c" width="44%" height="10%"></div>
  &nbsp; &nbsp; &nbsp;🌰🌰
  <div align=center> <img alt="讯飞星火-csqa" src="https://github.com/NUSTM/LLMs-Waver-In-Judgements/assets/84706021/bccb3640-d773-41e3-b403-6b964218eea2" width="44%" height="10%"></div>
</details>


<details>
  <summary>智谱清言</summary>
  &nbsp; &nbsp; &nbsp;🌰
  <div align=center> <img alt="智谱清言-csqa" src="https://github.com/NUSTM/LLMs-Waver-In-Judgements/assets/84706021/86edf71b-2073-4836-a706-da90cc9ce795" width="44%" height="10%"></div>
  &nbsp; &nbsp; &nbsp;🌰🌰
  <div align=center> <img alt="智谱清言-coin" src="https://github.com/NUSTM/LLMs-Waver-In-Judgements/assets/84706021/5aef5855-78ff-447f-889a-e3dd42ff5b72" width="44%" height="10%"></div>
</details>

[⬆️ Back to overview](#overview)


## Citation
If you find this work helpful, please cite our paper as follows:

```
xxx
```


## Any Questions?
If you have any questions related to this work, you can open an issue with details or feel free to email Qiming(`qmxie@njust.edu.cn`), Zengzhi(`zzwang@njust.edu.cn`).
