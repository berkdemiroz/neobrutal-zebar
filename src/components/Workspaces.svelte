<script lang="ts">
  import type { Window } from "glazewm";
  import type { GlazeWmOutput } from "zebar";

  import iconMap from "$lib/icon_map.json";
  import ignoredApps from "$lib/ignored_apps.json";

  import Button from "./Button.svelte";

  const getProcessIcon = (child: Window) => {
    const possibleAppNames = [
      child.title.toLowerCase(),
      child.processName.toLowerCase()
    ];

    if (ignoredApps.find((app) => possibleAppNames.includes(app.name))) return;

    let entry = iconMap.find((entry) =>
      entry.appNames
        .map((name) => name.toLowerCase())
        .some((name) => possibleAppNames.includes(name))
    );

    return entry?.iconName ?? "ti-background";
  };

  let { glazewm }: { glazewm: GlazeWmOutput } = $props();
</script>

{#if glazewm}
  <div class="flex flex-row gap-2 items-center">
    {#each glazewm.currentWorkspaces as workspace, i}
      <button
        class="flex items-center justify-center h-5 w-5 text-xs font-medium
               border border-zb-border rounded-base
               {workspace.hasFocus
          ? 'bg-[hsl(var(--nord7)_/_0.3)] text-[hsl(var(--nord7))]'
          : 'bg-[hsl(var(--nord6)_/_0.2)] text-[hsl(var(--nord6))]'}"
        onclick={() =>
          glazewm!.runCommand(`focus --workspace ${workspace.name}`)}
        aria-label={`Workspace ${i + 1}`}
      >
        {workspace.name}
      </button>
    {/each}
    <button
      aria-label="tiling-direction"
      class="flex items-center justify-center text-zb-tiling-direction h-5 w-5"
      onclick={() => glazewm!.runCommand("toggle-tiling-direction")}
    >
      <i class="ti ti-switch-{glazewm?.tilingDirection}"></i>
    </button>
  </div>
{/if}
