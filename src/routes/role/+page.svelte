<script lang="ts">
  import { goto } from '$app/navigation';
  import { onMount } from 'svelte';

  let currentUser: { name?: string; email?: string; password?: string; role?: string } = {};
  let selectedRole: 'host' | 'parasite' | null = null;

  onMount(() => {
    const stored = localStorage.getItem('currentUser');
    if (!stored) {
      goto('/role');
      return;
    }
    currentUser = JSON.parse(stored);
  });

  function selectRole(role: 'host' | 'parasite') {
    selectedRole = role;
  }

  function confirmRole() {
    if (!selectedRole) return;
    currentUser.role = selectedRole;
    localStorage.setItem('currentUser', JSON.stringify(currentUser));
    localStorage.setItem('loggedInUser', JSON.stringify(currentUser));

    const users = JSON.parse(localStorage.getItem('users') || '[]');
    const updatedUsers = users.map((u: any) =>
      u.email === currentUser.email ? { ...u, role: selectedRole } : u
    );
    localStorage.setItem('users', JSON.stringify(updatedUsers));

    goto(`/register/${selectedRole}`);
  }
</script>

<div class="container">
  <div class="card">
    <h2 class="title">🎯 เลือกบทบาทของคุณ</h2>
    <p class="subtitle">สำหรับนักเรียน ม.ปลาย / นักศึกษามหาวิทยาลัย</p>

    <div class="roles">
      <div class="role host {selectedRole === 'host' ? 'selected' : ''}" on:click={() => selectRole('host')}>
        <div class="icon">🎓</div>
        <div class="label">Host</div>
        <p class="desc">แบ่งปันประสบการณ์ เทคนิค และแหล่งเรียนรู้</p>
      </div>

      <div class="role parasite {selectedRole === 'parasite' ? 'selected' : ''}" on:click={() => selectRole('parasite')}>
        <div class="icon">🚀</div>
        <div class="label">Challenger</div>
        <p class="desc">เรียนรู้ผ่านเควส สะสมประสบการณ์ สร้างตัวตน</p>
      </div>
    </div>

    <button class="confirm-btn" on:click={confirmRole} disabled={!selectedRole}>ยืนยันการเลือก</button>
  </div>
</div>

<style>
  :global(body) {
    margin: 0;
    font-family: 'Segoe UI', sans-serif;
    background: linear-gradient(135deg, #e1f5fe, #fce4ec);
    min-height: 100vh;
  }

  .container {
    display: flex;
    justify-content: center;
    align-items: center;
    padding: 2rem;
    height: 100vh;
  }

  .card {
    background: white;
    border-radius: 24px;
    padding: 2rem 1.5rem;
    box-shadow: 0 8px 30px rgba(0, 0, 0, 0.08);
    text-align: center;
    width: 100%;
    max-width: 440px;
  }

  .title {
    font-size: 1.8rem;
    font-weight: 600;
    margin-bottom: 0.5rem;
    color: #333;
  }

  .subtitle {
    font-size: 1rem;
    color: #7b1fa2;
    margin-bottom: 1.5rem;
  }

  .roles {
    display: grid;
    grid-template-columns: 1fr 1fr;
    gap: 1rem;
    margin-bottom: 1.5rem;
  }

  .role {
    padding: 1rem;
    border-radius: 16px;
    cursor: pointer;
    color: white;
    transition: transform 0.2s ease, outline 0.2s;
    box-shadow: 0 4px 12px rgba(0, 0, 0, 0.05);
  }

  .role:hover {
    transform: translateY(-3px);
  }

  .role.host {
    background: linear-gradient(to bottom right, #42a5f5, #4fc3f7);
  }

  .role.parasite {
    background: linear-gradient(to bottom right, #f06292, #f48fb1);
  }

  .role.selected {
    outline: 3px solid #64d1c3;
  }

  .icon {
    font-size: 2rem;
    margin-bottom: 0.3rem;
  }

  .label {
    font-size: 1.2rem;
    font-weight: bold;
    margin-bottom: 0.2rem;
  }

  .desc {
    font-size: 0.85rem;
    opacity: 0.95;
  }

  .confirm-btn {
    background: #64d1c3;
    color: white;
    padding: 0.7rem 1.5rem;
    border: none;
    border-radius: 12px;
    font-weight: bold;
    cursor: pointer;
    font-size: 1rem;
    width: 100%;
  }

  .confirm-btn:disabled {
    background: #ccc;
    cursor: not-allowed;
  }

  @media (max-width: 500px) {
    .roles {
      grid-template-columns: 1fr;
    }
  }
</style>
