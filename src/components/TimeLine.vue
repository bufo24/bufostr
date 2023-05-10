<script setup lang="ts">
import { relayInit } from "nostr-tools";
import { computed, onMounted, ref } from "vue";
import Note from "./Note.vue";

const events = ref<Array<any>>([]);
const sortedEvents = computed(() =>
  events.value.sort((a, b) => b.created_at - a.created_at)
);

onMounted(async () => {
  const relay = relayInit("wss://relay.nostr.vet");

  await relay.connect();

  let sub = relay.sub([
    {
      authors: [
        "6754f561a7165c4e2e109b49867d88bd5695e90dbb67a95cb4132e3a0f16f679",
      ],
      kinds: [1],
    },
  ]);
  sub.on("event", (event) => {
    events.value.push(event);
  });
  sub.on("eose", () => {
    sub.unsub();
  });
});
</script>
<template>
  <div v-for="event of sortedEvents">
    <Note :pubkey="event.pubkey" :text="event.content" />
  </div>
</template>
