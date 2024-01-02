<script lang="ts">
	import { useLanyard } from '$lib/lanyard';

	const presence = useLanyard({ method: 'rest', id: '469514043053309952' });
	let borderColor = '#747f8d';
	let largeActivityImage: string;
	let largeActivityImage2: string;
	let time: string;
	let active1: string;
	let active2: string;
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

	function updateActivityImage(prsnce: any) {
		if ($presence !== undefined) {
			if ($presence.activities.length > 1 && $presence.activities[1].assets) {
				if (
					$presence.activities[1].assets !== undefined &&
					$presence.activities[1].assets.large_image !== undefined
				) {
					if ($presence.activities[1].assets.large_image.includes('spotify')) {
						const largeImage1 = $presence.activities[1].assets.large_image.substring(
							$presence.activities[1].assets.large_image.indexOf(':') + 1
						);
						largeActivityImage = 'https://i.scdn.co/image/' + largeImage1;
						active1 = 'spotify';
					} else {
						const largeImage1 = $presence.activities[1].assets.large_image;
						largeActivityImage =
							'https://' + largeImage1.substring(largeImage1.indexOf('https/') + 6);
						active1 = 'other';
					}
				}
				if ($presence.activities.length > 2 && $presence.activities[2].assets.large_image)
					if (
						$presence.activities[2].assets !== undefined &&
						$presence.activities[1].assets.large_image !== undefined
					) {
						if ($presence.activities[2].assets.large_image.includes('spotify')) {
							const largeImage2 = $presence.activities[2].assets.large_image.substring(
								$presence.activities[2].assets.large_image.indexOf(':') + 1
							);
							largeActivityImage2 = 'https://i.scdn.co/image/' + largeImage2;
							active2 = 'spotify';
						} else {
							const largeImage2 = $presence.activities[2].assets.large_image;
							largeActivityImage2 =
								'https://' + largeImage2.substring(largeImage2.indexOf('https/') + 6);
							active2 = 'other';
						}
					}
				console.log(largeActivityImage + ' ' + largeActivityImage2);
			}
		}
	}

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
	$: updateActivityImage($presence);
	$: updateBorderColor($presence);
	$: timeStamps($presence);
</script>

<pre>
    <!-- Because presence is a reactive store, use $presence to access the data -->
</pre>

<body class="bg-base text-text min-h-screen">
	<div class="h-screen heading lg:text-[14rem] flex items-center justify-center">
		<h1 class="text-white animate__animated animate__fadeInLeftBig transform w-[90vw] text-center">
			Helly
		</h1>
		<section id="section01" class="demo text-text">
			<a href="#section02"><span /> </a>
		</section>
	</div>
	<div class="h-screen flex text items-center justify-center bg-mantle text-[#cdd6f4]">
		<div class="animate__animated animate__fadeInLeftBig text-text">
			<div class="telegram transform hover:text-white">
				<a href="https://t.me/invitedtohell">telegram</a>
			</div>

			<div class="github transform hover:text-white">
				<a href="https://github.com/invitedtohell">github</a>
			</div>

			<div class="twitter transform hover:text-white">
				<a href="https://twitter.com/InvitedToHell">twitter</a>
			</div>

			<div class="email transform hover:text-white">
				<a href="emailto:info@helly.wtf">email</a>
			</div>

			<div class="planetmc transform hover:text-white">
				<a href="https://www.planetminecraft.com/member/invitedtoheaven/">planetmc</a>
			</div>
		</div>
	</div>
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
							<!-- User Information -->
							<div class="text-left px-2 flex flex-col content-center justify-center">
								<h2 class="text-3xl bold">
									<!-- {$presence.discord_user.username}-->
									InvitedToHell
								</h2>
								<h2 class="text-2xl" style="color: {borderColor}">
									{$presence.discord_status}
								</h2>
							</div>
						</div>

						<!-- User Activity -->
						{#if $presence.activities.length > 1}
							<div class="px-6 py-4">
								<p class="bold text-purple mb-2 text-2xl text-left">
									{$presence.activities[1].name}
								</p>

								<div class="flex row-auto items-center">
									{#if $presence.activities[1].assets.large_image}
										<img
											class="rounded-lg"
											src={largeActivityImage}
											alt=""
											width="70"
											height="70"
										/>
									{/if}
									<!-- User Activity Image -->
									{#if $presence.activities[1].assets}
										<div class="flex-col flex-auto pl-5 rounded-lg">
											{#if $presence.activities[1].state}
												<p class="regular text-text text-lg text-left content-center">
													{$presence.activities[1].state}
												</p>
											{/if}
											{#if $presence.activities[1].details}
												<p class="regular text-text text-lg text-left content-center">
													{$presence.activities[1].details}
												</p>
											{/if}
											{#if $presence.activities[1].timestamps && active1 === 'other'}
												<p class="regular text-text text-sm text-left content-center">
													{time}
												</p>
											{/if}
										</div>
									{:else}
										<div class="flex-col flex-auto">
											{#if $presence.activities[1].state}
												<p class="regular text-text text-lg text-left content-center">
													{$presence.activities[1].state}
												</p>
											{/if}
											{#if $presence.activities[1].details}
												<p class="regular text-text text-lg text-left content-center">
													{$presence.activities[1].details}
												</p>
											{/if}
											{#if $presence.activities[1].timestamps && active1 === 'other'}
												<p class="regular text-text text-sm text-left content-center">
													{time}
												</p>
											{/if}
										</div>
									{/if}
								</div>
							</div>
						{:else if $presence.activities.length === 1 && $presence.discord_status !== 'offline'}
							<p class="regular pt-5 text-text text-xl text-left content-center px-6">
								Currently doing nothing.
							</p>
						{/if}

						{#if $presence.activities.length > 2}
							<div class="px-6 py-4">
								<p class="bold text-purple mb-2 text-2xl text-left">
									{$presence.activities[2].name}
								</p>
								<div class="flex row-auto items-center">
									{#if $presence.activities[2].assets.large_image}
										<img
											class="rounded-lg"
											src={largeActivityImage2}
											alt=""
											width="70"
											height="70"
										/>
									{/if}
									<!-- User Activity Image -->
									{#if $presence.activities[2].assets}
										<div class="flex-col flex-auto pl-5">
											{#if $presence.activities[2].state}
												<p class="regular text-text text-lg text-left content-center">
													{$presence.activities[2].state}
												</p>
											{/if}
											{#if $presence.activities[2].details}
												<p class="regular text-text text-lg text-left content-center">
													{$presence.activities[2].details}
												</p>
											{/if}
											{#if $presence.activities[2].timestamps && active2 === 'other'}
												<p class="regular text-text text-sm text-left content-center">
													{time}
												</p>
											{/if}
										</div>
									{:else}
										<div class="flex-col flex-auto">
											{#if $presence.activities[2].state}
												<p class="text-text text-lg text-left content-center">
													{$presence.activities[2].state}
												</p>
											{/if}
											{#if $presence.activities[2].details}
												<p class="text-text text-lg text-left content-center">
													{$presence.activities[2].details}
												</p>
											{/if}
											{#if $presence.activities[2].timestamps && active2 === 'other'}
												<p class="text-text text-sm text-left content-center">
													{time}
												</p>
											{/if}
										</div>
									{/if}
								</div>
							</div>
						{/if}
					</div>
				{/if}
			</div>
		</div>
	</div>
</body>
