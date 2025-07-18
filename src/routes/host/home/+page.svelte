<script lang="ts">
  import { goto } from '$app/navigation';
  import { onMount } from 'svelte';
  import NavBar from '$lib/components/NavBar.svelte';

  let currentPage = 'home';
  let currentUserEmail = '';

  interface Post {
    type: string;
    link: string;
    description: string;
    fileURL: string | null;
    isImage: boolean;
    owner: string; // 👈 เพิ่ม field นี้
  }

  let posts: Post[] = [];

  let type = "Lecture";
  let link = "";
  let description = "";
  let file: File | null = null;

  function fileToBase64(file: File): Promise<string> {
    return new Promise((resolve, reject) => {
      const reader = new FileReader();
      reader.onload = () => resolve(reader.result as string);
      reader.onerror = reject;
      reader.readAsDataURL(file);
    });
  }

  onMount(async () => {
    const raw = localStorage.getItem('currentUser');
    if (!raw) {
      goto('/login');
      return;
    }
    const user = JSON.parse(raw);
    if (user.role !== 'host') {
      goto('/login');
      return;
    }
    currentUserEmail = user.email;

    const stored = localStorage.getItem("host_posts");
    if (stored) {
      const allPosts: Post[] = JSON.parse(stored);
      posts = allPosts.filter(p => p.owner === currentUserEmail); // 👈 กรองเฉพาะโพสต์ของตัวเอง
    }
  });

  async function submitPost() {
    let fileURL = null;
    let isImage = false;

    if (file) {
  const base64 = await fileToBase64(file);
  fileURL = `data:${file.type};base64,${base64.split(',')[1]}`;
  isImage = file.type.startsWith("image/");
}


    const newPost: Post = {
      type,
      link,
      description,
      fileURL,
      isImage,
      owner: currentUserEmail // 👈 บันทึกคนโพสต์
    };

    posts = [newPost, ...posts];

    const allPosts: Post[] = JSON.parse(localStorage.getItem("host_posts") || '[]');
    const updatedAllPosts = [newPost, ...allPosts];
    localStorage.setItem("host_posts", JSON.stringify(updatedAllPosts));

    type = "Lecture";
    link = "";
    description = "";
    file = null;
  }

  function handleFileChange(e: Event) {
    const input = e.target as HTMLInputElement;
    if (input?.files && input.files.length > 0) {
      file = input.files[0];
    }
  }

  function deletePost(index: number) {
    posts = posts.filter((_, i) => i !== index);

    const allPosts: Post[] = JSON.parse(localStorage.getItem("host_posts") || '[]');
    let count = -1;
    const updatedAllPosts = allPosts.filter((p: Post) => {
      if (p.owner === currentUserEmail) {
        count++;
        return count !== index;
      }
      return true;
    });

    localStorage.setItem("host_posts", JSON.stringify(updatedAllPosts));
  }
</script>

<NavBar {currentPage} />
<div class="container">
<main>
  <h2>HOST</h2>
  <p class="subtitle">Share your content beautifully</p>

  <div class="form-box">
    <label for="type">Content Type</label>
    <select id="type" bind:value={type}>
      <option>Lecture</option>
      <option>Tips</option>
      <option>Resources</option>
    </select>

    <label for="file">Upload File</label>
    <input id="file" type="file" on:change={handleFileChange} />

    <label for="link">Website Link (Optional)</label>
    <input id="link" type="url" bind:value={link} placeholder="Website Link(Optional)" />

    <label for="description">Describe (Optional)</label>
    <textarea id="description" bind:value={description} placeholder="Describe something"></textarea>



    <button on:click={submitPost}>POST</button>
  </div>

  <h3>Your previous post</h3>
  {#if posts.length === 0}
  <div class="no-post-box">
    <p>ยังไม่มีโพสต์</p>
  </div>
{:else}
  {#each posts as post, index}
    <div class="post">
        <p><strong>Type:</strong> {post.type}</p>
        <p><strong>Link:</strong> {post.link || "-"}</p>
        <p><strong>Description:</strong> {post.description || "-"}</p>

        {#if post.fileURL}
          {#if post.isImage}
            <img src={post.fileURL} alt="Uploaded by host" />
          {:else}
            <a href={post.fileURL} download>Download File</a>
          {/if}
        {/if}

        <div class="delete-container">
          <button class="delete-btn" on:click={() => deletePost(index)}>ลบโพสต์</button>
        </div>
      </div>
    {/each}
  {/if}
</main>
</div>

<style>

  :global(body) {
    font-family: 'Noto Sans', sans-serif;
    background-color: #f6fdff;
    color: #333;
  }


  main {
    padding: 5rem 1.2rem 2rem;
    max-width: 700px;
    margin: 0 auto;
  }

  h2 {
    font-size: 1.6rem;
    margin-bottom: 1rem;
    font-weight: 700;
    color: #2b2b2b;
  }

  .form-box {
    background: #ffffff;
    padding: 22px;
    border-radius: 14px;
    box-shadow: 0 6px 16px rgba(0, 0, 0, 0.05);
    margin-bottom: 30px;
  }

  label {
    font-weight: 600;
    font-size: 0.9rem;
    margin-top: 10px;
    display: block;
    color: #444;
  }

  input, select {
    width: 100%;
    padding: 10px 14px;
    margin-top: 6px;
    margin-bottom: 16px;
    border-radius: 14px;
    border: 2px solid #d1e3ea;
    background-color: #fff;
    font-size: 0.88rem;
    color: #333;
    height: 44px;
    box-sizing: border-box;
  }

  input:focus, select:focus {
    border-color: #7a9cc6;
    outline: none;
  }

  input::file-selector-button {
    background: transparent;
    border: none;
    font-weight: 600;
    font-family: 'Noto Sans', sans-serif;
    color: #666;
    cursor: pointer;
  }

  input::file-selector-button:hover {
    color: #4c7dc3;
  }

  button {
    width: 100%;
    padding: 11px;
    border-radius: 14px;
    background: #7a6ff0;
    color: white;
    border: none;
    cursor: pointer;
    font-weight: 600;
    font-size: 0.9rem;
    transition: background-color 0.3s ease;
    display: flex;
    justify-content: center;
    align-items: center;
  }

  button:hover {
    background: #6358d4;
  }

  .delete-btn {
    background: #e53935;
    margin-top: 10px;
    padding: 8px 16px;
    border-radius: 14px;
  }

  .delete-btn:hover {
    background: #c62828;
  }

  .post {
    background: #ffffff;
    padding: 18px;
    border-radius: 14px;
    box-shadow: 0 5px 12px rgba(0, 0, 0, 0.04);
    margin-bottom: 20px;
    transition: transform 0.2s ease;
  }
  .no-post-box {
  background: #ffffff;
  padding: 18px;
  border-radius: 14px;
  box-shadow: 0 5px 12px rgba(0, 0, 0, 0.04);
  margin-bottom: 20px;
  text-align: center;
  color: #444;
  font-weight: 500;
}


  .post:hover {
    transform: translateY(-2px);
  }

  .delete-container {
    display: flex;
    justify-content: flex-end;
    margin-top: 12px;
  }

  img {
    margin-top: 12px;
    max-width: 100%;
    border-radius: 8px;
    box-shadow: 0 3px 8px rgba(0, 0, 0, 0.08);
  }

  h3 {
    font-size: 1.1rem;
    margin-bottom: 0.8rem;
    font-weight: 600;
    color: #2b2b2b;
  }

  p {
    font-size: 0.9rem;
    margin-bottom: 0.4rem;
  }
  .container {
  min-height: 100vh;
  background: linear-gradient(135deg, #b7c7d8, #f3f4f7);
  padding: 6rem 1.5rem 3rem; /* ด้านบนเว้น navbar, ล่างให้พอดี */
  box-sizing: border-box;
}

main {
  background: #ffffff;
  padding: 2.5rem;
  border-radius: 24px;
  max-width: 560px;
  margin: 0 auto;
  box-shadow: 0 16px 32px rgba(0, 0, 0, 0.08);
  text-align: center;
}

h2 {
  font-size: 2rem;
  font-weight: 700;
  color: #333;
  margin-bottom: 0.25rem;
}

.subtitle {
  font-size: 1rem;
  color: #666;
  margin-bottom: 1.75rem;
}

.form-box {
  text-align: left;
  margin-bottom: 2rem;
}

input, select, textarea {
  width: 100%;
  padding: 10px 14px;
  border-radius: 12px;
  border: 1.5px dashed #cdd9e5;
  background-color: #f8fafc;
  margin-bottom: 16px;
  font-size: 0.9rem;
  color: #333;
}

textarea {
  height: 80px;
  resize: vertical;
}

button {
  background: linear-gradient(135deg, #8e44ec, #5e5ce6);
  border: none;
  padding: 12px;
  color: #fff;
  border-radius: 12px;
  width: 100%;
  font-weight: bold;
  font-size: 0.9rem;
  box-shadow: 0 3px 0 #4b3bbd;
  transition: all 0.2s ease-in-out;
}

button:hover {
  filter: brightness(1.05);
  transform: translateY(-1px);
}

.post, .no-post-box {
  background: #f9fbfd;
  border: 1px solid #e0e6ed;
  padding: 18px;
  border-radius: 16px;
  margin-bottom: 1.2rem;
  text-align: left;
}

.no-post-box {
  text-align: center;
  font-weight: 500;
  color: #666;
}

.delete-btn {
  background-color: #e53935;
  color: #fff;
  border: none;
  padding: 10px;
  border-radius: 12px;
  margin-top: 1rem;
  width: 100%;
  font-weight: 600;
  font-size: 0.85rem;
  cursor: pointer;
}

.delete-btn:hover {
  background-color: #c62828;
}


</style>



