<script>
  import { navigate } from "svelte-routing";


  function handleNavigation(event) {
    navigate("/", { replace: true });
  }

</script>

<div on:click={handleNavigation} class="cursor-pointer h-16 flex justify-center items-center">
  <p class="text-2xl text-teal-400 font-bold">Robin</p>
</div>
