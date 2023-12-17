<script lang="ts">
  import { onMount } from 'svelte';
	import { useLanyard } from 'svelte-lanyard';

	let data: any;
	onMount(() => {
		data = useLanyard('469514043053309952');
	});
</script>

<body class="bg-base text-text min-h-screen">
		<div class="h-screen heading flex items-center justify-center">
            <h1 class="text-white animate__animated animate__fadeInLeftBig transform">Helly</h1>
			<section id="section01" class="demo text-text">
				<a href="#section02"><span></span>
			  </section>
        </div>
		<div  class="h-screen flex text items-center justify-center bg-mantle text-[#cdd6f4]">
			<div class="animate__animated animate__fadeInLeftBig">
				<div class="telegram transform">
					<a href="https://t.me/invitedtohell">telegram</a>
				</div>
				
				<div class="github transform">
					<a href="https://github.com/invitedtohell">github</a>
				</div>

				<div class="twitter transform">
					<a href="https://twitter.com/InvitedToHell">twitter</a>
				</div>

				<div class="email transform">
					<a href="emailto:info@helly.wtf">email</a>
				</div>

				<div class="planetmc transform">
					<a href="https://www.planetminecraft.com/member/invitedtoheaven/">planetmc</a>
				</div>
			</div>
        </div>
		<div  class="h-screen flex text items-center justify-center bg-mantle">
			<div class="animate__animated animate__fadeInLeftBig items-center">
				{#if $data}
				<div class="flex items-center">
					<img class="rounded-full h-20 w-20" src="https://cdn.discordapp.com/avatars/{$data.discord_user.id}/{$data.discord_user.avatar}.png" alt="{$data.discord_user.username}#{$data.discord_user.discriminator}" />
					<p class="text-purple pl-5">{$data.discord_user.username}</p>
				</div>
				{:else}
				<p>Waiting to fetch Discord data...</p>
				{/if}
			</div>
				{#if $data.discord_status === 'online'}
				<div class="animate__animated animate__fadeInLeftBig items-center">
					<p class="text-purple">{$data.discord_user.username} is online</p>
				</div>
				{:else if $data.discord_status === 'offline'}
				<div class="animate__animated animate__fadeInLeftBig items-center">
					<p class="text-purple">{$data.discord_user.username} is offline</p>
				</div>
				{:else if $data.discord_status === 'dnd'}
				<div class="animate__animated animate__fadeInLeftBig items-center">
					<p class="text-purple">{$data.discord_user.username} is dnd</p>
				</div>
				{:else if $data.discord_status === 'idle'}
				<div class="animate__animated animate__fadeInLeftBig items-center">
					<p class="text-purple">{$data.discord_user.username} is idle</p>
				</div>
				{:else}
				<p>?</p>
				{/if}

        </div>
	</body>


