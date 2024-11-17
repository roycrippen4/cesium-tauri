<script lang="ts">
  import { Ion, Viewer } from 'cesium';
  import 'cesium/Build/Cesium/Widgets/widgets.css';
  import { onMount } from 'svelte';
  import { invoke } from '@tauri-apps/api/core';

  Ion.defaultAccessToken =
    'eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJqdGkiOiI5NjNmM2ZjNS0yMjhhLTRiNGMtYjNkZS1hMzA4NGViMTRlZTYiLCJpZCI6MTY3OTExLCJpYXQiOjE2OTUzNjIxMjZ9.m9st8lwGsSaP6ZjXUKVomVHVu-liiEskl4nLbQoj_KQ';

  let viewer: Viewer;
  onMount(() => {
    viewer = new Viewer('cesiumContainer', {
      animation: false,
      homeButton: false,
      timeline: false,
      fullscreenButton: false,
    });
  });

  let name = $state('');
  let greetMsg = $state('');

  async function greet(event: Event) {
    event.preventDefault();
    greetMsg = await invoke('greet', { name });
  }
</script>

<div class="flex flex-col">
  <main id="cesiumContainer" class="h-full w-full"></main>
  <label class="p-2">
    <input type="text" placeholder="Enter a name to greet..." bind:value={name} />
    <button type="submit" class="h-fit rounded-sm border border-gray-300 p-2" onclick={greet}>Greet</button>
  </label>
  {console.log(greetMsg)}
  {#if greetMsg}
    <p>{greetMsg}</p>
  {/if}
</div>
