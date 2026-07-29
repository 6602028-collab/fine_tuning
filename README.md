## Unsloth LLM Fine-Tuning 

Welcome to the repository! This project provides a highly optimized pipeline for downloading, tokenizing, and preparing Large Language Models (LLMs) for fine-tuning. By leveraging the power of the Unsloth library, this setup ensures that model training is not only fast but also highly efficient. 

Whether you're exploring data analytics, building business intelligence tools, or just diving into AI, this repository does the heavy lifting for you.

**What the project does** 
ServiceNow-AI/R1-Distill-SFT — reasoning traces distilled from DeepSeek-R1 (problem → reasoning chain → solution).

1. **Load model in 4-bit** via Unsloth (FastLanguageModel.from_pretrained) — quantized to fit on a free Colab GPU (T4, ~16GB).
2. **Attach LoRA adapters** (get_peft_model) — rank 16, alpha 16, applied to attention projections (q/k/v/o) and MLP projections (gate/up/down). Only these small adapter matrices get trained; the base model stays frozen.
3. **Dataset:** ServiceNow-AI/R1-Distill-SFT — reasoning traces distilled from DeepSeek-R1 (problem → reasoning chain → solution).
4. **Prompt formatting:** wrapped each example in a custom template (<problem>...</problem> + reasoning + solution) so the model learns to imitate reflective, exploratory reasoning.
5. **Training:** SFTTrainer from TRL — supervised fine-tuning, batch size 2, grad accumulation 4 (effective batch 8), 60 steps, lr 2e-4, AdamW 8-bit, linear schedule, mixed precision (bf16/fp16), gradient checkpointing.
6. **Inference:** applied Llama-3.1 chat template, tested with a reasoning prompt ("how many r's in strawberry").

This notebook environment is specifically designed to run on Google Colab. It handles the intricate processes of retrieving large model weights and rapidly tokenizing massive datasets so you can focus on building and experimenting rather than waiting for loading bars.

##  Key Features

* **Base model-** Llama-3.2-3B-Instruct, loaded via Unsloth's FastLanguageModel for optimized, memory-efficient loading.
* **4-bit quantization (QLoRA-style)-** Built entirely on a Python 3 kernel for maximum compatibility and ease of use.
* **Parameter-efficient fine-tuning with LoRA —** instead of updating all 3B parameters, only small low-rank adapter matrices are trained (rank r=16, lora_alpha=16), applied across both attention layers (q/k/v/o projections) and MLP layers (gate/up/down projections)..
* **Reasoning-focused dataset-**trained on ServiceNow-AI/R1-Distill-SFT, which contains problem → reasoning-trace → solution triples distilled from DeepSeek-R1, so the goal is teaching how the model thinks, not just final answers.
* **Reasoning-focused dataset-** trained on ServiceNow-AI/R1-Distill-SFT, which contains problem → reasoning-trace → solution triples distilled from DeepSeek-R1, so the goal is teaching how the model thinks, not just final answers.
* * **Supervised fine-tuning (SFT)-** via SFTTrainer from TRL — trains the model to imitate the reasoning+solution text directly, with EOS_TOKEN appended so the model learns when to stop generating.

## 🛠️ Prerequisites

Before you start, ensure you have the following:

* A Google account to access Google Colab.
* Access to a T4 GPU (available in Colab's free tier).
* Basic familiarity with Python 3.

## 🚀 Getting Started

1. **Open the Notebook:** Load the `.ipynb` file directly into Google Colab.
2. **Enable GPU:** Navigate to `Runtime` > `Change runtime type` and ensure the hardware accelerator is set to **T4 GPU**.
3. **Run the Cells:** Execute the cells sequentially. You will see progress bars indicating:
   * The successful download of model configuration files.
   * The initialization of Unsloth tokenization.
   * The processing of your dataset (up to 171k+ examples) at high speeds.

## 📊 Performance Metrics

This pipeline is built for speed. During typical runs, the data mapping and tokenization processes handle hundreds of examples per second, dramatically reducing the time it takes to prepare your data for training. 

## 🤝 Contributing

Contributions, issues, and feature requests are always welcome! Feel free to check the issues page if you want to contribute. 

## 📜 License

[Abhishek Kumar]
