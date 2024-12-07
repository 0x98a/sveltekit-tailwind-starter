<script lang="ts">
    import { createEventDispatcher } from 'svelte';
  
    // Props interface
    export let profile: {
      id: any;
      name: string;
      functions: any[];
    };
    export let currentProfile: any | null = null;
  
    // State variables
    let editState = false;
    let profileName = profile.name;
  
    // Event dispatcher
    const dispatch = createEventDispatcher();
  
    // Handle profile name change and save
    function handleChange() {
      dispatch('updateProfileName', { 
        id: profile.id, 
        name: profileName 
      });
      editState = false;
    }
  
    // Handle drop event
    function handleDrop(event: DragEvent) {
      event.preventDefault();
      dispatch('drop', { 
        profileId: profile.id, 
        event 
      });
    }
  
    // Handle double-click to select profile
    function selectProfile() {
      dispatch('selectProfile', { 
        profileId: profile.id 
      });
    }
  </script>
  
  <button 
    type="button"
    class="w-full {profile.functions.length > 0 ? 'h-12' : 'h-8'} bg-[#ebebeb] rounded-sm border-2 transition-all border-[#cbcbcb] flex flex-col justify-center items-start px-2"
    on:dragover|preventDefault
    on:drop|preventDefault={handleDrop}
    on:dblclick={selectProfile}
  >
    <div class="flex justify-between items-center w-full h-full text-black">
      <div class="flex flex-row justify-center items-center">
        <div 
          class="{editState 
            ? 'translate-x-0 w-8 z-10 scale-100' 
            : '-translate-x-2 w-0 z-0 scale-0'} duration-500 transition-all ease-in-out"
        >
          <!-- <EditCircle size={20} /> -->
        </div>
        
        <div class="flex w-full flex-col justify-center items-start leading-5 py-3 text-black">
          {#if editState}
            <input 
              class="
                {profile.name.length > 20 ? 'text-sm' : ''} 
                {profile.name.length > 30 ? 'text-xs' : ''} 
                w-60 text-black bg-[#ebebeb] focus:outline-none underline transition-all
              "
              bind:value={profileName}
              on:blur={handleChange}
            />
          {:else}
            <span 
              class="
                {profile.name.length > 20 ? 'text-sm' : ''} 
                {profile.name.length > 30 ? 'text-xs text-black' : ''} 
                max-w-72 truncate transition-all
              "
              on:dblclick={() => editState = true}
            >
              {profile.name}
            </span>
          {/if}
          
          {#if profile.functions.length > 0}
            <span class="text-xs text-gray-500">
              Contains {profile.functions.length} function{profile.functions.length !== 1 ? 's' : ''}
            </span>
          {/if}
        </div>
      </div>
      
      <div 
        class="
          {profile.functions.length > 0 ? 'h-9 w-11' : 'h-6 w-7'} 
          {currentProfile === profile.id ? 'opacity-100' : 'opacity-0'} 
          flex justify-center items-center transition-all
        "
      >
        <div class="w-4 h-4 rounded-full bg-[#1c8affc6] border-2 border-[#ffffff96]"></div>
      </div>
    </div>
  </button>
  
  <style>
    /* You can add any additional styles here if needed */
  </style>