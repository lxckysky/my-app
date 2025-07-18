<script lang="ts">
  import { onMount } from 'svelte';
  import { goto } from '$app/navigation';
  import NavBar from '$lib/components/NavBar.svelte';

  interface Post {
    type: string;
    link: string;
    description: string;
    fileURL: string | null;
    isImage: boolean;
    owner?: string;
  }

  let posts: Post[] = [];

  onMount(() => {
    const stored = localStorage.getItem("host_posts");
    if (stored) posts = JSON.parse(stored);
  });
</script>

<NavBar currentPage="home" />

<div class="container">
  <main>
    <div class="back-btn" on:click={() => history.back()}>
      <img src="/icons/back.png" alt="Back" />
      <span>Back</span>
    </div>

    <h2>💼 Tips for Work/Career</h2>
    <p class="subtitle">แนวทางสู่โลกการทำงานจาก Host</p>

    {#if posts.filter(p => p.type === 'Resources').length === 0}
      <div class="empty">ยังไม่มีโพสต์เกี่ยวกับ Career จาก Host</div>
    {:else}
      {#each posts.filter(p => p.type === 'Resources') as post}
        <div class="post-card">
          <p class="desc">{post.description}</p>
          {#if post.link}
            <p class="link">🔗 <a href={post.link} target="_blank">{post.link}</a></p>
          {/if}
          {#if post.fileURL}
            {#if post.isImage}
              <img src={post.fileURL} alt="Career Image" />
            {:else}
              <a class="file-btn" href={post.fileURL} target="_blank">📎 ดาวน์โหลดไฟล์</a>
            {/if}
          {/if}
          <p class="owner">🧑‍💼 Posted by: <strong>{post.owner || 'Unknown'}</strong></p>
        </div>
      {/each}
    {/if}
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
    font-size: 1.6rem;
    font-weight: 700;
    color: #333;
    text-align: center;
    margin-bottom: 0.5rem;
  }

  .subtitle {
    text-align: center;
    font-size: 0.95rem;
    color: #666;
    margin-bottom: 1.5rem;
  }

  .empty {
    text-align: center;
    font-size: 0.9rem;
    color: #999;
    padding: 1rem;
    background: #f8fafc;
    border-radius: 14px;
  }

  .post-card {
    background: #f9fbff;
    border: 1px dashed #cbd5e1;
    border-radius: 16px;
    padding: 1rem 1.2rem;
    margin-bottom: 1.5rem;
  }

  .desc {
    font-weight: 600;
    margin-bottom: 0.3rem;
    color: #444;
  }

  .link {
    font-size: 0.85rem;
    color: #5e5ce6;
    margin-bottom: 0.8rem;
    word-wrap: break-word;
  }

  .post-card img {
    max-width: 100%;
    border-radius: 10px;
    margin-top: 0.5rem;
  }

  .file-btn {
    display: inline-block;
    margin-top: 0.4rem;
    background: linear-gradient(135deg, #8e44ec, #5e5ce6);
    color: white;
    padding: 0.4rem 0.8rem;
    border-radius: 12px;
    text-decoration: none;
    font-size: 0.85rem;
    font-weight: 500;
    box-shadow: 0 3px 0 #4b3bbd;
  }

  .file-btn:hover {
    filter: brightness(1.1);
  }

  .owner {
    margin-top: 0.6rem;
    font-size: 0.8rem;
    color: #777;
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