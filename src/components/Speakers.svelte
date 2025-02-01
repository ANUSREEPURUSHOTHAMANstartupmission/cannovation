<script>
  import {onMount} from 'svelte';

  let speaker_list = [];

  onMount(()=>{
    fetch(`https://events.startupmission.in/api/event/wss-2021/speakers?category=speaker`)
        .then(response => response.json())
        .then((json) => {
          speaker_list = json;
        });
  });

</script>

{#each Object.entries(speaker_list) as [category, speakers]}
  <div class="mx-auto speakers__container ">

    <div class="grid grid-cols-2 gap-10 md:grid-cols-4 sm:grid-cols-3">
      {#each speakers as {name, designation, organisation, photo, linkedin}}
        
        <div class="relative h-auto">

          <div style="transform-origin: 1.2rem 1.5rem;" class="mx-auto h-full transition ease-in-out transform shadow speakers__item group">
            <div class="speakers__photo mb-3">
              <img src="{photo}" alt="name" class="transition duration-300  ">
            </div>
            <div class="px-2 pb-2 group-hover:text-red-500 ">
              <div class="text-lg font-bold  speakers__name">{name}</div>
              <div class="text-sm  speakers__designation">{designation}</div>
              <div class="text-sm  speakers__organisation">{organisation}</div>
             
            </div>
          </div>

        </div>
      {/each}
    </div>

  </div>
{/each}
