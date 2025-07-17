<script lang="ts">
  import { browser } from '$app/environment';

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
  approved: boolean;
  role: string;
  password: string;
  profilePic?: string;
  qna: {
    q1: string;
    q2: string;
    q3: string;
    q4: string;
  };
};


  let users: User[] = [];

  if (browser) {
    users = JSON.parse(localStorage.getItem('users_parasite') || '[]');
  }

  function approve(userEmail: string) {
    users = users.map((u: User) =>
      u.email === userEmail ? { ...u, approved: true } : u
    );
    localStorage.setItem('users_parasite', JSON.stringify(users));
  }

  function remove(userEmail: string) {
    users = users.filter((u: User) => u.email !== userEmail);
    localStorage.setItem('users_parasite', JSON.stringify(users));
  }
</script>

<main class="admin-container">
  <h2>ผู้สมัครประเภท Parasite</h2>

  {#if users.length === 0}
    <p style="text-align: center; margin-top: 2rem;">ยังไม่มีผู้สมัคร</p>
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
          <p><strong>สถานะ:</strong> {user.approved ? '✅ อนุมัติแล้ว' : '⏳ รอตรวจสอบ'}</p>
        </div>

        <div class="qna">
          <p><strong>1. เป้าหมายหลักของคุณในการใช้แพลตฟอร์มนี้คืออะไร?</strong> {user.qna.q1}</p>
          <p><strong>2. ระดับความรู้ในสาขาที่คุณเลือกอยู่ในระดับใด?</strong> {user.qna.q2}</p>
          <p><strong>3. โปรดเขียนเหตุผลสั้น ๆ ว่าทำไมคุณถึงสนใจใช้แพลตฟอร์มนี้</strong> {user.qna.q3}</p>
          <p><strong>4. รู้จักแพลตฟอร์มนี้จาก…</strong> {user.qna.q4}</p>
        </div>

        <div class="actions">
          {#if !user.approved}
            <button on:click={() => approve(user.email)}>✅ อนุมัติ</button>
          {/if}
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

  .card .info, .card .qna {
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

  .actions button:not(.danger) {
    background: #34d399;
    color: white;
  }
</style>
