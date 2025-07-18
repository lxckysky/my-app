<script lang="ts">
  export let selected = '';
  import { onMount, createEventDispatcher } from 'svelte';

  let users: { email: string; name: string }[] = [];
  const dispatch = createEventDispatcher();

  onMount(() => {
    const stored = localStorage.getItem('users');
    users = stored ? JSON.parse(stored) : [];

    if (!selected && users.length > 0) {
      selected = users[0].email;
      dispatch('change');
    }
  });

  function handleChange() {
    dispatch('change');
  }
</script>

<label for="chat-user">เลือกผู้ใช้ที่ต้องการแชทด้วย</label>
<select id="chat-user" bind:value={selected} on:change={handleChange}>
  {#each users as u}
    <option value={u.email}>{u.name}</option>
  {/each}
</select>

<style>
  label {
    font-weight: bold;
    margin: 1rem 0 0.5rem;
    display: block;
  }

  select {
    width: 100%;
    padding: 0.5rem;
    font-size: 1rem;
    border-radius: 8px;
    border: 1px solid #ccc;
  }
</style>
