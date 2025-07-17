<script lang="ts">
  import { goto } from '$app/navigation';

  let email = '';
  let password = '';
  let error = '';
  let showPassword = false;


  function login() {
    if (email === 'admin' && password === 'admin') {
      const adminUser = { email: 'admin', role: 'admin', password: 'admin' };
      localStorage.setItem('currentUser', JSON.stringify(adminUser));
      goto('/admin');
      return;
    }

    const hosts = JSON.parse(localStorage.getItem('users_host') || '[]');
    const parasites = JSON.parse(localStorage.getItem('users_parasite') || '[]');
    const allUsers = [...hosts, ...parasites];

    const found = allUsers.find((u: any) => u.email === email && u.password === password);

    if (found) {
      if (!found.approved) {
        error = 'บัญชีนี้ยังไม่ได้รับการอนุมัติ กรุณารอการตรวจสอบจากแอดมิน';
        return;
      }

      localStorage.setItem('currentUser', JSON.stringify(found));
      localStorage.setItem('loggedInUser', JSON.stringify(found));

      if (found.role === 'host') goto('/host/home');
      else if (found.role === 'parasite') goto('/parasite/home');
      else goto('/role');
    } else {
      error = 'อีเมลหรือรหัสผ่านไม่ถูกต้อง';
    }
  }

  function goToRegister() {
    goto('/role');
  }
</script>

<div class="background">
  <div class="login-container">
    <h2>เข้าสู่ระบบ</h2>

    {#if error}
      <p class="error">{error}</p>
    {/if}

    <input type="email" placeholder="Email" bind:value={email} />
    <div class="input-group">
    <input
  type={showPassword ? 'text' : 'password'}
  placeholder="Password"
  bind:value={password}
/>

<div class="toggle-container">
  <label class="toggle-password">
    <input type="checkbox" bind:checked={showPassword} />
    แสดงรหัสผ่าน
  </label>
</div>





  </div>

    <button on:click={login}>Login</button>

    <p class="small">
      ยังไม่มีบัญชี? <a on:click={goToRegister}>สมัครสมาชิก</a>
    </p>
  </div>
</div>

<style>
  .background {
    height: 100vh;
    background: linear-gradient(to bottom right, #e0f7fa, #fce4ec);
    display: flex;
    justify-content: center;
    align-items: center;
    animation: fadeIn 0.6s ease;
  }

  .login-container {
    width: 100%;
    max-width: 420px;
    padding: 2rem;
    border-radius: 20px;
    background: white;
    box-shadow: 0 8px 20px rgba(0, 0, 0, 0.08);
    text-align: center;
    transform: scale(0.95);
    animation: popUp 0.4s ease forwards;
  }

  h2 {
    margin-bottom: 1rem;
    color: #333;
    font-weight: 600;
    font-family: 'Segoe UI', sans-serif;
  }

  input {
  width: 100%;
  padding: 0.8rem;
  margin-bottom: 1rem;
  border: 1.8px solid #ccc;
  border-radius: 12px;
  font-size: 1rem;
  transition: all 0.2s ease;
  box-sizing: border-box;
  outline: none;
}

input:focus {
  border-color: #64d1c3;
  box-shadow: 0 0 0 2px rgba(100, 209, 195, 0.3);
}


  button {
    background: #64d1c3;
    color: white;
    border: none;
    padding: 0.8rem 1.5rem;
    border-radius: 14px;
    font-size: 1rem;
    font-weight: bold;
    cursor: pointer;
    width: 100%;
    transition: transform 0.2s ease, background 0.3s ease;
  }

  button:hover {
    background: #4cbeb1;
    transform: scale(1.02);
  }

  .error {
    color: #f44336;
    font-size: 0.9rem;
    margin-bottom: 1rem;
  }

  .small {
    margin-top: 1.5rem;
    font-size: 0.9rem;
    color: #666;
  }

  .small a {
    color: #64d1c3;
    cursor: pointer;
    text-decoration: underline;
  }


.inline-toggle {
  font-size: 0.9rem;
  color: #444;
  display: flex;
  align-items: center;
  gap: 0.4rem;
}
.toggle-container {
  width: 100%;
  display: flex;
  justify-content: flex-start;
  margin-top: 0.5rem;
  margin-bottom: 1rem;
  padding-left: 2px;
}
.toggle-password {
  display: flex;
  align-items: center;
  font-size: 0.9rem;
  color: #444;
  margin-top: -0.5rem;
  margin-bottom: 1rem;
  gap: 0.4rem;
  justify-content: flex-start; /* ขยับปุ่มไปชิดซ้าย */
  padding-left: 2px; /* เพิ่มระยะห่างจากขอบ */
  white-space: nowrap; /* ป้องกันข้อความขึ้นบรรทัดใหม่ */
}





  @keyframes popUp {
    to {
      transform: scale(1);
    }
  }

  @keyframes fadeIn {
    from {
      opacity: 0;
      transform: translateY(15px);
    }
    to {
      opacity: 1;
      transform: translateY(0);
    }
  }
</style>
