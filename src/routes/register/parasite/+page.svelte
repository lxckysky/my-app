<script lang="ts">
  import { goto } from '$app/navigation';
  let step = 1;

  let name = '', username = '', email = '', password = '', confirmPassword = '';
  let tel = '', birthday = '', gender = '';
  let university = '', major = '', year = '';

  let showPassword = false;
  let showConfirmPassword = false;

  let profilePic: File | null = null;
  let profilePicPreview = '';

  let emailError = '';
  let passwordError = '';
  let confirmPasswordError = '';
  let telError = '';

  $: emailError = email && !/^[^\s@]+@[^\s@]+\.[^\s@]+$/.test(email)
    ? 'รูปแบบอีเมลไม่ถูกต้อง'
    : '';

  $: passwordError = password && password.length < 8
    ? 'รหัสผ่านต้องมีอย่างน้อย 8 ตัวอักษร'
    : '';

  $: confirmPasswordError = confirmPassword && confirmPassword !== password
    ? 'รหัสผ่านไม่ตรงกัน'
    : '';

  $: telError = tel && !/^\d{10}$/.test(tel)
    ? 'เบอร์โทรต้องเป็นตัวเลข 10 หลัก'
    : '';

  $: isStep1Valid =
    name && username && email && password && confirmPassword && tel && birthday && gender &&
    !emailError && !passwordError && !confirmPasswordError && !telError;

  $: isStep2Valid = university && major && year;

  function handleFileChange(e: Event) {
    const file = (e.target as HTMLInputElement)?.files?.[0] || null;
    if (!file) return;
    profilePic = file;
    profilePicPreview = URL.createObjectURL(file);
  }

  function toBase64(file: File): Promise<string> {
    return new Promise((resolve, reject) => {
      const reader = new FileReader();
      reader.onload = () => resolve(reader.result as string);
      reader.onerror = reject;
      reader.readAsDataURL(file);
    });
  }

  async function register() {
    const users = JSON.parse(localStorage.getItem('users_parasite') || '[]');
    if (users.some((u: any) => u.email === email)) {
      alert("อีเมลนี้มีการใช้งานแล้ว");
      return;
    }

    const profilePicBase64 = profilePic ? await toBase64(profilePic) : '';

    const newUser = {
      name,
      username,
      email,
      password,
      tel,
      birthday,
      gender,
      university,
      major,
      year,
      role: 'parasite',
      approved: true,
      profilePic: profilePicBase64
    };

    users.push(newUser);
    localStorage.setItem('users_parasite', JSON.stringify(users));
    localStorage.setItem('currentUser', JSON.stringify(newUser));
    goto('/register/parasite/success');
  }
</script>

<main class="register-container">
  <h2>สมัครสมาชิก (Challenger)</h2>

  {#if step === 1}
    <div class="step">
      <div class="profile-pic-upload">
        <label class="upload-label">📸 รูปโปรไฟล์</label>
        <input type="file" accept="image/*" on:change={handleFileChange} />
        {#if profilePicPreview}
          <img src={profilePicPreview} alt="ตัวอย่างโปรไฟล์" class="preview-image" />
        {/if}
      </div>
      <input bind:value={name} placeholder="ชื่อ-นามสกุล" />

      <input bind:value={username} placeholder="Username" />

      <input type="email" bind:value={email} placeholder="Email" />
      {#if emailError}<p class="error-text">{emailError}</p>{/if}

      <div class="input-group">
        <input type={showPassword ? 'text' : 'password'} bind:value={password} placeholder="Password" />
        <label class="inline-toggle"><input type="checkbox" bind:checked={showPassword} /> แสดง</label>
      </div>
      {#if passwordError}<p class="error-text">{passwordError}</p>{/if}

      <div class="input-group">
        <input type={showConfirmPassword ? 'text' : 'password'} bind:value={confirmPassword} placeholder="Confirm Password" />
        <label class="inline-toggle"><input type="checkbox" bind:checked={showConfirmPassword} /> แสดง</label>
      </div>
      {#if confirmPasswordError}<p class="error-text">{confirmPasswordError}</p>{/if}

      <input bind:value={tel} placeholder="เบอร์โทรศัพท์" />
      {#if telError}<p class="error-text">{telError}</p>{/if}

      <input type="date" bind:value={birthday} />

      <select bind:value={gender}>
        <option value="" disabled>เลือกเพศ</option>
        <option>ชาย</option>
        <option>หญิง</option>
        <option>ไม่ระบุ</option>
      </select>
    </div>
  {:else if step === 2}
    <div class="step">
      <input bind:value={university} placeholder="ชื่อโรงเรียน / มหาวิทยาลัย" />
      <input bind:value={major} placeholder="สายการเรียน / แผนการเรียน" />
      <input bind:value={year} placeholder="ระดับชั้นปี (ม.4 / ม.5 / ม.6)" />
    </div>
  {/if}

  <div class="step-buttons">
    {#if step > 1}
      <button type="button" on:click={() => step--}>ย้อนกลับ</button>
    {/if}
    {#if step < 2}
      <button type="button" on:click={() => step++} disabled={!isStep1Valid}>ถัดไป</button>
    {:else}
      <button on:click={register} disabled={!isStep2Valid}>ลงทะเบียน</button>
    {/if}
  </div>
</main>

<style>
  .register-container {
    max-width: 500px;
    margin: 3rem auto;
    padding: 2.5rem;
    border-radius: 20px;
    background: #ffffff;
    box-shadow: 0 8px 20px rgba(0, 0, 0, 0.1);
    font-family: 'Segoe UI', sans-serif;
  }
  h2 {
    text-align: center;
    margin-bottom: 1.5rem;
  }
  input, select, textarea {
    display: block;
    width: 100%;
    padding: 0.75rem;
    margin-bottom: 1.2rem;
    border: 1px solid #ccc;
    border-radius: 12px;
    font-size: 1rem;
  }
  .step { margin-bottom: 1.5rem; }
  .step-buttons {
    display: flex;
    justify-content: space-between;
    gap: 1rem;
    margin-top: 1.5rem;
  }
  button {
    flex: 1;
    padding: 0.85rem;
    background: #8266f1;
    color: white;
    font-weight: bold;
    border: none;
    border-radius: 14px;
    font-size: 1rem;
    cursor: pointer;
    transition: background 0.3s ease;
  }
  button:hover {
    background: #6d4ce5;
  }
  button:disabled {
    background: #ccc;
    cursor: not-allowed;
  }
  .profile-pic-upload {
    text-align: center;
    margin-bottom: 2rem;
  }
  .upload-label {
    font-weight: 600;
    font-size: 1.1rem;
    margin-bottom: 1rem;
    display: block;
    color: #333;
  }
  .preview-image {
    margin-top: 1rem;
    max-width: 120px;
    max-height: 120px;
    object-fit: cover;
    border: 1px solid #ccc;
    border-radius: 12px;
  }
  .input-group {
    display: flex;
    align-items: center;
    gap: 0.75rem;
    margin-bottom: 0.3rem;
  }
  .inline-toggle {
    font-size: 0.85rem;
    color: #444;
    white-space: nowrap;
  }
  .error-text {
    color: #e53935;
    font-size: 0.85rem;
    margin-top: -0.6rem;
    margin-bottom: 0.8rem;
    padding-left: 4px;
  }
</style>
