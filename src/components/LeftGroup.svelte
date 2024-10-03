<script lang="ts">
  import type {
    BatteryOutput,
    CpuOutput,
    MemoryOutput,
    NetworkOutput,
    KeyboardOutput
  } from "zebar";

  import Meter from "./Meter.svelte";

  type LeftGroupProps = {
    battery: BatteryOutput;
    cpu: CpuOutput;
    memory: MemoryOutput;
    keyboard: KeyboardOutput;
    network: NetworkOutput;
  };

  let { battery, cpu, memory, network, keyboard }: LeftGroupProps = $props();
</script>

<i></i>
<div class="flex flex-row gap-3 items-center">
  <div class="flex gap-1 items-center">
    <i class="ti ti-ruler-2"></i>
    <Meter class="bg-zb-memory" percent={Math.round(memory?.usage ?? 0)} />
  </div>
  <div class="flex gap-1 items-center">
    <i class="ti ti-cpu"></i>
    <Meter class="bg-zb-cpu" percent={Math.round(cpu?.usage ?? 0)} />
  </div>
  <div class="flex gap-1 items-center">
    <i class="ti ti-bolt"></i>
    <Meter
      class="bg-zb-battery-good"
      percent={Math.round(battery?.chargePercent ?? 100)}
    />
  </div>
  <div class="flex flex-row items-center gap-1">
    {#if network?.defaultInterface?.type === "ethernet"}
      <i class="ti ti-network"></i>
    {:else if network?.defaultInterface!.type === "wifi"}
      {#if network.defaultGateway!.signalStrength! >= 75}
        <i class="ti ti-wifi"></i>
      {:else if network.defaultGateway!.signalStrength! >= 50}
        <i class="ti ti-wifi-2"></i>
      {:else if network.defaultGateway!.signalStrength! >= 25}
        <i class="ti ti-wifi-1"></i>
      {:else}
        <i class="ti ti-wifi-off"></i>
      {/if}
      {network.defaultGateway?.ssid}
    {:else}
      <i class="ti ti-wifi-off"></i>
    {/if}
  </div>
  {#if keyboard}
    <div>
      <i class="nf nf-fa-keyboard"></i>
      {keyboard.layout.substring(3, 5)}
    </div>
  {/if}
  <i></i>
</div>
