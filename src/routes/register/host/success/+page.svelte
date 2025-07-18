<script lang="ts">
  import { goto } from '$app/navigation';
</script>

<main class="success-page">
  <h2>🎉 สมัครเรียบร้อยแล้ว!</h2>
  <p>กรุณารอการอนุมัติจากแอดมินก่อนเข้าสู่ระบบ</p>
  <button on:click={() => goto('/login')}>กลับไปหน้า Login</button>
</main>

<style>
  .success-page {
    text-align: center;
    max-width: 500px;
    margin: 5rem auto;
    font-family: 'Segoe UI', sans-serif;
    background: #fff;
    padding: 2rem;
    border-radius: 20px;
    box-shadow: 0 8px 20px rgba(0, 0, 0, 0.1);
  }

  h2 {
    color: #2e7d32;
    margin-bottom: 1rem;
  }

  p {
    font-size: 1.1rem;
    margin-bottom: 2rem;
  }

  button {
    padding: 0.8rem 1.5rem;
    background: #4bc0b0;
    border: none;
    color: white;
    font-weight: bold;
    border-radius: 10px;
    cursor: pointer;
    font-size: 1rem;
  }

  button:hover {
    background: #39998e;
  }
</style>
