<script lang="ts">
  import { getCurrentWindow } from "@tauri-apps/api/window";
  import {
    Minus,
    Minimize as Unmaximize,
    Square as Maximize,
    X as Close,
    Atom,
    Menu,
  } from "lucide-svelte";
  import { onDestroy, onMount } from "svelte";
  import { listen } from "@tauri-apps/api/event";

  let isMaximized = false;
  let unlisten: () => void;

  onMount(async () => {
    unlisten = await listen("tauri://resize", async () => {
      isMaximized = await getCurrentWindow().isMaximized();
    });
  });

  onDestroy(() => {
    unlisten();
  });

  async function minimize() {
    getCurrentWindow().minimize();
  }

  async function maximize() {
    console.log("maximize");
    if (await getCurrentWindow().isMaximized()) {
      getCurrentWindow().unmaximize();
      isMaximized = false;
    } else {
      getCurrentWindow().maximize();
      isMaximized = true;
    }
  }

  async function close() {
    getCurrentWindow().close();
  }
</script>

<header
  class="col-span-3 bg-zinc-900 flex justify-between items-stretch titlebar"
>
  <div class="title flex items-center gap-4 self-center px-2">
    <p class="flex gap-2"><Atom color="#F1A049" size="18px" /></p>
    <button><Menu size="18px" class="text-zinc-500" /></button>
  </div>
  <div class="window-controls flex">
    <button on:click={minimize} class="hover:bg-zinc-700"
      ><Minus size="18px" /></button
    >
    <button on:click={maximize} class="hover:bg-zinc-700"
      >{#if isMaximized}
        <Unmaximize size="16px" />
      {:else}
        <Maximize size="14px" />
      {/if}
    </button>
    <button on:click={close} class="hover:bg-red-600"
      ><Close size="18px" /></button
    >
  </div>
</header>

<style>
  .titlebar {
    -webkit-app-region: drag;
  }

  .window-controls {
    -webkit-app-region: no-drag;
  }

  .window-controls button {
    @apply px-4 transition-colors;
  }
</style>
