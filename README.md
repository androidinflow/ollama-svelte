# SvelteKit/Ollama AI Frontend

This is a very basic frontend for the local Ollama API built with SvelteKit.

## Install Ollama

(MacOs) Simply download the zip https://ollama.com/download and extract its contents to the Applications folder and that's it. 

The first time you run Ollama you will be prompted to "Run your first model". Simply copy the command, open a terminal window and run it.

Ollama is installed and will auto start with the operating system. Note that Ollama will unload after 4 minutes of inactivity.

Important: Depending on you connection speed Ollama may take a while to start if you are running (and pulling) a model for the first time. 

Once downloaded Ollama will open the local version.

## Clone this Repo

Clone the repo here [https://github.com/Mark2M/svollama-ai-portal](https://github.com/Mark2M/svollama-ai-portal).

## Install the Dependencies

Once you've created a project and installed dependencies with `npm install` (or `pnpm install` or `yarn`).

## Start a development server
```bash
npm run dev

# or start the server and open the app in a new browser tab
npm run dev -- --open
```

## Building

To create a production version of your app:

```bash
npm run build
```

You can preview the production build with `npm run preview`.

> To deploy your app, you may need to install an [adapter](https://kit.svelte.dev/docs/adapters) for your target environment.
