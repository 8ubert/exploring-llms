# Exploring LLMs

Explorando as capacidades dos Modelos de Linguagem (LLMs) com a biblioteca Transformers da Hugging Face.

## 📚 Checkpoint 04

**Autores:**  
- João Victor Santos Alves (RM99634)  
- Luiza Barros Hubert (RM97905)  
- Suellen Guedes Rufino (RM97687)  

## 🗂️ Notebooks

1. [EntendendoLLMs.ipynb](./EntendendoLLMs.ipynb)
2. [Tokens,DevicesEMemóriaDeConversa.ipynb](./Tokens,DevicesEMemóriaDeConversa.ipynb)
3. [FiltrandoJailbreak.ipynb](./FiltrandoJailbreak.ipynb)

---

## 📖 Descrição do Projeto

Este projeto visa desenvolver um sistema de detecção de prompts de jailbreak utilizando modelos de linguagem (LLMs) ajustados. O sistema combina dois modelos: um classificador de prompts e um gerador de texto.

### 🔍 O que é Fine-Tuning?

**Fine-tuning** é o processo de ajustar um modelo de aprendizado de máquina pré-treinado em uma nova tarefa, utilizando um conjunto de dados específico. Isso permite que o modelo aprenda a realizar tarefas especializadas, melhorando seu desempenho em situações específicas.

### 🚨 O que é Jailbreak?

**Jailbreak** refere-se a técnicas utilizadas para contornar as limitações de segurança de um modelo de linguagem, possibilitando a geração de conteúdos indesejados ou maliciosos. Prompts de jailbreak são comandos projetados para manipular o comportamento do modelo, levando-o a gerar respostas inadequadas.

## ⚙️ Implementação

Neste notebook, utilizamos:

- **Classificador de Jailbreak:** disponível em [Hugging Face](https://huggingface.co/jackhhao/jailbreak-classifier)
- **Modelo de Geração de Texto:** [TinyLlama](https://huggingface.co/TinyLlama/TinyLlama-1.1B-Chat-v1.0)

### 🛠️ Funcionalidade

A função principal do sistema é a seguinte:

```python

def processa_prompt(modelo, prompt):
  print("="*80)

  # Codificando o prompt em tokens
  tokens = modelo.tokenizer.encode(prompt, return_tensors='pt')
  print(f"- prompt original: {prompt}")
  print(f"- tokens: {tokens}")
  print(f"- # de tokens: {len(tokens)}")

  # Decodificando os tokens
  decoded = modelo.tokenizer.convert_ids_to_tokens(tokens[0])
  print(f"- tokens decodificados: {decoded}")

  first_tokens = modelo.tokenizer.convert_ids_to_tokens(range(5))
  print(f"- primeiros tokens: {first_tokens}")

  # Processando o prompt com o modelo
  # O nível de "criatividade" do modelo é alterado para 0.7 através do parâmetro "temperature"
  # É necessário setar "do_sample"=True para controlar a temperatura
  output = modelo(prompt, do_sample=True, temperature=0.7)
  print(f"- output do modelo: {output}")
  print(f"- texto gerado efetivamente: {output[0]['generated_text']}")

  print("="*80)
  return

processa_prompt(modelo, "Once upon a time")


