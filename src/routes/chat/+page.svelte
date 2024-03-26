<script>

  import ollama from 'ollama/browser'; // IMPORTANT: use 'ollama/browser' not 'ollama', which won't build in Svelte.
  import { onMount } from 'svelte';
  import { writable } from 'svelte/store';
  import { fade } from 'svelte/transition';

  const chatLog = writable({model: undefined, lastID: 0, history: []});

  let modelsList = undefined;
  let show = false;
  let prompt = 'What is 2 + 2?';
  let aiResponse = undefined;
  let ollamaFail = true;
  let iconColor = 'var(--color-primary)'
  let iconOpacity = '.72'
  let ollamaRunning = false;

  const svgAI = `
    <svg xmlns="http://www.w3.org/2000/svg" viewBox="0 0 24 24" width="1.24em" height="1.24em" style="opacity: ${iconOpacity}"><g fill="none" stroke="${iconColor}" stroke-linecap="round" stroke-linejoin="round" stroke-width="2"><path d="M21 16V8a2 2 0 0 0-1-1.73l-7-4a2 2 0 0 0-2 0l-7 4A2 2 0 0 0 3 8v8a2 2 0 0 0 1 1.73l7 4a2 2 0 0 0 2 0l7-4A2 2 0 0 0 21 16z"></path><path d="m7.5 4.21l4.5 2.6l4.5-2.6m-9 15.58V14.6L3 12m18 0l-4.5 2.6v5.19M3.27 6.96L12 12.01l8.73-5.05M12 22.08V12"></path></g></svg>
    `;

  const svgUser = `
    <svg xmlns="http://www.w3.org/2000/svg" viewBox="0 0 24 24" data-setname="Mono Icons" data-author="Mono" data-url="https://github.com/mono-company/mono-icons" width="1.24em" height="1.24em" style="opacity: ${iconOpacity}">"<path fill="${iconColor}" d="M12 4a4 4 0 1 0 0 8a4 4 0 0 0 0-8zM6 8a6 6 0 1 1 12 0A6 6 0 0 1 6 8zm2 10a3 3 0 0 0-3 3a1 1 0 1 1-2 0a5 5 0 0 1 5-5h8a5 5 0 0 1 5 5a1 1 0 1 1-2 0a3 3 0 0 0-3-3H8z"></path>"
          </svg>
    `;

  const svgChat = `
    <svg xmlns="http://www.w3.org/2000/svg" viewBox="0 0 24 24" width="1.66em" height="1.66em"><path fill="currentColor" d="M2 4v14l4.8-3.6c.346-.26.767-.4 1.2-.4h8a2 2 0 0 0 2-2V4a2 2 0 0 0-2-2H4a2 2 0 0 0-2 2Zm2 10V4h12v8H7.334a1.984 1.984 0 0 0-1.2.4L4 14Z"></path><path fill="currentColor" d="M22 22V9a2 2 0 0 0-2-2v11l-2.134-1.6a1.984 1.984 0 0 0-1.2-.4H7a2 2 0 0 0 2 2h7c.433 0 .854.14 1.2.4L22 22Z"></path></svg>
  `;


  function clearHistory(){
    try {
      if(!confirm("Are you sure you want to clear this chat...?")){return};
      localStorage.removeItem('localChat');
      $chatLog.history = [];
      $chatLog.lastID = 0;
    } catch (error) {
      alert('Process failed! Please verify this domain has access to local storage in your browser settings...');
    }
  }


  async function getAnswer(){
    try {
      if(!$chatLog.model){
        alert("Please select a model to begin...");
        return;
      }
      if(!prompt){
        alert("Please enter a prompt (question) and try again...");
        return;
      }
      aiResponse = undefined;
      // <!-- : Add the question to the chat history (note that the history get's passed with the next question etc.) -->
      const question = {role: 'user', content: prompt};
      $chatLog.lastID++;
      question.id = $chatLog.lastID;
      $chatLog.history.push(question);
      $chatLog = $chatLog;
      // <!-- : Make the Initial Request to the AI API -->
      setTimeout(async() => {
        aiResponse = await ollama.chat({
          model: $chatLog.model,
          messages: $chatLog.history,
          stream: true 
        })
        setTimeout(() => { // Make sure the question is at the top of the history panel.
          const child = document.getElementById(`prompt${question.id}`);
          child.scrollIntoView();
        }, 0);
        // <!-- : Add an entry to the chat log for the answer and append each "part" -->
        const answer = {role: 'assistant', content: ''};
        $chatLog.lastID++;
        answer.id = $chatLog.lastID;
        answer.content = '';
        $chatLog.history.push(answer);
        $chatLog = $chatLog;
        //
        for await (const part of aiResponse) {
          $chatLog.model,
          $chatLog.history[$chatLog.lastID - 1].content += part.message.content
          $chatLog = $chatLog;
        }
        // <!-- : Save chat to local storage -->
        localStorage.setItem('localChat', JSON.stringify($chatLog));
        prompt = '';
      }, 0);
    } catch (error) {
      alert('There was an error processing your request.'); 
      console.error(error);
    }
  }


  async function checkOllamaRunning(){
    ollamaRunning = false;
    await fetch('http://localhost:11434/')
    .then(async (res) => {
      ollamaRunning = res.status === 200;
      return
    })
    .catch((e) => {
      console.error('Error in checkOllamaRunning', e);
    });   
  }


  onMount(async () => {
    try {
      // Check if we have a saved chat in the local history
      let localChat = JSON.parse(localStorage.getItem('localChat')) ?? undefined;
      if(localChat){
        $chatLog = localChat;
      }
      await checkOllamaRunning();
      if(!ollamaRunning){return}; // : Abort if Ollama is not running
      modelsList = await ollama.list()
      modelsList = modelsList.models;
      if(modelsList && modelsList.length === 0){
        alert("You don't currently have any local models. Please rectify and try again...");
        return;
      }      
      ollamaFail = false;
      show = true;
    } catch (error) {
      if(!modelsList || modelsList?.history.length === 0){
        alert("Either Ollama is not running or you don't currently have any local models. Please rectify and try again...")
      } else {
        alert("There was an error processing your request...")
      }
      console.error('error in onMount', error); 
    }
  });


  /* 
    <!-- : Nice to haves....
      
      : Allow multiple models to be selected and multiple questions to be asked, then
        ~ run same question(s) through multiple models (be sure to only visit each model once to avoid reloading for each question)  
        ~ On completion feed back answers into another AI for comparitive analysis.

      : Create a batch of test questions covering:
        ~ basic maths
        ~ my brain teaser
        ~ politically sensitive material

      : Test Scenarios
        ~ Repeatedly ask the same very simple question
        ~ Get another model to rewrite/optimise the questions first
        ~ Get a model to grade another models work

      : Team Mode: Have automated chat between 2 or more AI
        ~ Give each a basic identity based on what its good at.
        ~ Start the conversation with a single prompt.
        ~ Define basic protocols for keeping conversations on track and avoiding infinite loops. I.e. need some sort of governer to keep on subject.
        ~ When each model responds have another verify the response has value, is relevant and if possible strip anything that isn't and also reword for best clarity etc.

    : -->
  */

</script>

  {#if !ollamaRunning }
    <br>
    <h2>Ollama not Running</h2>
  {:else if ollamaFail }
    <br>
    <center>
      <h2>No Local Models</h2>
      <p>
        Go to the <a href="/models">"Models"</a> page to download one or more models...
      </p> 
    </center>
  {:else if show }
    <div style="margin-block: .66em; background-color: #fcfcfc; padding: .22em; border-radius: var(--border-radius); border: 1px solid var(--color-border); display: flex; flex-direction: row;">
      <label for="promptText" style="padding-bottom: 0; padding-right: 1em;">
        Model
      </label>
      <select class="input" bind:value={$chatLog.model} style="padding-right: 1em; font-size:larger; flex: 1; border: transparent;">
        {#each modelsList as m}
          <option value="{m.name}">{m.name}</option>
        {/each}
      </select>
    </div>
    <p style="
      background-color: #fcfcfc; 
      border-radius: var(--border-radius); 
      sborder: 1px solid var(--color-border); 
      display: flex; 
      flex-direction: row;
      ">
      <label for="promptText" style="display: none;">Enter a prompt...</label>
      <textarea
        on:keydown={(keycode) => {
          switch (keycode.code) {
            case 'Enter':
              if(keycode.shiftKey){return};
              getAnswer();
              break;
      
            default:
              break;
          }
        }}
        class="input"
        name="promptText"
        style='
          flex: 1;
          padding: 1em;
          border-radius: 4px;
          resize: none;
border: 1px solid var(--color-border);

          '
        id="promptText"
        rows="3"
        placeholder="Enter your question here..."
        bind:value={prompt}
        />
    </p>
    <div style='
      display: grid;
      grid-template-columns: 1fr min-content min-content;
      grid-template-rows: auto;
      margin-bottom: 0;
      padding: .42em .66em;
      border-bottom: 1px solid var(--color-primary);
      box-shadow: var(--shadow-bottom);
    '>
    <span style="float: left; color: var(--color-highlight);">
      {@html svgChat}
    </span>
    <button class="btn-secondary" 
      style="margin: 0;"
      on:click={clearHistory}
      >
        Clear History
    </button>
    <button class="btn-primary" style="margin-left: .42em;"
      on:click={getAnswer}
      >
        Send
    </button>
  </div>

  {#if $chatLog?.history?.length > 0 }
    <div style="
      height: 62vh; 
      overflow-y: scroll;
      padding-inline: 1em;
      margin-top: .24em;
      "
      in:fade
      >
      <!-- : Chat History Items -->
      {#each $chatLog.history as chat}
        {#if chat.role === 'user' }
          <div id='prompt{chat.id}' 
            class="prompt" 
            in:fade
            >
            <span class="row-icon">{@html svgUser}</span> 
            <span class="chat-content">{chat.content}</span>
          </div>
        {:else}
          <div id='response{chat.id}' 
            class="response" 
            in:fade
            >
            <span class="row-icon">{@html svgAI}</span> 
            <span class="chat-content">{chat.content.lenth === 0 ? 'Waiting...' : chat.content}</span>
          </div>
        {/if}
      {/each}
    </div>
  {:else}
   <h2 style="margin: .42em 0 0; padding-bottom: 2px;">No chat history, ask a question to get started...</h2>
    <div style="
      min-height: 62vh; 
      overflow-y: scroll;
      padding-inline: 1em;
      "
      in:fade
      >
    </div>
  {/if}
{/if}

<style>

  .prompt:first-of-type{
    margin-top: 1em;
  }

  .prompt, .response{
    margin-bottom: .88em !important;
    background-color: rgba(255,255,255,.82);
    border: 1px solid var(--color-border);
    padding-inline: .42em;
    border-radius: var(--border-radius);
    text-align: left;
    display: grid;
    grid-template-columns: min-content 1fr;
    place-items: center;
    padding-block: .22em;
  }

  .prompt{
    border-radius: var(--border-radius) var(--border-radius) 0 0;
    border-bottom-color: transparent;
    margin-bottom: 0 !important;
  }

  .response{
    color:rgb(96, 96, 96);
    margin-top: 0;
  }

  .response:last-of-type{
    color: darkslategray;
    border: 1px solid var(--color-border);
    padding: .66em;
    bottom: 0;
    margin-bottom: 42em !important;
    background-color: white
  }

  .row-icon{
    margin-right: .42em; 
    margin-top: 0; 
    margin-bottom: auto;
  }

  .chat-content{
    margin-left: .24em;
     margin-right: auto;
  }

</style>

