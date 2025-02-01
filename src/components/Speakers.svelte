<script>
  import {onMount} from 'svelte';

  let speaker_list = [];

  export let title;
  export let label;
  export let category;
  export let slug;

  let full_view = true;

  function findPos(obj) {
    var curtop = 0;
    if (obj.offsetParent) {
        do {
            curtop += obj.offsetTop;
        } while (obj = obj.offsetParent);
    return curtop;
    }
  }

  function handleView(e) {
    full_view = true;

    console.log(findPos(document.getElementById("speakers"))+1, findPos(document.getElementById("speakers")) );

    window.scroll({
      left:0,
      top:findPos(document.getElementById(category))+500, 
      behavior:'smooth'
    });

  }

  onMount(()=>{
    fetch(`https://events.startupmission.in/api/event/${slug}/speakers?category=${category}`)
        .then(response => response.json())
        .then((json) => {
          speaker_list = json;
        });
  });

</script>


{#each Object.entries(speaker_list) as [category, speakers]}
  <div class="mx-auto speakers__container overflow-hidden { full_view ? "" : "max-h-[1000px]"} openTrans relative">

    <div class="grid grid-cols-2 gap-3  lg:grid-cols-4 md:gap-10 md:grid-cols-4 sm:gap-5">


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

    {#if !full_view}
      <div  class="flex items-center justify-center bg-gradient-to-t from-huddle from-25% via-[#c70000bd] to-transparent h-24 bottom-0 absolute w-full z-[10]">
        <button on:click={handleView} class="border border-white text-white rounded-full px-5 py-3">
          View More
        </button>
      </div>
    {/if}


  </div>
{/each}
