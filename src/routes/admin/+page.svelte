<script lang="ts">
  import { onMount } from 'svelte';
  import { goto } from '$app/navigation';

  function goTo(path: string) {
    goto(`/admin/${path}`);
  }

  function logout() {
    localStorage.removeItem('currentUser');
    goto('/login');
  }

  onMount(() => {
    const currentUser = localStorage.getItem('currentUser');
    const parsed = currentUser && JSON.parse(currentUser);

    if (!parsed || parsed.email !== 'admin' || parsed.password !== 'admin') {
      goto('/login');
    }
  });
</script>

<main class="admin-container">
  <div class="header">
    <h1>Admin Dashboard</h1>
    <button class="logout-btn" on:click={logout}>Logout</button>
  </div>

  <p>เลือกประเภทผู้ใช้ที่คุณต้องการจัดการ</p>

  <div class="card-grid">
    <div class="card" on:click={() => goTo('host')}>
      <h2>👤 Host</h2>
      <p>ตรวจสอบและอนุมัติผู้สมัคร Host</p>
    </div>

    <div class="card" on:click={() => goTo('parasite')}>
      <h2>🧠 Challenger</h2>
      <p>ตรวจสอบผู้สมัคร Parasite</p>
    </div>
  </div>
</main>

<style>
  .admin-container {
    max-width: 600px;
    margin: 4rem auto;
    padding: 2rem;
    text-align: center;
    font-family: 'Segoe UI', sans-serif;
    position: relative;
  }

  .header {
    display: flex;
    justify-content: space-between;
    align-items: center;
    margin-bottom: 1rem;
  }

  h1 {
    font-size: 2rem;
  }

  .logout-btn {
    padding: 0.5rem 1rem;
    font-size: 0.9rem;
    background: #f44336;
    color: white;
    border: none;
    border-radius: 10px;
    cursor: pointer;
    transition: background 0.3s;
  }

  .logout-btn:hover {
    background: #d32f2f;
  }

  p {
    color: #666;
    margin-bottom: 2rem;
  }

  .card-grid {
    display: grid;
    grid-template-columns: 1fr;
    gap: 1.5rem;
  }

  .card {
    background: #f7f7f7;
    padding: 1.5rem;
    border-radius: 14px;
    cursor: pointer;
    transition: all 0.2s ease;
    box-shadow: 0 4px 12px rgba(0,0,0,0.05);
  }

  .card:hover {
    background: #e9e9ff;
    transform: translateY(-2px);
  }

  .card h2 {
    margin-bottom: 0.5rem;
  }

  .card p {
    font-size: 0.95rem;
  }
</style>
