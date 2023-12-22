<script lang="ts">
  import { useLanyard } from "$lib/lanyard";

  const presence = useLanyard({ method: "rest", id: "469514043053309952" });
  let borderColor = "#747f8d";
  let largeActivityImage: string;
  function updateBorderColor(prsnce: any) {
    if ($presence !== undefined) {
      if ($presence.discord_status === "streaming") {
        borderColor = "#593695";
      } else if ($presence.discord_status === "idle") {
        borderColor = "#faa61a";
      } else if ($presence.discord_status === "dnd") {
        borderColor = "#f04747";
      } else if ($presence.discord_status === "online") {
        borderColor = "#43b581";
      } else if ($presence.discord_status === "offline") {
        borderColor = "#747f8d";
      } else {
        borderColor = "#747f8d";
      }
    }
  }

  function updateActivityImage(prsnce: any) {
    if ($presence !== undefined) {
      if ($presence.activities.length > 1 && $presence.activities[1].assets) {
        const largeImage = $presence.activities[1].assets.large_image;
        largeActivityImage =
          "https://" + largeImage.substring(largeImage.indexOf("https/") + 6);
      }
    }
  }


  
  function timeStamps() {
    if ($presence !== undefined) {
      if ($presence.activities.length > 1 && $presence.activities[1].timestamps) {
        const start = $presence.activities[1].timestamps.start;
        console.log(JSON.stringify($presence.activities[1].timestamps));
        const elapsed = Math.floor(Date.now() / 1000) - start / 1000;
        let hours: any = Math.floor(elapsed / 3600);
        if (hours < 10) hours = '0' + hours;
        let minutes: any = Math.floor((elapsed - hours * 3600) / 60);
        if (minutes < 10) minutes = '0' + minutes;
        let seconds: any = Math.floor(elapsed - hours * 3600 - minutes * 60);
        if (seconds < 10) seconds = '0' + seconds;
        if (hours === '00') {
          return "for " + minutes + "min";
        }
        else {
        return "for " + hours + "h " + minutes + "min";
      }
      }
    }
  }
  $: updateActivityImage($presence);
  $: updateBorderColor($presence);
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
  <div
    class="h-screen flex text items-center justify-center bg-mantle text-[#cdd6f4]"
  >
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
        <a href="https://www.planetminecraft.com/member/invitedtoheaven/"
          >planetmc</a
        >
      </div>
    </div>
  </div>
  <div class="h-screen flex text items-center justify-center bg-mantle">
    <div class="animate__animated animate__fadeInLeftBig items-center">
      <div
        class="animate__animated animate__fadeInLeftBig items-center w-max h-max"
      >
        {#if $presence}
          <div
            class="bg-crust text-text md rounded-md overflow-hidden lg:w-[30vw] pt-5 pb-5"
          >
            <!-- User Avatar -->
            <div class="flex flex-auto justify-center items-center">
              <div class="p-1">
                <img
                  class="!w-20 !h-20 rounded-full mx-auto border-solid border-4"
                  style="border-color: {borderColor};"
                  src="https://cdn.discordapp.com/avatars/{$presence
                    .discord_user.id}/{$presence.discord_user.avatar}"
                />
              </div>
              <!-- User Information -->
              <div class="text-center px-6 py-4">
                <h2 class="text-3xl mb-2">
                  {$presence.discord_user.username}
                </h2>
              </div>
            </div>
            <!-- User Activity -->
            {#if $presence.activities.length > 1}

            <div class="px-6 py-4">
              <p
                class="font-semibold text-purple mb-2 text-2xl text-center content-center"
              >
                {$presence.activities[1].name}
              </p>
              <div class="flex row-auto justify-center items-center">
                {#if $presence.activities[1].assets}
                <img
                  class="p-0.5"
                  src={largeActivityImage}
                  alt=""
                  width="60"
                  height="60"
                />
                {/if}
                <!-- User Activity Image -->
                <div class="flex-col flex">
                  {#if $presence.activities[1].state}
                  <p class="text-text text-xl text-center content-center">
                    {$presence.activities[1].state}
                  </p>
                  {/if}
                  {#if $presence.activities[1].details}
                  <p class="text-text text-xl text-center content-center">
                    {$presence.activities[1].details}
                  </p>
                  {/if}
                  {#if $presence.activities[1].timestamps}
                  <p class="text-text text-xl text-center content-center">
                    {timeStamps()}
                  </p>
                  {/if}
                </div>
                  
              </div>
            </div>
            {:else}
                  <p class="pt-5 text-text text-xl text-center content-center">Currently doing nothing!</p> 
                  {/if}
          </div>
        {/if}
      </div>
    </div>
  </div></body
>
