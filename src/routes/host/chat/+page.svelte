<svelte:head>
  <link href="https://fonts.googleapis.com/css2?family=Noto+Sans:wght@400;600;700&display=swap" rel="stylesheet" />
</svelte:head>

<script lang="ts">
  import { onMount, tick } from 'svelte';
  import { goto } from '$app/navigation';
  import NavBar from '$lib/components/NavBar.svelte'; // ✅ ชื่อตรงกับไฟล์จริง

  let currentUserEmail = '';
  let selectedUser = '';
  let newMessage = '';
  let chatbox: HTMLDivElement;
  let messages: { sender: string; receiver: string; text: string; timestamp: string }[] = [];
  let allUsers: string[] = [];
  let currentPage = 'chat';



  onMount(() => {
    const user = localStorage.getItem('currentUser');
    if (!user) {
      goto('/login');
      return;
    }

    currentUserEmail = JSON.parse(user).email;
    const currentUser = JSON.parse(user);
    const currentRole = currentUser.role;
    const targetRole = currentRole === 'host' ? 'parasite' : 'host';
    const allUsersRaw = JSON.parse(localStorage.getItem(`users_${targetRole}`) || '[]');
    allUsers = allUsersRaw.map((u: any) => u.email).filter((u: string) => u !== currentUserEmail);

    selectedUser = localStorage.getItem('chatWith') || '';
    if (selectedUser) loadMessages();
  });

  function loadMessages() {
    const all = JSON.parse(localStorage.getItem('messages') || '[]');
    messages = all.filter(
      (m: any) =>
        (m.sender === currentUserEmail && m.receiver === selectedUser) ||
        (m.sender === selectedUser && m.receiver === currentUserEmail)
    );
    scrollToBottom();
  }

  function sendMessage() {
    const text = newMessage.trim();
    if (!text || !selectedUser || !currentUserEmail) return;

    const all = JSON.parse(localStorage.getItem('messages') || '[]');
    const newMsg = {
      sender: currentUserEmail,
      receiver: selectedUser,
      text,
      timestamp: new Date().toISOString()
    };
    all.push(newMsg);
    localStorage.setItem('messages', JSON.stringify(all));
    newMessage = '';
    loadMessages();
  }

  async function scrollToBottom() {
    await tick();
    if (chatbox) chatbox.scrollTop = chatbox.scrollHeight;
  }

  function goTo(page: string) {
    goto(`/host/${page}`);
  }
</script>


<NavBar {currentPage} />


<div class="container">
  <div class="chat-wrapper">
    <div class="sidebar">
      <h3>  ผู้ใช้งาน</h3>
      <ul>
        {#each allUsers as user}
          <li
  class:selected={selectedUser === user}
  on:click={() => { selectedUser = user; localStorage.setItem('chatWith', user); loadMessages(); }}
  role="button"
  tabindex="0"
  on:keydown={(e) => e.key === 'Enter' && selectedUser !== user && (() => {
    selectedUser = user;
    localStorage.setItem('chatWith', user);
    loadMessages();
  })()}
>
  {user}
</li>

        {/each}
      </ul>
    </div>

    <div class="chat-area">
      <h2>คุยกับ: {selectedUser || 'ยังไม่เลือกผู้ใช้'}</h2>

      {#if selectedUser}
        <div class="chat-box" bind:this={chatbox}>
          {#each messages as msg}
            <div class="msg-row {msg.sender === currentUserEmail ? 'right' : 'left'}">
              <div class="msg-bubble {msg.sender === currentUserEmail ? 'self' : 'other'}">
                <div class="msg-text">{msg.text}</div>
                <div class="msg-time">{new Date(msg.timestamp).toLocaleTimeString([], { hour: '2-digit', minute: '2-digit' })}</div>
              </div>
            </div>
          {/each}
        </div>

        <div class="chat-input">
          <input bind:value={newMessage} placeholder="พิมพ์ข้อความ..." on:keydown={(e) => e.key === 'Enter' && sendMessage()} />
          <button on:click={sendMessage}>ส่ง</button>
        </div>
      {:else}
        <p class="no-chat">กรุณาเลือกผู้ใช้เพื่อเริ่มแชท</p>
      {/if}
    </div>
  </div>
</div>

<style>

:global(body) {
  font-family: 'Noto Sans', sans-serif;
  margin: 0;
  background: linear-gradient(135deg, #cbdcf4, #eaf2f8);
}




.container {
  padding-top: 4.5rem;
  display: flex;
  justify-content: center;
  min-height: 100vh;
  padding-bottom: 4rem; /* ✅ เพิ่ม padding ล่างให้ลอยขึ้น */
}


.chat-wrapper {
  background: #ffffff;
  border-radius: 20px;
  width: 90%;
  max-width: 1000px;
  height: 85vh; /* ย่อความสูงลง */
  display: flex;
  box-shadow: 0 12px 30px rgba(0, 0, 0, 0.08);
  overflow-y: auto;

}


.sidebar {
  background: #f7f9fc;
  width: 240px;
  padding: 1rem;
  border-right: 1px solid #e2e8f0;
  overflow-y: auto;
}

.sidebar h3 {
  margin-bottom: 1rem;
  font-size: 1rem;
  color: #444;
}

.sidebar ul {
  list-style: none;
  padding: 0;
  margin: 0;
}

.sidebar li {
  padding: 0.6rem;
  border-radius: 10px;
  cursor: pointer;
  margin-bottom: 0.5rem;
  transition: background 0.2s;
}

.sidebar li.selected {
  background: #5e5ce6;
  color: #fff;
}

.chat-area {
  flex: 1;
  padding: 1.2rem;
  display: flex;
  flex-direction: column;
  justify-content: space-between;
  min-width: 0; /* ช่วยให้ไม่ overflow */
}


.chat-area h2 {
  font-size: 1.1rem;
  color: #333;
  margin-bottom: 1rem;
  text-align: center;
}

.chat-box {
  flex: 1;
  overflow-y: auto; /* ✅ ให้กล่องแชทเลื่อนได้ */
  padding: 1rem;
  background: #fefefe;
  border-radius: 14px;
  border: 1px dashed #d2dce9;
  box-shadow: inset 0 1px 3px rgba(0,0,0,0.03);
  margin-bottom: 1rem;
}


.msg-row {
  display: flex;
  margin-bottom: 0.8rem;
}

.msg-row.left {
  justify-content: flex-start;
}
.msg-row.right {
  justify-content: flex-end;
}

.msg-bubble {
  max-width: 80%;
  padding: 0.7rem 1rem;
  border-radius: 18px;
  font-size: 0.9rem;
  box-shadow: 0 2px 6px rgba(0, 0, 0, 0.05);
}

.msg-bubble.self {
  background: #e4dcfd;
  color: #333;
}
.msg-bubble.other {
  background: #e0f5ff;
  color: #333;
}

.msg-time {
  font-size: 0.7rem;
  margin-top: 0.3rem;
  color: #777;
}

.chat-input {
  display: flex;
  gap: 0.5rem;
  padding-top: 0.5rem;
}

.chat-input input {
  flex: 1;
  border: 1.5px dashed #cdd9e5;
  border-radius: 12px;
  padding: 0.6rem 0.8rem;
  background: #f9fbfc;
  font-size: 0.9rem;
}

.chat-input button {
  background: linear-gradient(135deg, #8e44ec, #5e5ce6);
  border: none;
  color: white;
  border-radius: 12px;
  padding: 0.6rem 1rem;
  font-weight: 600;
  font-size: 0.9rem;
  box-shadow: 0 2px 0 #4b3bbd;
  cursor: pointer;
}

.no-chat {
  text-align: center;
  margin-top: 2rem;
  color: #888;
  font-size: 0.95rem;
}


</style>

