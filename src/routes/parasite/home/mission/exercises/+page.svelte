<script lang="ts">
  import { goto } from '$app/navigation';
  import NavBar from '$lib/components/NavBar.svelte';

  function goBack() {
    history.back();
  }

  function goToSubject(subject: string) {
    goto(`/parasite/home/mission/practice/${subject}`);
  }

  const subjects = [
    { name: 'ฟิสิกส์', icon: 'physics', code: 'physics' },
    { name: 'เคมี', icon: 'chemistry', code: 'chemistry' },
    { name: 'ชีวะ', icon: 'biology', code: 'biology' },
    { name: 'คณิต', icon: 'math', code: 'math' },
    { name: 'ไทย', icon: 'thai', code: 'thai' },
    { name: 'อังกฤษ', icon: 'english', code: 'english' }
  ];
</script>

<NavBar currentPage="home" />

<main class="container">
  <div class="back-btn" on:click={goBack}>
    <img src="/icons/back.png" alt="Back" />
    <span>Back</span>
  </div>

  <div class="header">
    <h2>Practice Exercises</h2>
    <p class="subtitle">เลือกหมวดวิชาที่คุณต้องการฝึกฝน</p>
  </div>

  <div class="grid">
    {#each subjects as subject}
      <button class="card" on:click={() => goToSubject(subject.code)}>
        <img src={`/icons/${subject.icon}.png`} alt="" />
        <span>{subject.name}</span>
      </button>
    {/each}
  </div>
</main>



<style>
  .container {
  padding: 6rem 1.5rem 4rem; /* <--- เปลี่ยนจาก 2rem เป็น 6rem */
  background: linear-gradient(135deg, #e0f7fa, #f1f8ff);
  min-height: 100vh;
  font-family: 'Noto Sans', sans-serif;
}


  .header {
    text-align: center;
    margin-bottom: 2rem;
    position: relative;
  }

  .header h2 {
    font-size: 1.8rem;
    color: #333;
    margin: 0.5rem 0;
  }

  .subtitle {
    font-size: 0.95rem;
    color: #666;
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


  .grid {
    display: grid;
    grid-template-columns: repeat(3, 1fr);
    gap: 1.2rem;
  }

  .card {
  all: unset; /* ล้างสไตล์ปุ่ม */
  background: white;
  border-radius: 16px;
  padding: 1rem;
  text-align: center;
  cursor: pointer;
  transition: all 0.2s ease;
  box-shadow: 0 4px 12px rgba(0, 0, 0, 0.1);
  display: flex;
  flex-direction: column;
  align-items: center;
}

.card:hover {
  transform: translateY(-5px);
  background: #f0faff;
}


  .card img {
    width: 48px;
    height: 48px;
    margin-bottom: 0.5rem;
  }

  .card span {
    display: block;
    font-weight: 600;
    color: #333;
    font-size: 1rem;
  }

  @media (max-width: 500px) {
    .grid {
      grid-template-columns: repeat(2, 1fr);
    }
  }
  
</style>
