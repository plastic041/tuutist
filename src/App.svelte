<script lang="ts">
  import { onMount } from "svelte";

  const isNotificationPermitted = Notification.permission === "granted";

  onMount(() => {
    Notification.requestPermission();
  });

  let inputElement: HTMLInputElement;

  const onSubmit = (e: { preventDefault: () => void }) => {
    e.preventDefault();
    // notification using the browser API
    const notifiString = inputElement.value;
    if (isNotificationPermitted) {
      setTimeout(() => {
        new Notification(notifiString);
      }, 1000);
    } else {
      alert("Notification not permitted");
    }
  };
</script>

<div class="container mx-auto h-screen py-4">
  <div class="mx-auto flex w-96 flex-col gap-2">
    <header class="font-header text-6xl">Tuutist</header>
    <main>
      <form
        class="flex flex-col gap-4 rounded bg-white p-4 shadow"
        on:submit={onSubmit}
      >
        <label class="flex flex-col gap-1">
          <span class="text text-gray-700">내용</span>
          <input
            class="rounded border p-2 text-2xl"
            type="text"
            bind:this={inputElement}
          />
        </label>
        <button
          type="submit"
          disabled={!isNotificationPermitted}
          class="rounded bg-blue-500 py-2 px-4 font-bold text-white hover:bg-blue-700 active:bg-blue-800 disabled:opacity-25"
          >알림</button
        >
        {#if !isNotificationPermitted}
          <span class="text-red-500">알림을 허용해주세요.</span>
        {/if}
      </form>
    </main>
  </div>
</div>

<style>
  :global(body) {
    @apply bg-teal-100;
  }
</style>
