<script lang="ts">
  import { goto } from '$app/navigation';
  function goToLogin() {
    goto('/login');
  }
</script>

<div class="screen">
  <div class="content">
    <h1 class="title">
      One App To<br />
      <span class="highlight">Learn Anything</span>
    </h1>
    <p class="subtitle">
      🚀 Learn - Grow - Guide.<br />
      All in one gamified journey.
    </p>
    <img src="/icons/graduation.png" alt="Graduation Icon" class="hero" />
    <button class="get-started" on:click={goToLogin}>🚀 Get Started</button>
  </div>
</div>

<style>
  .screen {
    display: flex;
    justify-content: center;
    align-items: center;
    height: 100vh;
    background: linear-gradient(135deg, #f8f9ff, #e0ecff);
    font-family: 'Segoe UI', 'Noto Sans Thai', sans-serif;
    animation: fadeIn 1s ease;
  }

  .content {
    text-align: center;
    width: 100%;
    max-width: 480px;
    padding: 3rem 2rem;
    background: white;
    border-radius: 2rem;
    box-shadow: 0 12px 32px rgba(0, 0, 0, 0.08);
    display: flex;
    flex-direction: column;
    align-items: center;
    animation: popUp 0.8s ease;
  }

  .title {
    font-size: 2rem;
    font-weight: 700;
    color: #333;
    line-height: 1.4;
    margin-bottom: 0.8rem;
  }

  .highlight {
    background: linear-gradient(90deg, #7b61ff, #9c8cff);
    -webkit-background-clip: text;
    -webkit-text-fill-color: transparent;
  }

  .subtitle {
    font-size: 1.05rem;
    color: #444;
    margin-bottom: 1.5rem;
    line-height: 1.5;
  }

  .hero {
    width: 160px;
    height: auto;
    margin-bottom: 2rem;
    animation: float 2.5s ease-in-out infinite;
  }

  .get-started {
    background: linear-gradient(to right, #ff8a65, #f44336);
    color: white;
    padding: 0.85rem 2.2rem;
    border: none;
    border-radius: 999px;
    font-size: 1.1rem;
    font-weight: 600;
    cursor: pointer;
    box-shadow: 0 6px 20px rgba(244, 67, 54, 0.4);
    transition: transform 0.2s ease, box-shadow 0.3s ease;
  }

  .get-started:hover {
    transform: translateY(-2px);
    box-shadow: 0 8px 24px rgba(244, 67, 54, 0.5);
  }

  @keyframes fadeIn {
    from { opacity: 0; transform: translateY(20px); }
    to { opacity: 1; transform: translateY(0); }
  }

  @keyframes popUp {
    0% { transform: scale(0.95); opacity: 0; }
    100% { transform: scale(1); opacity: 1; }
  }

  @keyframes float {
    0%, 100% { transform: translateY(0); }
    50% { transform: translateY(-8px); }
  }
</style>
