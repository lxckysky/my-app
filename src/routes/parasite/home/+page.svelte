<script lang="ts">
  import { onMount } from 'svelte';
  import { goto } from '$app/navigation';
  import NavBar from '$lib/components/NavBar.svelte';

  let username = '';

  onMount(() => {
    const raw = localStorage.getItem('currentUser');
    if (!raw) {
      goto('/login');
      return;
    }

    const user = JSON.parse(raw);
    if (user.role !== 'parasite' || !user.approved) {
      goto('/login');
      return;
    }

    username = user.name || 'Anonymous';
  });

  function goTo(page: string) {
    goto(`/parasite/${page}`);
  }

  function goToResource(path: string) {
    goto(`/parasite/home/${path}`);
  }

  function goToMission(path: string) {
    goto(`/parasite/home/mission/${path}`);
  }
</script>


<NavBar currentPage="home" />

<div class="container">
  <main>
    <h2>Challenger Dashboard</h2>
    <p class="subtitle">Explore missions and grow</p>

    <div class="top-box">
      <p class="username">👤 {username}</p>
      <p class="point">🔥 10 / 1000 XP</p>
      <div class="progress-bar"><div class="progress" style="width: 1%"></div></div>
    </div>

    <div class="card">
      <h3>🎯 Missions</h3>
      <div class="btn" on:click={() => goToMission('exercises')}>📝 Practice Exercises</div>
      <div class="btn" on:click={() => goToMission('mock')}>🧪 Mock Test</div>
      <div class="btn" on:click={() => goToMission('competition')}>🏆 Competitions</div>
      <div class="btn" on:click={() => goToMission('activity')}>🎒 Activity</div>
    </div>

    <div class="card">
      <h3>📚 Resources</h3>
      <div class="btn" on:click={() => goToResource('resource/lecture')}>📘 Lecture Notes</div>
      <div class="btn" on:click={() => goToResource('resource/tip')}>🎓 University Tips</div>
      <div class="btn" on:click={() => goToResource('resource/tip2')}>💼 Career Tips</div>
    </div>
  </main>
</div>

<style>
  :global(body) {
    font-family: 'Noto Sans', sans-serif;
    background: linear-gradient(to bottom right, #f2f6fc, #e0f7fa);
    margin: 0;
  }

  .container {
    padding: 5.5rem 1rem 2rem;
    min-height: 100vh;
    box-sizing: border-box;
  }

  main {
    max-width: 600px;
    margin: auto;
    background: white;
    border-radius: 24px;
    padding: 2rem;
    box-shadow: 0 8px 20px rgba(0, 0, 0, 0.06);
  }

  h2 {
    font-size: 1.8rem;
    font-weight: 700;
    margin-bottom: 0.5rem;
    color: #333;
    text-align: center;
  }

  .subtitle {
    font-size: 1rem;
    text-align: center;
    margin-bottom: 2rem;
    color: #666;
  }

  .top-box {
    background: #f9fbfd;
    border: 1px dashed #cbd5e1;
    border-radius: 16px;
    padding: 1rem;
    margin-bottom: 1.5rem;
  }

  .username {
    font-weight: 600;
    color: #444;
    margin-bottom: 0.3rem;
  }

  .point {
    font-weight: bold;
    color: #555;
    margin-bottom: 0.6rem;
  }

  .progress-bar {
    width: 100%;
    height: 10px;
    background: #e2e8f0;
    border-radius: 10px;
    overflow: hidden;
  }

  .progress {
    height: 100%;
    background: linear-gradient(to right, #f78ca0, #f9748f);
  }

  .card {
    background: #ffffff;
    padding: 1.2rem;
    border-radius: 18px;
    box-shadow: 0 4px 12px rgba(0, 0, 0, 0.04);
    margin-bottom: 1.5rem;
  }

  .card h3 {
    margin-bottom: 0.8rem;
    color: #333;
    font-size: 1.1rem;
  }

  .btn {
    background: linear-gradient(135deg, #8e44ec, #5e5ce6);
    color: white;
    padding: 0.6rem 0.9rem;
    margin-bottom: 0.6rem;
    border: none;
    border-radius: 10px;
    cursor: pointer;
    text-align: center;
    font-weight: 500;
    transition: 0.2s ease-in-out;
    font-size: 0.88rem;
    box-shadow: 0 3px 0 #4b3bbd;
  }

  .btn:hover {
    transform: translateY(-2px);
    filter: brightness(1.05);
  }
</style>
