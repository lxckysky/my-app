<script lang="ts">
  import { goto } from '$app/navigation';
  import NavBar from '$lib/components/NavBar.svelte';

  const mockTests = [
    {
    title: "TCASter MOCK Exam",
    url: "https://tcaster.net/mock-m-6/",
    desc: "สนามสอบจำลอง TCAS ทั้ง TGAT/TPAT และ A-Level ทั้งแบบ Paper-based และ Computer-based"
  },
  {
    title: "MonkeyEveryday Mock Exam",
    url: "https://monkeyeveryday.com/mock-exam",
    desc: "Mock Exam สำหรับสอบเข้ามหา'ลัย พร้อมระบบจับเวลาตามจริง และวิเคราะห์ผลคะแนน"
  },
  {
    title: "IDP IELTS Thailand",
    url: "https://ielts.idp.com/thailand/prepare/ielts-sample-test",
    desc: "Free online IELTS sample tests และ In-House Mock Exam (สอบที่ศูนย์สอบ)"
  },
  {
    title: "CanPacific College - TOEIC Practice",
    url: "https://www.canpacificcollege.com/toeictest/reading5/toeic-directions5.php",
    desc: "On-line TOEIC Practice Test"
  },
  {
    title: "SmartMathPro Unseen Mock Test (TGAT TPAT)",
    url: "https://www.smartmathpro.com/smpmocktest/",
    desc: "Unseen Mock Test สำหรับ TGAT TPAT 1, 3, 5 พร้อมเฉลยละเอียด"
  }
  ];

  function goBack() {
    goto('/parasite/home');
  }
</script>

<NavBar currentPage="mock" />

<main class="mock-container">
  <button class="back-btn" on:click={goBack}>
    <img src="/icons/back.png" alt="back" />
    <span>Back</span>
  </button>

  <div class="header">
    <h2>Mock Test Center</h2>
    <p class="subtitle">รวมแหล่งข้อสอบเสมือนจริง เพื่อเตรียมตัวให้พร้อม!</p>
  </div>

  <div class="mock-list">
    {#each mockTests as test}
      <div class="mock-card" on:click={() => window.open(test.url, '_blank')}>
        <h3>{test.title}</h3>
        <p>{test.desc}</p>
      </div>
    {/each}
  </div>
</main>

<style>
  .mock-container {
    padding: 6rem 1.5rem 3rem;
    background: linear-gradient(135deg, #e0f7fa, #f9f9ff);
    min-height: 100vh;
    font-family: 'Noto Sans', sans-serif;
    position: relative;
  }

  .back-btn {
    position: absolute;
    top: 5.5rem;
    left: 1.5rem;
    display: inline-flex;
    align-items: center;
    gap: 0.5rem;
    background: #f4f6fc;
    border: 1px solid #cbd5e1;
    border-radius: 999px;
    padding: 0.5rem 1rem;
    font-size: 0.9rem;
    color: #444;
    cursor: pointer;
    box-shadow: 0 2px 4px rgba(0,0,0,0.05);
    z-index: 10;
  }

  .back-btn img {
    width: 18px;
    height: 18px;
  }

  .header {
    text-align: center;
    margin-bottom: 2rem;
  }

  .header h2 {
    font-size: 1.8rem;
    color: #333;
    margin: 0.5rem 0;
  }

  .subtitle {
    font-size: 0.95rem;
    color: #555;
  }

  .mock-list {
    display: flex;
    flex-direction: column;
    gap: 1rem;
  }

  .mock-card {
    background: white;
    border-radius: 16px;
    padding: 1.2rem;
    box-shadow: 0 4px 12px rgba(0,0,0,0.08);
    cursor: pointer;
    transition: transform 0.2s ease, background 0.3s ease;
  }

  .mock-card:hover {
    transform: translateY(-5px);
    background: #f0faff;
  }

  .mock-card h3 {
    font-size: 1.05rem;
    color: #333;
    margin-bottom: 0.5rem;
  }

  .mock-card p {
    font-size: 0.9rem;
    color: #555;
    margin: 0;
  }
</style>
