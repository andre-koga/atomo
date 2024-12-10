<script lang="ts">
  import "../styles.css";
  import Footer from "../ui/layout/+Footer.svelte";
  import Header from "../ui/layout/+Header.svelte";
  import Sidebar from "../ui/layout/+Sidebar.svelte";

  let sidebarWidth: number = 250; // Initial width of the sidebar
  const minSidebarWidth = 200;
  const maxSidebarWidth = 400;
  let isResizing = false;

  function handleMouseDown(event: MouseEvent) {
    isResizing = true;
    document.addEventListener("mousemove", handleMouseMove);
    document.addEventListener("mouseup", handleMouseUp);
    // change cursor to col-resize:
    document.body.style.cursor = "col-resize";
  }

  function handleMouseMove(event: MouseEvent) {
    if (isResizing) {
      sidebarWidth = Math.min(
        Math.max(event.clientX, minSidebarWidth),
        maxSidebarWidth
      );
    }
  }

  function handleMouseUp() {
    isResizing = false;
    document.removeEventListener("mousemove", handleMouseMove);
    document.removeEventListener("mouseup", handleMouseUp);
    // reset cursor:
    document.body.style.cursor = "";
  }
</script>

<div
  class="grid overflow-hidden h-screen grid-cols-[auto,1px,auto] gap-[1px] grid-rows-[2rem,1fr,1.5rem] dark:text-white dark:bg-zinc-800"
>
  <Header />
  <Sidebar width={sidebarWidth} />
  <button
    class="w-1 transition-colors -mx-1 bg-transparent cursor-ew-resize z-50 hover:bg-zinc-600"
    aria-label="Resizer"
    on:mousedown={handleMouseDown}
  ></button>
  <div class="overflow-hidden">
    <slot></slot>
  </div>
  <Footer />
</div>

<style>
</style>
