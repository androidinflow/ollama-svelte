  <h1>About</h1>
  <p>
    This is a starter Svelte frontend for running local AI models through Ollama.
  </p>

  <div style="
    display: grid;
    grid-template-columns: 1fr 4fr;
    grid-template-rows: auto;
    ">
    <div class="info-card">
      <p class="info-label">Developed by</p>
      <div class="info-data">
        <p>Mark Hayes</p>
      </div>
    </div>
    <div class="info-card">
      <p class="info-label">Frontend</p>
      <div class="info-data">
        <p><a target='new' href="https://www.svelte.dev">Svelte</a>, using the <a href="https://www.npmjs.com/package/ollama">Ollama Javascript library</a></p>
      </div>
    </div>
    <div class="info-card">
      <p class="info-label">AI Provider</p>
      <div class="info-data">
        <p><a target='new' href="https://www.ollama.com">Ollama</a> (running locally), models can be downloaded from Ollamas <a target='new' href="https://www.ollama.com/libray">Models Library</a></p>
      </div>
    </div>
    <div class="info-card">
      <p class="info-label">
        Code Repository
      </p>
      <div class="info-data">
        <p>
          <a href="https://github.com/Mark2M/svollama-ai-portal/">
            GitHub
          </a>
        </p>
      </div>
    </div>
    <div class="info-card">
      <p class="info-label">What it does</p>
      <div class="info-data">
        <p>Provides a Svelte (desktop) frontend for the Ollama API.</p>
        <p>Stores chat history in local storage.</p>
        <p>Lists available models from the Ollama library (as of March 2024) for download and removal.</p>
        <p>Allows user to switch between different locally installed models.</p>
      </div>
    </div>
    <div class="info-card">
      <p class="info-label">What it doesn't do</p>
      <div class="info-data">
          <p>Share your data.</p>
          <p>Track your usage or browsing behaviour or anything else.</p>
          <p>Permanently store your chat history.<br><strong>However</strong>, local storage is used to remember chat history and the last loaded model. This data will persist until you clear the chat history manually (on the Chat page) or clear your browsers cache.</p>
          <p>Not intended for use on mobile devices.</p>
      </div>
    </div>

  </div>


<style>

  .info-card{
    display: grid;
    grid-column: 1 / 3;
    grid-template-columns: subgrid;
    gap: .24em;
    padding: .12em 1em;
    margin: .42em auto;
    border-radius: calc(var(--border-radius) * 2);
  }

  .info-label, .info-data, .info-data > p{
    background-color: rgba(255,255,255,.66);
    margin: 0;
    padding: 0 .42em;
  }
  
  .info-label{
    min-width: 8em;
    color: var(--color-primary-dark);
  }

  .info-card:last-of-type{
    margin-bottom: 22em;
  }

  .info-data p{
    margin-bottom: .22em;
    padding-top: 0;
  }

</style>
