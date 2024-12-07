<script lang="ts">
	import * as m from '$lib/paraglide/messages.js';
	import { writable } from 'svelte/store';
	import { Check, Search } from 'lucide-svelte';
	import ProfileButton from '$components/ProfileButton.svelte';

	let functionProfiles = writable<{[key: number]: number | null}>({});
	let showSearch = writable(false);
	let selectedFunctions = writable<Set<string>>(new Set());
	let currentProfile = writable(1);
	let currentSearch = writable("");

function handleUpdateProfileName(event: any) {
  // Handle update profile name event
}

function handleDropProfile(event: any) {
  // Handle drop profile event
}

function handleSelectProfile(event: any) {
  // Handle select profile event
}

const handleDragStart = (e: DragEvent, func: any) => {
    const draggingFunctions = selectedFunctions.size > 0 ? [...selectedFunctions] : [func.id];
    e.dataTransfer.setData("functions", JSON.stringify(draggingFunctions));
  };

  const handleDrop = (e: DragEvent, profileId: number) => {
    e.preventDefault();

    const dataTransfer = e.dataTransfer.getData("functions");

    try {
      // Parse the dragged function data
      const functionIds: number[] = JSON.parse(dataTransfer);

      if (Array.isArray(functionIds)) {
        profiles = profiles.map((profile) => ({
          ...profile,
          functions: (profile.functions || []).filter(
            (func) => !functionIds.some((id) => getFunctionName(id).name === func)
          ),
        }));

        profiles = profiles.map((profile) => {
          if (profile.id === profileId) {
            const updatedFunctions = [
              ...profile.functions,
              ...functionIds.map((funcId) => getFunctionName(funcId).name),
            ];

            return { ...profile, functions: [...new Set(updatedFunctions)] }; // Ensure no duplicates
          }
          return profile;
        });

        selectedFunctions.clear();
      } else {
        console.error("Invalid drag data: expected an array of function IDs.");
      }
    } catch (error) {
      console.error("Error parsing drag data", error);
    }
  };

  const getFunctionName = (functionId: number): any => {
    return functions.find((func) => func.id === functionId) || {
      name: "Unknown function",
      id: -1,
    };
  };

  const handleDragOver = (e: DragEvent) => {
    e.preventDefault();
  };


let profiles = [
	{
		id: 0,
		name: "global profile",
		custom_section_name: "lol",
		functions: [] as string[],
		settings: [] as any[]
	},
	{
		id: 1,
		name: "Profile 1",
		functions: [] as string[],
		settings: [
			{
				name: "obfuscation",
				state: true
			},
			{
				name: "mutationEngine",
				value: 4
			}
		]
	}
]
	// Type definitions
	type Function = {
		id: number;
		name: string;
		profile?: number | null;
	};

	function findProfileByFunctionName(name: string): string | null {
		return null;
	}

	function handleSelect(funcId: number, isShiftKey: boolean) {
		selectedFunctions.update(prev => {
			const newSet = new Set(prev);
			const funcIdString = funcId.toString();

			if (newSet.has(funcIdString)) {
				newSet.delete(funcIdString);
			} else {
				newSet.add(funcIdString);
			}

			return newSet;
		});
	}



	function handleDoubleClick(func: Function) {
		// Implement double-click logic
	}

	function toggleSearch() {
		showSearch.update(value => !value);
	}

	function handleSearchInput(event: Event) {
		const target = event.currentTarget as HTMLInputElement;
		currentSearch.set(target.value);
	}

	const functions: Function[] = [
		{ id: 1, name: "sub_1" },
		{ id: 2, name: "sub_2" },
		{ id: 3, name: "sub_3" },
		{ id: 4, name: "sub_4" },
		{ id: 5, name: "sub_5" },
		{ id: 6, name: "sub_6" },
		{ id: 7, name: "sub_7" },
		{ id: 8, name: "sub_8" },
		{ id: 9, name: "sub_94124214512512512512512512512341241241212412412" },
		{ id: 10, name: "sub_10" },
		{ id: 11, name: "sub_11" },
	];
</script>

<svelte:head>
	<title>SvelteKit + TypeScript + TailwindCSS starter</title>
</svelte:head>

<div class="w-screen h-screen bg-white p-10 flex flex-row gap-2 lol">
	<div class="w-1/4 min-w-1/4 h-full gap-2 flex flex-col">
		<div class="w-full h-full bg-[#e0e0e0] border-2 border-[#cbcbcb] overflow-hidden">
			<div class="w-full h-[3rem] border-b-2 border-[#cbcbcb] flex justify-between items-center px-2 overflow-hidden">
				<div class='flex flex-row gap-1 justify-center items-center'>
					<span class="text-[#5c5c5c]">Functions</span>
				</div>
				<div class="flex flex-row gap-2">
					<button on:click={toggleSearch} class="relative flex items-center bg-[#d6d6d6] rounded-md border-2 border-[#cbcbcb] hover:scale-[105%] hover:bg-[#e0e0e0] hover:border-[#aaaaaa] duration-300 transition-all min-w-8 w-8 hover:w-[5.8rem] h-8 overflow-hidden pl-1">
						<Search size={"20"} color='#5c5c5c'/>
						<span class="text-[#5c5c5c] absolute left-7 text-md break-keep text-ellipsis block overflow-hidden whitespace-nowrap">
							Search
						</span>
					</button>
				</div>
			</div>
			<div class="w-full h-full flex flex-col overflow-y-auto pb-12">
				<div class='w-full p-2'>

					<div 
					class="w-full {$showSearch 
					  ? 'translate-y-0 h-12 py-1 px-3' 
					  : '-translate-y-32 h-0'} 
					transition-all ease-in-out duration-500 outline-none focus:outline-none"
				  >
					<input 
					  value={$currentSearch} 
					  on:input={handleSearchInput}
					  class="w-full h-full border text-black rounded-sm border-[#bfbfbf] px-2 text-lg bg-[#efefef] focus:outline-none"
					/>
				  </div>
				</div>

				<!-- svelte-ignore a11y_no_static_element_interactions -->
				<div 
					on:drop|preventDefault={(e) => handleDrop($currentProfile, e)}
					on:dragover|preventDefault
				>
					{#each functions as func (func.id)}
						<button 
							type="button"
							draggable={true}
							class="w-full {findProfileByFunctionName(func.name) ? 'bg-white/[0.5]' : ''} 
								   {!findProfileByFunctionName(func.name) ? 'min-h-7 h-7' : 'h-10'} 
								   hover:bg-white/[0.4] text-black transition-all duration-300 px-2 overflow-hidden"
							on:dragstart={() => handleDragStart(func)}
							on:click={(e) => handleSelect(func.id, e.shiftKey)}
							on:dblclick={() => handleDoubleClick(func)}
						>
							<div class='flex justify-between items-center transition-all'>
								<div class='flex flex-row justify-center items-center gap-2 transition-all overflow-hidden'>
									<!-- <IconMathIntegralX size={20} color='#5c5c5c' class='transition-all' style="flexShrink: 0"/> -->
									<div class="leading-5 flex flex-col justify-center items-start h-full transition-all overflow-hidden w-full">
										
										<span class="{func.profile == null ? 'text-sm' : 'text-md'} w-full truncate text-ellipsis overflow-hidden whitespace-nowrap">

											{func.name}
										</span>

										{#if findProfileByFunctionName(func.name)}
											<span class="text-xs">
												Profile: {findProfileByFunctionName(func.name)}
											</span>
										{/if}
									</div>
								</div>

								<div class="transition-all">
									<div class="{$selectedFunctions.has(func.id.toString()) ? 'opacity-100 translate-y-0' : 'opacity-0 translate-y-4'} transition-all">
										<Check stroke={"5px"} />
									</div>
								</div>
							</div>
						</button>
					{/each}
				</div>
			</div>
		</div>
	</div>










	<div class="w-[51%] h-full bg-[#e0e0e0] rounded-xs border-2 border-[#cbcbcb] flex flex-row">
		<div class="w-2/6 h-full border-r-2 border-[#cbcbcb]">
			<div class="w-full h-[3rem] border-b-2 border-[#cbcbcb] flex justify-between items-center overflow-hidden">
				<div class='flex flex-row gap-1 justify-center items-center px-2'>
					<!-- <IconUserCog size={20} color='#3f8cff'/> -->
					<span class="text-[#5c5c5c]">Profiles</span>
				</div>
			</div>
			<div class="flex flex-col gap-2 p-2 ">


				{#each profiles.slice(1) as profile (profile.id)}
					<ProfileButton 
						profile={profiles[profile.id]} 
						currentProfile={currentProfile}
						on:updateProfileName={handleUpdateProfileName}
						on:drop={handleDropProfile}
						on:selectProfile={handleSelectProfile}
					/>
				{/each}
				<!-- {data.slice(1).map((profile: any) => (
					<ProfileButton profile={profile} currentProfile={currentProfile} handleDrop={handleDrop} setCurrentProfile={setCurrentProfile} getFunctionsForProfile={getFunctionsForProfile} setProfileName={setProfileName}/>
				))} -->
			</div>
		</div>
		<div class="w-4/6 h-full overflow-y-hidden">
			<div class="w-full h-[3rem] flex justify-between items-center px-2  border-b-2 border-[#cbcbcb]">
				<div class='flex flex-row gap-1 justify-start items-center'>
					<!-- <IconSettingsFilled size={20} color='#3f8cff'/> -->
					<span class="text-[#5c5c5c]">Settings</span>
				</div>
				<div class="flex flex-row gap-2">					
					<!-- <NewProfileButton addNewProfile={addNewProfile}/>

					<ExportButton/>

					<ImportButton/>

					<ResetButton resetProfiles={resetAll}/> -->
				</div>
			</div>
			<div class="bg-[#eee] max-h-full overflow-auto relative">
					<div class='flex flex-row w-full sticky bg-[#e0e0e0] top-0 z-10 h-[3rem] border-b-2  border-[#cbcbcb] justify-start items-center px-2'>
						<!-- <IconCode size={20} color='#5c5c5c'/> -->
						<div class="w-full leading-3  flex flex-col justify-center items-start px-2">
							<span class="text-[#5c5c5c] text-md mt-1">Obfuscation</span>
							<span class="text-[#5c5c5c] text-xs">Contains 7 options</span>
						</div>
					</div>
					<div class='w-full  p-2 flex flex-col gap-0.5'>
	
<!-- 
							<ObfuscationCheckbox id={1} getCurrentValue={getCurrentValue} toggleOption={toggleOption} title="Simple Constant Obfuscation" description="Simple constant obfuscation" tooltip="No data here" link="https://google.com" option="simpleconstobfuscation" type="boolean"/>

							<SliderNumber id={2} getCurrentValue={getCurrentValue} toggleOption={toggleOption} title="Mutation engine" description="This will enable the mutation engine with mutation" max={5} tooltip="lfdsdfsdadsfsdfsdfol" link="https://google.com" option="mutationEngine"/>

							<ObfuscationCheckbox id={3} getCurrentValue={getCurrentValue} toggleOption={toggleOption} title="Opaque Block Duplication" description="adsfsdfdsfsdafdsfsd" tooltip="No data here" link="https://google.com" option="opaqueblock" type="boolean"/>

							<ObfuscationCheckbox id={4} getCurrentValue={getCurrentValue} toggleOption={toggleOption} title="IDA Decompiler Crasher" description="Crashes IDA Decompiler when trying to decompile functions" tooltip="No data here" link="https://google.com" option="idadecompcrasher" type="boolean"/>

							<ObfuscationCheckbox id={5} getCurrentValue={getCurrentValue} toggleOption={toggleOption} title="Dematerialize constants" description="assaddsfsdafsadfasdfsadf" tooltip="No data here" link="https://google.com" option="dematerialize_constants" type="boolean"/>

							<ObfuscationCheckbox id={6} getCurrentValue={getCurrentValue} toggleOption={toggleOption} title="Loop-based constant encoding" description="assaddsfsdafsadfasdfsadf" tooltip="No data here" link="https://google.com" option="loop_constant" type="boolean"/>

							<ObfuscationCheckbox id={7} getCurrentValue={getCurrentValue} toggleOption={toggleOption} title="Mixed Boolean Arithmetic" description="Also known as MBA Obfuscation" tooltip="No data here" link="https://google.com" option="mba" type="boolean"/>


  -->
					</div>
					

					<div class='flex flex-row w-full sticky bg-[#e0e0e0] top-0 z-10 h-[3rem] border-b-2  border-[#cbcbcb] justify-start items-center px-2'>
						<!-- <IconCode size={20} color='#5c5c5c'/> -->
						<div class="w-full leading-3  flex flex-col justify-center items-start px-2">
							<span class="text-[#5c5c5c] text-md mt-1">Obfuscation</span>
							<span class=" text-[#5c5c5c] text-xs">Contains 7 options</span>
						</div>
					</div>
					<div class="">
						<!-- <ObfuscationCheckbox id={21} getCurrentValue={getCurrentValue} toggleOption={toggleOption} title="No unsafe assumptions" description="aasdasdsdafadsfsadfasdfsadf" tooltip="No data here" link="https://google.com" option="no_unsafe" type="boolean"/>
						<ObfuscationCheckbox id={22} getCurrentValue={getCurrentValue} toggleOption={toggleOption} title="BMI2" description="aadasfsadfgsdfgsdfgsdafgsdgsdgsadgsdg" tooltip="No data here" link="https://google.com" option="bmi2" type="boolean"/>
						<ObfuscationCheckbox id={23} getCurrentValue={getCurrentValue} toggleOption={toggleOption} title="BMI1" description="asdgsadgsadgsadgasdgasdgasdg" tooltip="No data here" link="https://google.com" option="bmi1" type="boolean"/> -->
						<div class='h-12 bg-[#e0e0e0] '></div>
					</div>

					
				</div>


		</div>



		</div>



















	
</div>