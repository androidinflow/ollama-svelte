
<script>

  import ollama from 'ollama/browser';
  import { writable } from "svelte/store";
  import { onMount } from "svelte";


  const ollamaModelsLibrary = writable([
    {
        "name": "alfred",
        "installed": false,
        "description": "A robust conversational model designed to be used for both chat and instruct use cases."
    },
    {
        "name": "all-minilm",
        "installed": false,
        "description": "Embedding models on very large sentence level datasets."
    },
    {
        "name": "bakllava",
        "installed": false,
        "description": "BakLLaVA is a multimodal model consisting of the Mistral 7B base model augmented with the LLaVA architecture."
    },
    {
        "name": "codebooga",
        "installed": false,
        "description": "A high-performing code instruct model created by merging two existing code models."
    },
    {
        "name": "codellama",
        "installed": false,
        "description": "A large language model that can use text prompts to generate and discuss code."
    },
    {
        "name": "codeup",
        "installed": false,
        "description": "Great code generation model based on Llama2."
    },
    {
        "name": "deepseek-coder",
        "installed": false,
        "description": "DeepSeek Coder is a capable coding model trained on two trillion code and natural language tokens."
    },
    {
        "name": "deepseek-llm",
        "installed": false,
        "description": "An advanced language model crafted with 2 trillion bilingual tokens."
    },
    {
        "name": "dolphin-mistral",
        "installed": false,
        "description": "The uncensored Dolphin model based on Mistral that excels at coding tasks. Updated to version 2.6."
    },
    {
        "name": "dolphin-mixtral",
        "installed": false,
        "description": "An uncensored, fine-tuned model based on the Mixtral mixture of experts model that excels at coding tasks. Created by Eric Hartford."
    },
    {
        "name": "dolphin-phi",
        "installed": false,
        "description": "2.7B uncensored Dolphin model by Eric Hartford, based on the Phi language model by Microsoft Research."
    },
    {
        "name": "dolphincoder",
        "installed": false,
        "description": "An uncensored variant of the Dolphin model family that excels at coding, based on StarCoder2."
    },
    {
        "name": "duckdb-nsql",
        "installed": false,
        "description": "7B parameter text-to-SQL model made by MotherDuck and Numbers Station."
    },
    {
        "name": "everythinglm",
        "installed": false,
        "description": "Uncensored Llama2 based model with support for a 16K context window."
    },
    {
        "name": "falcon",
        "installed": false,
        "description": "A large language model built by the Technology Innovation Institute (TII) for use in summarization, text generation, and chat bots."
    },
    {
        "name": "gemma",
        "installed": false,
        "description": "Gemma is a family of lightweight, state-of-the-art open models built by Google DeepMind."
    },
    {
        "name": "goliath",
        "installed": false,
        "description": "A language model created by combining two fine-tuned Llama 2 70B models into one."
    },
    {
        "name": "llama-pro",
        "installed": false,
        "description": "An expansion of Llama 2 that specializes in integrating both general language understanding and domain-specific knowledge, particularly in programming and mathematics."
    },
    {
        "name": "llama2",
        "installed": false,
        "description": "Llama 2 is a collection of foundation language models ranging from 7B to 70B parameters."
    },
    {
        "name": "llama2-chinese",
        "installed": false,
        "description": "Llama 2 based model fine tuned to improve Chinese dialogue ability."
    },
    {
        "name": "llama2-uncensored",
        "installed": false,
        "description": "Uncensored Llama 2 model by George Sung and Jarrad Hope."
    },
    {
        "name": "llava",
        "installed": false,
        "description": "LLaVA is a novel end-to-end trained large multimodal model that combines a vision encoder and Vicuna for general-purpose visual and language understanding. Updated to version 1.6."
    },
    {
        "name": "magicoder",
        "installed": false,
        "description": "Magicoder is a family of 7B parameter models trained on 75K synthetic instruction data using OSS-Instruct, a novel approach to enlightening LLMs with open-source code snippets."
    },
    {
        "name": "meditron",
        "installed": false,
        "description": "Open-source medical large language model adapted from Llama 2 to the medical domain."
    },
    {
        "name": "medllama2",
        "installed": false,
        "description": "Fine-tuned Llama 2 model to answer medical questions based on an open source medical dataset."
    },
    {
        "name": "megadolphin",
        "installed": false,
        "description": "MegaDolphin-2.2-120b is a transformation of Dolphin-2.2-70b created by interleaving the model with itself."
    },
    {
        "name": "mistral",
        "installed": false,
        "description": "The 7B model released by Mistral AI, updated to version 0.2."
    },
    {
        "name": "mistral-openorca",
        "installed": false,
        "description": "Mistral OpenOrca is a 7 billion parameter model, fine-tuned on top of the Mistral 7B model using the OpenOrca dataset."
    },
    {
        "name": "mistrallite",
        "installed": false,
        "description": "MistralLite is a fine-tuned model based on Mistral with enhanced capabilities of processing long contexts."
    },
    {
        "name": "mixtral",
        "installed": false,
        "description": "A high-quality Mixture of Experts (MoE) model with open weights by Mistral AI."
    },
    {
        "name": "neural-chat",
        "installed": false,
        "description": "A fine-tuned model based on Mistral with good coverage of domain and language."
    },
    {
        "name": "nexusraven",
        "installed": false,
        "description": "Nexus Raven is a 13B instruction tuned model for function calling tasks."
    },
    {
        "name": "nomic-embed-text",
        "installed": false,
        "description": "A high-performing open embedding model with a large token context window."
    },
    {
        "name": "notus",
        "installed": false,
        "description": "A 7B chat model fine-tuned with high-quality data and based on Zephyr."
    },
    {
        "name": "notux",
        "installed": false,
        "description": "A top-performing mixture of experts model, fine-tuned with high-quality data."
    },
    {
        "name": "nous-hermes",
        "installed": false,
        "description": "General use models based on Llama and Llama 2 from Nous Research."
    },
    {
        "name": "nous-hermes2",
        "installed": false,
        "description": "The powerful family of models by Nous Research that excels at scientific discussion and coding tasks."
    },
    {
        "name": "nous-hermes2-mixtral",
        "installed": false,
        "description": "The Nous Hermes 2 model from Nous Research, now trained over Mixtral."
    },
    {
        "name": "open-orca-platypus2",
        "installed": false,
        "description": "Merge of the Open Orca OpenChat model and the Garage-bAInd Platypus 2 model. Designed for chat and code generation."
    },
    {
        "name": "openchat",
        "installed": false,
        "description": "A family of open-source models trained on a wide variety of data, surpassing ChatGPT on various benchmarks. Updated to version 3.5-0106."
    },
    {
        "name": "openhermes",
        "installed": false,
        "description": "OpenHermes 2.5 is a 7B model fine-tuned by Teknium on Mistral with fully open datasets."
    },
    {
        "name": "orca-mini",
        "installed": false,
        "description": "A general-purpose model ranging from 3 billion parameters to 70 billion, suitable for entry-level hardware."
    },
    {
        "name": "orca2",
        "installed": false,
        "description": "Orca 2 is built by Microsoft research, and are a fine-tuned version of Meta's Llama 2 models. The model is designed to excel particularly in reasoning."
    },
    {
        "name": "phi",
        "installed": false,
        "description": "Phi-2: a 2.7B language model by Microsoft Research that demonstrates outstanding reasoning and language understanding capabilities."
    },
    {
        "name": "phind-codellama",
        "installed": false,
        "description": "Code generation model based on Code Llama."
    },
    {
        "name": "qwen",
        "installed": false,
        "description": "Qwen 1.5 is a series of large language models by Alibaba Cloud spanning from 0.5B to 72B parameters"
    },
    {
        "name": "samantha-mistral",
        "installed": false,
        "description": "A companion assistant trained in philosophy, psychology, and personal relationships. Based on Mistral."
    },
    {
        "name": "solar",
        "installed": false,
        "description": "A compact, yet powerful 10.7B large language model designed for single-turn conversation."
    },
    {
        "name": "sqlcoder",
        "installed": false,
        "description": "SQLCoder is a code completion model fined-tuned on StarCoder for SQL generation tasks"
    },
    {
        "name": "stable-beluga",
        "installed": false,
        "description": "Llama 2 based model fine tuned on an Orca-style dataset. Originally called Free Willy."
    },
    {
        "name": "stable-code",
        "installed": false,
        "description": "Stable Code 3B is a model offering accurate and responsive code completion at a level on par with models such as CodeLLaMA 7B that are 2.5x larger."
    },
    {
        "name": "stablelm-zephyr",
        "installed": false,
        "description": "A lightweight chat model allowing accurate, and responsive output without requiring high-end hardware."
    },
    {
        "name": "stablelm2",
        "installed": false,
        "description": "Stable LM 2 1.6B is a state-of-the-art 1.6 billion parameter small language model trained on multilingual data in English, Spanish, German, Italian, French, Portuguese, and Dutch."
    },
    {
        "name": "starcoder",
        "installed": false,
        "description": "StarCoder is a code generation model trained on 80+ programming languages."
    },
    {
        "name": "starcoder2",
        "installed": false,
        "description": "StarCoder2 is the next generation of transparently trained open code LLMs that comes in three sizes: 3B, 7B and 15B parameters."
    },
    {
        "name": "starling-lm",
        "installed": false,
        "description": "Starling is a large language model trained by reinforcement learning from AI feedback focused on improving chatbot helpfulness."
    },
    {
        "name": "tinydolphin",
        "installed": false,
        "description": "An experimental 1.1B parameter model trained on the new Dolphin 2.8 dataset by Eric Hartford and based on TinyLlama."
    },
    {
        "name": "tinyllama",
        "installed": false,
        "description": "The TinyLlama project is an open endeavor to train a compact 1.1B Llama model on 3 trillion tokens."
    },
    {
        "name": "vicuna",
        "installed": false,
        "description": "General use chat model based on Llama and Llama 2 with 2K to 16K context sizes."
    },
    {
        "name": "wizard-math",
        "installed": false,
        "description": "Model focused on math and logic problems"
    },
    {
        "name": "wizard-vicuna",
        "installed": false,
        "description": "Wizard Vicuna is a 13B parameter model based on Llama 2 trained by MelodysDreamj."
    },
    {
        "name": "wizard-vicuna-uncensored",
        "installed": false,
        "description": "Wizard Vicuna Uncensored is a 7B, 13B, and 30B parameter model based on Llama 2 uncensored by Eric Hartford."
    },
    {
        "name": "wizardcoder",
        "installed": false,
        "description": "State-of-the-art code generation model"
    },
    {
        "name": "wizardlm",
        "installed": false,
        "description": "General use 70 billion parameter model based on Llama 2."
    },
    {
        "name": "wizardlm-uncensored",
        "installed": false,
        "description": "Uncensored version of Wizard LM model"
    },
    {
        "name": "xwinlm",
        "installed": false,
        "description": "Conversational model based on Llama 2 that performs competitively on various benchmarks."
    },
    {
        "name": "yarn-llama2",
        "installed": false,
        "description": "An extension of Llama 2 that supports a context of up to 128k tokens."
    },
    {
        "name": "yarn-mistral",
        "installed": false,
        "description": "An extension of Mistral to support context windows of 64K or 128K."
    },
    {
        "name": "yi",
        "installed": false,
        "description": "A high-performing, bilingual language model."
    },
    {
        "name": "zephyr",
        "installed": false,
        "description": "Zephyr beta is a fine-tuned 7B version of mistral that was trained on on a mix of publicly available, synthetic datasets."
    }
  ]);

  const svgSpinner = `
    <svg xmlns="http://www.w3.org/2000/svg" viewBox="0 0 24 24" width=".66em" height=".66em"><g fill="none" stroke="currentColor" stroke-linecap="round" stroke-width="2"><path stroke-dasharray="60" stroke-dashoffset="60" stroke-opacity=".3" d="M12 3C16.9706 3 21 7.02944 21 12C21 16.9706 16.9706 21 12 21C7.02944 21 3 16.9706 3 12C3 7.02944 7.02944 3 12 3Z"><animate fill="freeze" attributeName="stroke-dashoffset" dur="1.3s" values="60;0"></animate></path><path stroke-dasharray="15" stroke-dashoffset="15" d="M12 3C16.9706 3 21 7.02944 21 12"><animate fill="freeze" attributeName="stroke-dashoffset" dur="0.3s" values="15;0"></animate><animateTransform attributeName="transform" dur="1.5s" repeatCount="indefinite" type="rotate" values="0 12 12;360 12 12"></animateTransform></path></g></svg>
    `

  const svgDownload = `
    <svg xmlns="http://www.w3.org/2000/svg" viewBox="0 0 717 717" width="1.24em" height="1.24em"><path fill="currentColor" d="M586 271H457V60c0-18-15-33-33-33H295c-19 0-34 15-34 33v211H132c-17 0-23 11-10 24l214 213c6 6 14 9 23 9s18-3 24-9l213-213c12-13 8-24-10-24zM0 422v228c0 10 5 16 16 16h684c11 0 17-6 17-16V422c0-10-8-17-17-17h-65c-9 0-17 8-17 17v145H98V422c0-10-7-17-16-17H16c-9 0-16 8-16 17z"></path></svg>
    `;

  const svgBin = `
    <svg xmlns="http://www.w3.org/2000/svg" viewBox="0 0 16 16" width="1.24em" height="1.24em"><path fill="currentColor" d="M2 5v10c0 .55.45 1 1 1h9c.55 0 1-.45 1-1V5H2zm3 9H4V7h1v7zm2 0H6V7h1v7zm2 0H8V7h1v7zm2 0h-1V7h1v7zm2.25-12H10V.75A.753.753 0 0 0 9.25 0h-3.5A.753.753 0 0 0 5 .75V2H1.75a.752.752 0 0 0-.75.75V4h13V2.75a.752.752 0 0 0-.75-.75zM9 2H6v-.987h3V2z"></path></svg>
    `;

  let modelsMap = new Map();


  onMount(async() => {
    try {
      let modelsList = await ollama.list()
      modelsList = modelsList.models;
      if(modelsList && modelsList.length === 0){
        alert("You don't currently have any local models. Please rectify and try again...");
        return;
      } 
      for (let model of modelsList){ 
        modelsMap.set(model.name.split(":")[0], model);
      } 
      $ollamaModelsLibrary.forEach(model => {
        model.installed = modelsMap.get(model.name) != undefined;
      });
      $ollamaModelsLibrary = $ollamaModelsLibrary;
    } catch (error) {
      alert("There was an error processing your request...")
      console.error('error in onMount', error);       
    }
  })


</script>


  <h1>
    Models
  </h1>
  <div
    style="
      display: flex;
      flex-direction: column;
      height: 120vh;
      overflow-y: scroll;
    ">
    {#each $ollamaModelsLibrary as model }
      <div class="list-row">
        <span style="grid-column: 1;">
          <span style="font-size: .88em; font-weight: 600; display: inline-block; padding-bottom: .22em;">
            {model.name}
          </span>
          {#if model.installed | model.pulling}
            <span style="
              display: inline-block;
              padding: .12em .66em;
              font-size: .88em;
              border-radius: calc(var(--border-radius)* 2);
              background-color: {model.installed ? 'green' : 'orange'};
              color: var(--color-primary-complimentary);
              margin-left: 1.24em;
              transition: all ease-in .14s;
              ">
              {model.installed ? 'Installed' : 'Pulling'} 
              {#if model.pulling }
                <span style="display: inline-block; margin-top: 0;">
                  {@html svgSpinner}
                </span>
              {/if}
            </span>
          {/if}
          <br>
          <span>
            {model.description}
          </span>
        </span>

        <button 
          on:click={async() => {
            if(model.installed){
              if(!confirm(`Are you sure you want to delete the model ${model.name}?`)){return};
              // Delete
              await ollama.delete({model: model.name});
              model.installed = false;
              alert("Model "+model.name+" deleted...");
            } else {
              // Download
              model.pulling = true; 
              await ollama.pull({model: model.name});
              model.pulling = false; 
              model.installed = true;
              alert("Model "+model.name+" pulled...");
            }
          }}
          style="
            grid-column: 2; 
            font-size: normal; 
            border: none; 
            background: transparent;
            cursor: pointer;
            position: absolute;
            right: .12em;
            top: .5em;
            color: var(--color-svg-icon);
            ">
          {@html model.installed ? svgBin : svgDownload}
        </button>
      </div>
    {/each}
  
  </div>
<style>

  .list-row{
    background-color: rgba(255,255,255,.24);
    display: grid;
    grid-template-columns: 1fr min-content;
    text-align: left;
    border: none;
    color: var(--standard-text-color);
    padding: .42em 1em;
    margin-block: .12em;
    border: 1px solid rgba(200,200,200,.44);
    position: relative;
    border-radius: var(--border-radius);
  }
  
  .list-row:first-of-type{
    margin-top: 1em;
  }

  .list-row:nth-child(even){
    background-color: rgba(242,242,242,.24);
  }

  .list-row:last-of-type{
    margin-bottom: 12em;
  }

</style>