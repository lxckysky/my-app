<script lang="ts">
  import { browser } from '$app/environment';
  import { goto } from '$app/navigation';

function goBackToAdmin() {
  goto('/admin');
}

  type User = {
    name: string;
    username: string;
    email: string;
    tel: string;
    birthday: string;
    gender: string;
    university: string;
    major: string;
    year: string;
    role: string;
    password: string;
    profilePic?: string;
  };

  let users: User[] = [];

  if (browser) {
    users = JSON.parse(localStorage.getItem('users_parasite') || '[]');
  }

  function remove(userEmail: string) {
    users = users.filter((u: User) => u.email !== userEmail);
    localStorage.setItem('users_parasite', JSON.stringify(users));
  }
</script>

<main class="admin-container">

  <div class="back-btn" on:click={goBackToAdmin}>
  <img src="/icons/back.png" alt="Back" />
  <span>Back</span>
</div>

  
  <h2>ผู้ใช้งานประเภท Parasite</h2>

  {#if users.length === 0}
    <p style="text-align: center; margin-top: 2rem;">ยังไม่มีผู้ใช้</p>
  {:else}
    {#each users.filter(u => u.role === 'parasite') as user (user.email)}
      <div class="card">
        {#if user.profilePic}
          <img src={user.profilePic} alt="รูปโปรไฟล์" class="profile-pic" />
        {/if}

        <div class="info">
          <p><strong>ชื่อ:</strong> {user.name}</p>
          <p><strong>Username:</strong> {user.username}</p>
          <p><strong>อีเมล:</strong> {user.email}</p>
          <p><strong>เบอร์โทร:</strong> {user.tel}</p>
          <p><strong>วันเกิด:</strong> {user.birthday}</p>
          <p><strong>เพศ:</strong> {user.gender}</p>
          <p><strong>โรงเรียน/มหาวิทยาลัย:</strong> {user.university}</p>
          <p><strong>สายการเรียน:</strong> {user.major}</p>
          <p><strong>ระดับชั้น:</strong> {user.year}</p>
          <p><strong>รหัสผ่าน:</strong> {user.password}</p>
        </div>

        <div class="actions">
          <button class="danger" on:click={() => remove(user.email)}>🗑️ ลบ</button>
        </div>
      </div>
    {/each}
  {/if}
</main>

<style>
  .admin-container {
    max-width: 860px;
    margin: 4rem auto;
    font-family: 'Segoe UI', sans-serif;
  }

  h2 {
    text-align: center;
    margin-bottom: 2rem;
  }

  .card {
    background: #f9f9f9;
    padding: 1.5rem;
    border-radius: 14px;
    margin-bottom: 2rem;
    box-shadow: 0 4px 10px rgba(0,0,0,0.1);
  }

  .card .info {
    margin-bottom: 1rem;
  }

  .card p {
    margin: 0.3rem 0;
  }

  .profile-pic {
    display: block;
    width: 100px;
    height: 100px;
    object-fit: cover;
    margin: 0 auto 1rem;
    border-radius: 50%;
    border: 2px solid #ccc;
  }

  .actions button {
    margin-right: 0.5rem;
    padding: 0.6rem 1.2rem;
    border: none;
    border-radius: 10px;
    font-weight: bold;
    cursor: pointer;
    font-size: 0.95rem;
  }

  .actions .danger {
    background: #ef4444;
    color: white;
  }
  .back-btn {
  display: flex;
  align-items: center;
  gap: 0.4rem;
  background: #f4f6fc;
  border: 1px solid #cbd5e1;
  border-radius: 999px;
  padding: 0.4rem 0.8rem;
  width: fit-content;
  cursor: pointer;
  margin-bottom: 1.2rem;
  transition: background 0.3s ease, box-shadow 0.2s ease;
  font-weight: 500;
  font-size: 0.85rem;
  color: #444;
  box-shadow: 0 2px 4px rgba(0, 0, 0, 0.04);
}

.back-btn img {
  width: 18px;
  height: 18px;
}

.back-btn:hover {
  background: #e6ecf5;
  box-shadow: 0 3px 6px rgba(0, 0, 0, 0.08);
}

</style>
