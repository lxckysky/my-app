<script lang="ts">
  import { onMount } from 'svelte';
  import { goto } from '$app/navigation';
  import NavBar from '$lib/components/NavBar.svelte';

  let name = '';
  let username = '';
  let email = '';
  let tel = '';
  let dob = '';
  let gender = '';
  let role = '';
  let university = '';
  let major = '';
  let year = '';
  let level = '';
  let xp = 700;
  let profilePic = '';

  let editing = false;
  let tempName = '';
  let tempTel = '';
  let tempYear = '';
  let tempUniversity = '';
  let tempMajor = '';

  let showPasswordForm = false;
  let currentPassword = '';
  let newPassword = '';
  let confirmPassword = '';
  let passwordMsg = '';
  let currentUser: any = {};

  onMount(() => {
  const cu = localStorage.getItem('currentUser');
  if (!cu) {
    goto('/login');
    return;
  }

  try {
    currentUser = JSON.parse(cu);
  } catch (e) {
    console.error('Failed to parse currentUser from localStorage:', e);
    goto('/login');
    return;
  }

  name = currentUser?.name || '';
  username = currentUser?.username || '';
  email = currentUser?.email || '';
  tel = currentUser?.tel || '';
  dob = currentUser?.birthday || '';
  gender = currentUser?.gender || '';
  role = currentUser?.role || 'parasite';
  profilePic = currentUser?.profilePic || '/icons/avatar.png';
  level = currentUser?.level || '1';
  xp = currentUser?.xp || 700;

  // รองรับ school หรือ university แล้วแต่ว่า user เก็บอันไหน
  university = currentUser?.university || currentUser?.school || '';
  major = currentUser?.major || '';
  year = currentUser?.year || currentUser?.grade || '';

  // ตั้งค่าให้ field ชั่วคราวไว้แก้ไข
  tempName = name;
  tempTel = tel;
  tempYear = year;
  tempUniversity = university;
  tempMajor = major;
});


  function toggleEdit() {
    editing = !editing;
    if (editing) {
      tempName = name;
      tempTel = tel;
      tempYear = year;
      tempUniversity = university;
      tempMajor = major;
    }
  }

  function saveProfile() {
    name = tempName;
    tel = tempTel;
    year = tempYear;
    university = tempUniversity;
    major = tempMajor;

    const users = JSON.parse(localStorage.getItem('users') || '[]');
    const updatedUsers = users.map((u: any) =>
      u.email === email
        ? { ...u, name, tel, year, university, major }
        : u
    );
    localStorage.setItem('users', JSON.stringify(updatedUsers));

    currentUser = { ...currentUser, name, tel, year, university, major };
    localStorage.setItem('currentUser', JSON.stringify(currentUser));

    editing = false;
  }

  function togglePasswordForm() {
    showPasswordForm = !showPasswordForm;
    passwordMsg = '';
    currentPassword = '';
    newPassword = '';
    confirmPassword = '';
  }

  function changePassword() {
    if (!currentPassword || !newPassword || !confirmPassword) {
      passwordMsg = 'Please fill in all fields.';
      return;
    }

    if (currentPassword !== currentUser.password) {
      passwordMsg = 'Incorrect current password.';
      return;
    }

    if (newPassword !== confirmPassword) {
      passwordMsg = 'New passwords do not match.';
      return;
    }

    const users = JSON.parse(localStorage.getItem('users') || '[]');
    const updatedUsers = users.map((u: any) =>
      u.email === email ? { ...u, password: newPassword } : u
    );
    localStorage.setItem('users', JSON.stringify(updatedUsers));

    currentUser.password = newPassword;
    localStorage.setItem('currentUser', JSON.stringify(currentUser));

    passwordMsg = 'Password changed successfully!';
    showPasswordForm = false;
  }

  function logout() {
    localStorage.removeItem('currentUser');
    goto('/login');
  }
</script>

<NavBar currentPage="profile" />

<div class="profile-container">
  <div class="info-box">
    <div class="profile-header">
  <h1 class="profile-title">Challenger Profile</h1>
  <img class="avatar" src={profilePic} alt="avatar" />
  <h2 class="username">{username}</h2>

  <h2 class="level-display">🎮 LEVEL {level}</h2>
  <div class="xp-bar-container">
    <p class="xp-text">🔥 {xp} / 1000 XP</p>
    <div class="xp-bar">
      <div class="xp-fill" style="width: {xp / 10}%"></div>
    </div>
  </div>

  <button class="record-btn">My Record Book</button>
</div>


    <h3>Personal Info</h3>
    <p><strong>Name:</strong> {#if editing}<input bind:value={tempName} />{:else}{name}{/if}</p>
    <p><strong>Username:</strong> {username}</p>
    <p><strong>Email:</strong> {email}</p>
    <p><strong>Phone:</strong> {#if editing}<input bind:value={tempTel} />{:else}{tel}{/if}</p>
    <p><strong>Date of Birth:</strong> {dob}</p>
    <p><strong>Gender:</strong> {gender}</p>
    <p><strong>Role:</strong> {role}</p>
    <p><strong>University:</strong> {#if editing}<input bind:value={tempUniversity} />{:else}{university}{/if}</p>
    <p><strong>Major:</strong> {#if editing}<input bind:value={tempMajor} />{:else}{major}{/if}</p>
    <p><strong>Year:</strong> {#if editing}<input bind:value={tempYear} />{:else}{year}{/if}</p>

    <div class="btn-group">
      {#if editing}
        <button on:click={saveProfile}>Save</button>
      {:else}
        <button on:click={toggleEdit}>Edit Profile</button>
      {/if}
    </div>
  </div>

  <div class="info-box">
    <h3>Password Settings</h3>
    <button on:click={togglePasswordForm} class="change-password-btn">Change Password</button>

    {#if showPasswordForm}
      <div class="password-form">
        <input type="password" bind:value={currentPassword} placeholder="Current Password" />
        <input type="password" bind:value={newPassword} placeholder="New Password" />
        <input type="password" bind:value={confirmPassword} placeholder="Confirm New Password" />
        <button on:click={changePassword}>Save Password</button>
        {#if passwordMsg}<p class="msg">{passwordMsg}</p>{/if}
      </div>
    {/if}
  </div>

  <button class="logout-btn" on:click={logout}>Logout</button>
</div>


<style>
  :global(body) {
    font-family: 'Noto Sans', sans-serif;
    background: linear-gradient(to bottom right, #e4ebf5, #dfe9f3);
    margin: 0;
  }

  .profile-header {
  text-align: center;
  margin-bottom: 2rem;
}

.profile-title {
  font-size: 1.5rem;
  font-weight: 600;
  margin-bottom: 1rem;
  color: #333;
}

.avatar {
  width: 100px;
  height: 100px;
  border-radius: 50%;
  object-fit: cover;
  margin-bottom: 0.5rem;
}

.username {
  font-size: 1.2rem;
  color: #e64980;
  margin: 0.3rem 0 1rem;
}

.record-btn {
  background: white;
  border: 2px solid #333;
  padding: 0.4rem 1rem;
  border-radius: 999px;
  font-weight: 600;
  cursor: pointer;
  transition: background 0.3s ease;
}

.record-btn:hover {
  background: #f0f0f0;
}




  .profile-container {
    max-width: 600px;
    margin: 6rem auto 4rem;
    padding: 2rem;
    background: white;
    border-radius: 20px;
    box-shadow: 0 12px 30px rgba(0, 0, 0, 0.08);
    text-align: center;
  }

  .avatar {
    width: 100px;
    height: 100px;
    border-radius: 50%;
    margin-bottom: 1rem;
  }

  h2 {
    margin: 0.5rem 0;
    color: #5e5ce6;
    font-size: 1.6rem;
  }

  .name-input {
    font-size: 1.4rem;
    padding: 0.5rem;
    border: 1px dashed #aaa;
    border-radius: 10px;
    margin-bottom: 1rem;
  }

  .record-btn {
    background: transparent;
    border: 2px solid #5e5ce6;
    padding: 0.4rem 1rem;
    border-radius: 20px;
    font-weight: 600;
    margin-bottom: 1.5rem;
    cursor: pointer;
  }

  .info-box {
    background: #f9fbff;
    border-radius: 12px;
    padding: 1rem;
    margin: 1rem 0;
    text-align: left;
  }

  .info-box h3 {
    color: #444;
    margin-bottom: 0.8rem;
  }

  .info-box p {
    margin: 0.5rem 0;
  }

  .info-box input {
    padding: 0.4rem;
    border-radius: 8px;
    border: 1px solid #ccc;
    margin-top: 0.3rem;
    width: 100%;
  }
  .info-box p {
  margin: 0.5rem 0;
  display: flex;
  align-items: center;
}

.info-box p strong {
  width: 110px;
  flex-shrink: 0;
}


  .btn-group {
    display: flex;
    justify-content: center;
    margin-top: 1rem;
  }

  .btn-group button {
    background: #5e5ce6;
    color: white;
    padding: 0.5rem 1rem;
    border: none;
    border-radius: 10px;
    margin: 0 0.5rem;
    cursor: pointer;
    font-weight: 600;
  }

  .password-form input {
    margin: 0.4rem 0;
    padding: 0.5rem;
    width: 100%;
    border-radius: 8px;
    border: 1px solid #ccc;
  }

  .password-form .msg {
    color: #d32f2f;
    margin-top: 0.5rem;
  }

  .logout-btn {
    background: #f44336;
    color: white;
    padding: 0.6rem 1rem;
    border: none;
    border-radius: 10px;
    margin-top: 2rem;
    font-weight: 600;
    cursor: pointer;
  }

  .change-password-btn {
    background: #5e5ce6;
    color: white;
    padding: 0.5rem 1rem;
    border: none;
    border-radius: 10px;
    font-weight: 600;
    margin-top: 0.5rem;
    cursor: pointer;
    transition: background 0.3s ease;
  }

  .change-password-btn:hover {
    background: #4a48cc;
  }
  .password-form {
  margin-top: 1rem;
  background: #f4f6fc;
  padding: 1rem;
  border-radius: 14px;
  box-shadow: inset 0 1px 3px rgba(0, 0, 0, 0.05);
}

.password-form input {
  margin: 0.5rem 0;
  padding: 0.6rem 1rem;
  width: 100%;
  border-radius: 12px;
  border: 1px solid #ccc;
  font-size: 1rem;
  background-color: #fff;
  transition: border 0.3s ease;
}

.password-form input:focus {
  border-color: #5e5ce6;
  outline: none;
}

.password-form button {
  background: linear-gradient(to right, #7b61ff, #5e5ce6);
  color: white;
  padding: 0.6rem 1.4rem;
  border: none;
  border-radius: 999px;
  font-weight: bold;
  font-size: 1rem;
  margin-top: 0.8rem;
  cursor: pointer;
  transition: background 0.3s ease;
}

.password-form button:hover {
  background: linear-gradient(to right, #6f5ee8, #4e4ce0);
}

.password-form .msg {
  color: #e53935;
  margin-top: 0.5rem;
  font-size: 0.9rem;
}
.level-display {
  font-size: 1.6rem;
  font-weight: 800;
  letter-spacing: 0.5px;
  text-transform: uppercase;
  text-align: center;
  background: linear-gradient(to right, #ff758c, #ff7eb3);
  -webkit-background-clip: text;
  -webkit-text-fill-color: transparent;
  text-shadow: 0 0 2px rgba(255, 120, 150, 0.4);
  margin-bottom: 0.5rem;
}


.xp-bar-container {
  text-align: center;
  margin-bottom: 1.5rem;
}

.xp-text {
  font-weight: bold;
  color: #444;
  margin-bottom: 0.3rem;
}

.xp-bar {
  width: 80%;
  height: 12px;
  background-color: #e0e0e0;
  border-radius: 999px;
  margin: 0 auto;
  overflow: hidden;
}

.xp-fill {
  height: 100%;
  background: linear-gradient(to right, #ff758c, #ff7eb3);
  border-radius: 999px;
  transition: width 0.4s ease;
}


</style>