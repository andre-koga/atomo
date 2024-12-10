<script lang="ts">
  import { File } from "lucide-svelte";

  export let width: number; // Initial width of the sidebar

  const tabs = ["Home", "About", "Contact"];
  let chosenTab: number | null = 0;

  function changeTab(i: number) {
    if (chosenTab === i) return;
    chosenTab = i;
  }
</script>

<nav
  style="width: {width}px;"
  class="sidebar bg-zinc-800 gap-[1px] overflow-hidden grid grid-cols-[2.5rem,auto]"
>
  <nav class="bg-zinc-900 py-1 vertical-buttons items-center flex flex-col">
    {#each tabs as tab, i}
      <button
        on:click={() => changeTab(i)}
        class="{chosenTab == i ? '' : 'text-zinc-600'} py-2">{tab}</button
      >
    {/each}
  </nav>

  <div class="bg-zinc-900">
    {chosenTab}
    this is the sidebar aeluhu wieuifhawefh awlkejfhla kewjfhr
  </div>
</nav>
