<script lang="ts">
  import { goto } from '$app/navigation';
  import { onMount } from 'svelte';

  export let currentPage: string;
  let role = '';

  onMount(() => {
    const raw = localStorage.getItem('currentUser');
    if (raw) {
      const user = JSON.parse(raw);
      role = user.role;
    }
  });

  function goTo(page: string) {
    if (role) {
      goto(`/${role}/${page}`);
    }
  }
</script>

<style>
  .top-nav {
    display: flex;
    justify-content: space-around;
    align-items: center;
    background: #ffffffee;
    padding: 0.5rem 0;
    box-shadow: 0 2px 6px rgba(0, 0, 0, 0.05);
    position: fixed;
    top: 0;
    width: 100%;
    z-index: 1000;
    border-bottom: 1.5px solid #e0e6ed;
    backdrop-filter: blur(8px);
  }

  .nav-btn {
    flex: 1;
    background: none;
    border: none;
    font-size: 0.7rem;
    font-weight: 500;
    color: #666;
    text-align: center;
    display: flex;
    flex-direction: column;
    align-items: center;
    gap: 0.2rem;
    padding: 0.3rem 0;
    cursor: pointer;
    transition: color 0.2s ease;
  }

  .nav-btn.active {
    color: #5e5ce6;
    font-weight: 700;
  }

  .nav-icon {
    width: 20px;
    height: 20px;
    max-width: 100%;
    object-fit: contain;
  }

  @media (max-width: 500px) {
    .nav-icon {
      width: 18px;
      height: 18px;
    }

    .nav-btn span {
      font-size: 0.65rem;
    }
  }
</style>

<div class="top-nav">
  <button class:active={currentPage === 'home'} class="nav-btn" on:click={() => goTo('home')}>
    <img src="/icons/home.png" class="nav-icon" alt="Home" />
    <span>Home</span>
  </button>

  <button class:active={currentPage === 'chat'} class="nav-btn" on:click={() => goTo('chat')}>
    <img src="/icons/chat.png" class="nav-icon" alt="Chat" />
    <span>Chat</span>
  </button>

  <button class:active={currentPage === 'profile'} class="nav-btn" on:click={() => goTo('profile')}>
    <img src="/icons/profile.png" class="nav-icon" alt="Profile" />
    <span>Profile</span>
  </button>
</div>
