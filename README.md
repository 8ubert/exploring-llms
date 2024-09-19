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

![Notebooks](https://tse3.mm.bing.net/th?id=OIP.r8ON7toWMgnFRjdRGLCMowHaC-&pid=Api&P=0&h=180) <!-- Substitua pelo caminho da sua imagem -->

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
from transformers import pipeline

# Carregar o classificador de jailbreak
jailbreak_classifier = pipeline("text-classification", model="jackhhao/jailbreak-classifier")
# Carregar o modelo generativo
text_generator = pipeline("text-generation", model="TinyLlama/TinyLlama-1.1B-Chat-v1.0")

def process_prompt(user_prompt):
    # Checar se o prompt é potencialmente malicioso
    classification = jailbreak_classifier(user_prompt)
    if classification[0]['label'] == 'jailbreak':
        return "Este prompt é potencialmente malicioso e não será processado."
    else:
        # Gerar texto seguro
        response = text_generator(user_prompt, max_new_tokens=100)
        return response[0]['generated_text']
