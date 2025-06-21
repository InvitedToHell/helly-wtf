<script lang="ts">
	import Model from '$lib/components/Model.svelte';
	import { useLanyard } from '$lib/lanyard';
	const presence = useLanyard({ method: 'rest', id: '469514043053309952' });

	// Check the presence data
	$: {
		console.log($presence); // This will print the data whenever it changes
	}

	// Variables to hold activity images
	let largeActivityImage: string;
	let largeActivityImage2: string;
	let time: string;

	// Function to update activity image and status
	function updateActivityImage(prsnce: any) {
		if ($presence !== undefined && Array.isArray($presence.activities)) {
			for (let i = 0; i < $presence.activities.length; i++) {
				if ($presence.activities[i].assets?.large_image) {
					const activityImage = $presence.activities[i].assets.large_image;

					if (activityImage.includes('spotify')) {
						const largeImage = activityImage.substring(activityImage.indexOf(':') + 1);
						console.log('Found Spotify activity at index ' + i); // Debugging line
						if (i === 0) {
							largeActivityImage = 'https://i.scdn.co/image/' + largeImage;
						} else if (i === 1) {
							largeActivityImage2 = 'https://i.scdn.co/image/' + largeImage;
						}
					}
				}
			}
		}
	}

	// Function to update border color based on presence status
	let borderColor = '#747f8d';
	function updateBorderColor(prsnce: any) {
		if ($presence !== undefined) {
			if ($presence.discord_status === 'streaming') {
				borderColor = '#593695';
			} else if ($presence.discord_status === 'idle') {
				borderColor = '#faa61a';
			} else if ($presence.discord_status === 'dnd') {
				borderColor = '#f04747';
			} else if ($presence.discord_status === 'online') {
				borderColor = '#43b581';
			} else if ($presence.discord_status === 'offline') {
				borderColor = '#747f8d';
			} else {
				borderColor = '#747f8d';
			}
		}
	}

	// Function to calculate time since activity started
	function timeStamps(prsnce: any) {
		if ($presence !== undefined) {
			if ($presence.activities.length > 1 && $presence.activities[1].timestamps) {
				const start = $presence.activities[1].timestamps.start;
				console.log(JSON.stringify($presence.activities[1].timestamps));
				const elapsed = Math.floor(Date.now() / 1000) - start / 1000;
				let hours: any = Math.floor(elapsed / 3600);
				let minutes: any = Math.floor((elapsed - hours * 3600) / 60);
				let seconds: any = Math.floor(elapsed - hours * 3600 - minutes * 60);
				if (hours == '0') {
					if (minutes == '0') {
						time = 'for ' + seconds + 's';
					} else {
						time = 'for ' + minutes + 'min';
					}
				} else {
					time = 'for ' + hours + 'h ' + minutes + 'min';
				}
			}
		}
	}

	// Reactive updates
	$: updateActivityImage($presence);
	$: updateBorderColor($presence);
	$: timeStamps($presence);

	let linktreeSection: HTMLElement;

	function scrollToLinktree() {
		linktreeSection.scrollIntoView({ behavior: 'smooth' });
	}
</script>

<body class="bg-base text-text min-h-screen">
	<section id="welcome" class="text-text">
		<div class="h-screen lg:text-[14rem] items-center justify-center flex">
			<div>
				<div class="transform w-[80vw] text-center">
					<Model />
				</div>
				<div class="text-center text-lg">
					<a href="https://ctp-webr.ing/helly/previous">&larr; </a><a href="https://ctp-webr.ing/"
						>Catppuccin webring
					</a><a href="https://ctp-webr.ing/helly/next"> &rarr;</a>
				</div>
			</div>
		</div>
		<section id="linktree" bind:this={linktreeSection} class="text-text">
			<a href="#linktree" on:click|preventDefault={scrollToLinktree}><span /> </a>
		</section>

		<section id="linktree" class="demo text-text">
			<div
				class="h-screen flex text items-center justify-center bg-mantle text-[#cdd6f4] md:text-[5rem]"
			>
				<div class="animate__animated animate__fadeInLeftBig text-text">
					<div class="telegram transform hover:text-white">
						<a href="https://t.me/invitedtohell">telegram</a>
					</div>

					<div class="github transform hover:text-white">
						<a href="https://github.com/invitedtohell">github</a>
					</div>

					<div class="mastodon transform hover:text-white">
						<a href="https://social.catppuccin.com/@helly">mastodon</a>
					</div>

					<div class="email transform hover:text-white">
						<a href="mailto:contact@pompomsoft.de">email</a>
					</div>

					<div class="planetmc transform hover:text-white">
						<a href="https://www.planetminecraft.com/member/invitedtoheaven/">planetmc</a>
					</div>
				</div>
			</div>
		</section>

		<!-- Lanyard -->
		<div class="h-screen flex text items-center justify-center bg-mantle">
			<div class="animate__animated animate__fadeInLeftBig items-center">
				<div class="animate__animated animate__fadeInLeftBig items-center w-max h-max">
					{#if $presence}
						<div class="bg-crust text-text md rounded-lg overflow-hidden lg:w-[30vw] pt-5 pb-5">
							<!-- User Avatar -->
							<div class="flex flex-auto justify-left pl-5 items-left">
								<div class="p-1">
									<!-- svelte-ignore a11y-missing-attribute -->
									<img
										class="!w-20 !h-20 rounded-full mx-auto border-solid border-4"
										style="border-color: {borderColor};"
										src="https://cdn.discordapp.com/avatars/{$presence.discord_user.id}/{$presence
											.discord_user.avatar}"
									/>
								</div>
								<div class="text-left px-2 flex flex-col content-center justify-center">
									<h2 class="text-3xl bold">InvitedToHell</h2>
									<h2 class="text-2xl" style="color: {borderColor}">
										{$presence.discord_status}
									</h2>
								</div>
							</div>

							<!-- Spotify Activities -->
							{#if $presence && $presence.activities && $presence.activities.length > 0}
								{#each $presence.activities as activity, i}
									{#if activity.assets?.large_image && activity.assets.large_image.includes('spotify')}
										<div class="px-6 py-4" key={i}>
											<p class="bold text-purple mb-2 text-2xl text-left">
												{activity.name}
											</p>
											<div class="flex row-auto items-center">
												<!-- svelte-ignore a11y-img-redundant-alt -->
												<img
													class="rounded-lg"
													src="https://i.scdn.co/image/{activity.assets.large_image.split(':')[1]}"
													alt="Spotify Activity Image"
													width="70"
													height="70"
												/>
												<div class="flex-col flex-auto pl-5 rounded-lg">
													{#if activity.state}
														<p class="regular text-text text-lg text-left content-center">
															{activity.state}
														</p>
													{/if}
													{#if activity.details}
														<p class="regular text-text text-lg text-left content-center">
															{activity.details}
														</p>
													{/if}
												</div>
											</div>
										</div>
									{/if}
								{/each}
							{/if}
						</div>
					{/if}
				</div>
			</div>
		</div>
	</section></body
>
