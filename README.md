# Ask Again, Then Fail: Large Language Models' Vacillations in Judgment

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
    - [The Impact of Tone Intensity 🗣️](#the-impact-of-tone-intensity)
    - [Error Analysis 🔍](#error-analysis)
    - [Can the Mechanism Correct Models❓](#can-the-mechanism-correct-models)
  - [Mitigation Method Exploration](#mitigation-method-exploration)
  - [Examples 🌰](#examples)
  - [Any Question?](#any-questions)
  - [Citation](#citation)



## Overview
❗️ With the emergence of generative conversational large language models (LLMs) like ChatGPT, serving as virtual assistants in various fields, the stability and reliability of their responses have become crucial. **However, during usage, it has been observed that these models tend to waver in their judgments when confronted with follow-up questions from users expressing skepticism or disagreement.** [🌰 Like these examples 🌰](#examples)

🪛 In this work, we draw inspiration from questioning strategies in education and propose a **FOLLOW-UP QUESTIONING MECHANISM** along with two evaluation metrics to assess the judgment consistency of LLMs before and after exposure to disturbances. We evaluate the judgment consistency of ChatGPT, PaLM2-Bison, and Vicuna-13B under this mechanism across eight reasoning benchmarks. Empirical results show that even when the initial answers are correct, judgment consistency sharply decreases when LLMs face disturbances such as questioning, negation, or misleading. 

📊 Additionally, we study these models’ judgment consistency under various settings (sampling temperature and prompts) to validate this issue further, observing the impact of prompt tone and conducting an in-depth error analysis for deeper behavioral insights. Furthermore, we also explore several prompting methods to mitigate this issue and demonstrate their effectiveness.

🗒 **NOTE**: We define judgment consistency as the consistency of the model’s final answers when handling objective questions with definitive answers.



## FOLLOW-UP QUESTIONING MECHANISM
To evaluate this consistency of large language models, we design a **FOLLOW-UP QUESTIONING MECHANISM**. This mechanism consists of three types of follow-up questions, organized in two different forms. After the model initially answers correctly, we continue dialogues to question, negate, or mislead it, then observe any judgment changes.
<div align=center> <img alt="method" src="https://github.com/NUSTM/LLMs-Waver-In-judgments/assets/84706021/15c39d6a-d453-4960-b606-ed380463c7b5" width="72%" height="40%"> </div>

The prompts we used in the experiment. C, O, and L represent closed-ended questions, open-ended questions, leading questions, respectively. {M_A} denotes the misleading answers.
<div align=center> <img alt="prompts-a" src="https://github.com/NUSTM/LLMs-Waver-In-judgments/assets/84706021/d2553939-650b-403a-a7fa-d6d5e91026b5" width="36%" height="9%"> </div>

We employ two metrics to assess the judgment consistency of LLMs after the execution of the mechanism.
- **Modification (M.)** measures the difference in model performance before and after the mechanism execution.
- **Modification Rate (M. Rate)** represents the occurrence rate of Modifications, defined as the ratio of Modification to the initial model performance.
<div align=center> <img alt="metrics" src="https://github.com/NUSTM/LLMs-Waver-In-judgments/assets/84706021/4730911f-8376-44d3-a035-a763ef001906" width="50%" height="23%"> </div>



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
<div align=center> <img alt="results-chatgpt-d" src="https://github.com/NUSTM/LLMs-Waver-In-judgments/assets/84706021/59d31541-189b-4143-97ac-40814757646a" width="66%" height="33%"> </div>

The results of ChatGPT in Progressive Form.
<div align=center> <img alt="results-chatgpt-p" src="https://github.com/NUSTM/LLMs-Waver-In-judgments/assets/84706021/1f6a2805-5436-4790-97c7-21830081a394" width="66%" height="26%"> </div>

The results of the mechanism in Direct Form (Left) and Progressive Form (Right) on PaLM2-Bison and Vicuna-13B.
<div align=center> <img alt="results-palm-vicuna-d-p" src="https://github.com/NUSTM/LLMs-Waver-In-judgments/assets/84706021/bba9e036-b3d1-4bf3-8d31-6b5449f63174" width="66%" height="26%"> </div>

🗒 **NOTE**: ↓ implies a decline in accuracy after the mechanism execution. The results represent the average metrics across all datasets in the respective type (cf. Benchmarks). Bold denotes the poorest judgment consistency. 



## Further Studies

### The Impact of Sampling Temperature
Intuitively, the lower the sampling temperature, the more deterministic the generated outputs, whereas higher temperature lead to more diverse outputs. Given that, *does this judgment consistency issue still exist when the temperature is 0?* 

To investigate this, we evaluate the model’s judgment consistency under the mechanism at the temperature of 0, utilizing representative datasets: StrategyQA, CoinFlip and MultiArith, and employ closed-ended, open-ended, and leading questions to disturb the model, respectively (due to their demonstrated lowest judgment consistency).
<div align=center> <img alt="results-temperature" src="https://github.com/NUSTM/LLMs-Waver-In-judgments/assets/84706021/5b31bbd6-4a46-4021-9e2d-d222e2fd849f" width="56%" height="33%"></div>

🗒 **NOTE**: Before denotes initial accuracy before applying the mechanism. Bold denotes the poorest judgment consistency.


### The Impact of Different Prompts
*Do the models waver in their judgments under other prompts as well?* To investigate this, we employ prompts written by annotators A, B, and C across these models.
<div align=center> <img width="780" alt="prompts-all" src="https://github.com/NUSTM/LLMs-Waver-In-judgments/assets/84706021/dd896872-f72b-4b9e-bb46-cd58ac649ddd" width="56%" height="12%"> </div>

The impact of different prompts on Modification (Direct Form).
<div align=center> <img alt="results-prompts" src="https://github.com/NUSTM/LLMs-Waver-In-judgments/assets/84706021/07980c82-1ae7-453e-8548-c7bc07d2026f" width="56%" height="15%"> </div>


### The Impact of Tone Intensity
Considering the practical educational scenario, when students face questioning, denial, or misinformation, their judgments often experience a significant impact from the teacher’s tone intensity of speech. Therefore, we explore the influence of using different prompts on the model’s judgment consistency from the perspective of tone intensity. Due to the limited capabilities of the model, Vicuna-13B cannot score different prompts within the 0 to 10 range based on the strength of tone as per our request. In addition, compared to the other two models, Vicuna-13B shows relatively small fluctuations in judgment consistency when different prompts are used. Therefore, we only explore the impact of the tone intensity of prompts on ChatGPT and PaLM2-Bison.

Considering the varying interpretations of tone intensity by different models, we first have ChatGPT and PaLM2-Bison separately rate the tone intensity of prompts A, B, and C on a scale of 0 to 10. We categorize the questions into different types, calculate the average Modification for the three prompts within each question type across all datasets. The models’ tone intensity scores for the three prompts (cf. The Impact of Different Prompts) were taken as reference points.
<div align=center> <img alt="results-tone-intensity" src="https://github.com/NUSTM/LLMs-Waver-In-judgments/assets/84706021/6bc32e8b-8350-41a7-98ff-dc878a3fb6d6" width="30%" height="30%"> </div>


### Error Analysis
Using ChatGPT’s judgment consistency as the reference, we analyze error examples in StrategyQA, CoinFlip, and MultiArith, employing closed-ended, open-ended and leading questions to mislead the model. These datasets represent commonsense, symbolic, and arithmetic reasoning tasks, respectively. Specifically, we conduct an error analysis on randomly sampled 50 error examples from each model on each dataset.

We find a common pattern in these errors, where the initial response typically begins with an acknowledge of a mistake, e.g., “*I apologize for my mistake.*”. Based on the subsequent responses, these errors can be classified into following four types:
- **Error#1 Unable to answer**
  - The model, realizing its error, claims inability to answer or maintains neutrality.
- **Error#2 Modify the question**
  - The model, having admitted its previous mistake, tries to justify its initial incorrect response by altering the question and introducing new conditions to make the initial answer seem reasonable. 
- **Error#3 Direct answer modification**
  - The model, upon acknowledging its mistake, directly corrects the answer without providing additional explanation.
- **Error#4 Correct process, wrong answer**
  - The model’s original reasoning steps are correct, but having previously admitted to an error, it is compelled to concoct an incorrect answer to maintain consistency.
<div align=center> <img alt="results-error-analysis" src="https://github.com/NUSTM/LLMs-Waver-In-judgments/assets/84706021/5b84abaf-9bfd-448c-888a-203cf4547508" width="36%" height="10%"> </div>



### Can the Mechanism Correct Models?
Students may gradually arrive at the correct answer under the teacher’s follow-up questioning. So, *can the mechanism provide an opportunity for initially incorrect answers to become correct?* In the previous setup, the mechanism only considers to follow-up question samples with initially correct answers. To investigate this, we conduct experiments on samples with initially incorrect answers using this mechanism.
<div align=center> <img alt="results-error-to-right" src="https://github.com/NUSTM/LLMs-Waver-In-judgments/assets/84706021/563287b2-3d97-4840-8422-46bb13987980" width="56%" height="20%"> </div>



## Mitigation Method Exploration
Essentially, we believe that this issue originates from the misalignment between the model’s response generation process when facing disturbances and the thinking process of humans under similar disturbances. In this work, we explore several prompting strategies to mitigate this issue, including zero-shot and few-shot prompting.
- **Zero-shot prompting**
  - Zero-shot-CoT: *Let’s think step by step.*
  - EmotionPrompt: *This is very important to my career.*
- **Few-shot prompting**
  - we randomly select several samples from the training set to construct demonstration examples of multi-turn dialogues under this mechanism, providing manually written response reflective of human thought processes in follow-up question-answering. In responding to
follow-up questions within these samples, the model response doesn’t directly admit to mistakes as ChatGPT does. Instead, it begins by clarifying its thoughts and reconsidering step by step, initiating responses with, "*Please wait for a moment. In order to answer your question, I need to take a moment to reconsider. I will now clear my mind of distractions and approach this step by step.*"
<div align=center> <img alt="results-mitigation-method" src="https://github.com/NUSTM/LLMs-Waver-In-judgments/assets/84706021/c1435e42-09a6-4626-b53f-94d435e6b9bf" width="56%" height="20%"> </div>



## Examples
Here are examples of ChatGPT, Bard, Vicuna-13b, and some other Chinese large language models.

<details>
  <summary>ChatGPT</summary>
   &nbsp; &nbsp; &nbsp;🌰
  <div align=center> <img alt="chatgpt-csqa" src="https://github.com/NUSTM/LLMs-Waver-In-judgments/assets/84706021/e8d19e49-d7d8-4c7d-83c9-1fc46af9fbe3" width="50%" height="10%"></div>

   &nbsp; &nbsp; &nbsp;🌰🌰
  <div align=center> <img alt="chatgpt-coin" src="https://github.com/NUSTM/LLMs-Waver-In-judgments/assets/84706021/d0a18bca-69ca-4732-8a8a-daf89eff6cb2" width="50%" height="10%"></div>
  
</details>


<details>
  <summary>Bard</summary>
  &nbsp; &nbsp; &nbsp;🌰
  <div align=center> <img alt="bard-math" src="https://github.com/NUSTM/LLMs-Waver-In-judgments/assets/84706021/490a3c27-c13f-47e2-9d21-4dde7e5d60c6" width="50%" height="10%"></div>

  &nbsp; &nbsp; &nbsp;🌰🌰
  <div align=center> <img alt="bard-coin" src="https://github.com/NUSTM/LLMs-Waver-In-judgments/assets/84706021/a570f822-4d39-442c-90fb-d021fcc705c4" width="50%" height="10%"></div>

</details>


<details>
  <summary>Vicuna-13b</summary>
  &nbsp; &nbsp; &nbsp;🌰
  <div align=center> <img alt="vicuna13b-math" src="https://github.com/NUSTM/LLMs-Waver-In-judgments/assets/84706021/f383d51a-4643-4872-8738-4bc92e6e5d46" width="55%" height="10%"></div>

  &nbsp; &nbsp; &nbsp;🌰🌰
  <div align=center> <img alt="vicuna13b-csqa" src="https://github.com/NUSTM/LLMs-Waver-In-judgments/assets/84706021/0ac1e9ba-b585-46a9-972d-0fb732e2e109" width="55%" height="10%"></div>

</details>


<details>
  <summary>文心一言</summary>
  &nbsp; &nbsp; &nbsp;🌰
  <div align=center> <img alt="文心一言-math" src="https://github.com/NUSTM/LLMs-Waver-In-judgments/assets/84706021/ab3d01fa-f48d-4710-b895-0e166d94c6c8" width="50%" height="10%"></div>

  &nbsp; &nbsp; &nbsp;🌰🌰
  <div align=center> <img alt="文心一言-coin" src="https://github.com/NUSTM/LLMs-Waver-In-judgments/assets/84706021/4981abcf-64c1-4a6e-9b65-008c2fb6249a" width="50%" height="10%"></div>

</details>


<details>
  <summary>讯飞星火</summary>
  &nbsp; &nbsp; &nbsp;🌰
  <div align=center> <img alt="讯飞星火-math" src="https://github.com/NUSTM/LLMs-Waver-In-judgments/assets/84706021/ca8418ec-2dba-44a4-88a6-e7f49c2b92ce" width="50%" height="10%"></div>

  &nbsp; &nbsp; &nbsp;🌰🌰
  <div align=center> <img alt="讯飞星火-csqa" src="https://github.com/NUSTM/LLMs-Waver-In-judgments/assets/84706021/1148a287-3db3-4494-bdaf-489edcdd6380" width="50%" height="10%"></div>

</details>


<details>
  <summary>智谱清言</summary>
  &nbsp; &nbsp; &nbsp;🌰
  <div align=center> <img alt="智谱清言-csqa" src="https://github.com/NUSTM/LLMs-Waver-In-judgments/assets/84706021/1724d4aa-a3ad-4f8b-a4b0-c637f865bd6f" width="50%" height="10%"></div>

  &nbsp; &nbsp; &nbsp;🌰🌰
  <div align=center> <img alt="智谱清言-coin" src="https://github.com/NUSTM/LLMs-Waver-In-judgments/assets/84706021/38ab536a-adf3-4e3d-8af4-7096c8b99c99" width="50%" height="10%"></div>

</details>

[⬆️ Back to overview](#overview)


## Citation
If you find this work helpful, please cite our paper as follows:

```
@article{xie2023ask,
  title={Ask Again, Then Fail: Large Language Models' Vacillations in judgment},
  author={Xie, Qiming and Wang, Zengzhi and Feng, Yi and Xia, Rui},
  eprint={2310.02174},
  year={2023}
}
```


## Any Questions?
If you have any questions related to this work, you can open an issue with details or feel free to email Qiming(`qmxie@njust.edu.cn`), Zengzhi(`zzwang@njust.edu.cn`).
