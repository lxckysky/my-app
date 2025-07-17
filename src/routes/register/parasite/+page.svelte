<script lang="ts">
  import { goto } from '$app/navigation';
  let step = 1;

  let name = '', username = '', email = '', password = '', confirmPassword = '';
  let tel = '', birthday = '', gender = '';
  let university = '', major = '', year = '';

  let q1 = '', q1_other = '', q2 = '', q3 = '', q4 = '', q4_other = '';

  let showPassword = false;
  let showConfirmPassword = false;

  let profilePic: File | null = null;
  let profilePicPreview = '';

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
    if (!name || !username || !email || !password || !confirmPassword || !tel || !birthday || !gender || !university || !major || !year || !q1 || !q2 || !q3 || !q4) {
      alert("กรุณากรอกข้อมูลให้ครบทุกช่อง");
      return;
    }
    if (password !== confirmPassword) {
      alert("รหัสผ่านไม่ตรงกัน");
      return;
    }

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
      approved: false,
      profilePic: profilePicBase64,
      qna: {
        q1: q1 === 'อื่นๆ' ? q1_other : q1,
        q2,
        q3,
        q4: q4 === 'อื่นๆ' ? q4_other : q4
      }
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
      <div class="input-group">
        <input type={showPassword ? 'text' : 'password'} bind:value={password} placeholder="Password" />
        <label class="inline-toggle"><input type="checkbox" bind:checked={showPassword} /> แสดง</label>
      </div>
      <div class="input-group">
        <input type={showConfirmPassword ? 'text' : 'password'} bind:value={confirmPassword} placeholder="Confirm Password" />
        <label class="inline-toggle"><input type="checkbox" bind:checked={showConfirmPassword} /> แสดง</label>
      </div>
      <input bind:value={tel} placeholder="เบอร์โทรศัพท์" />
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
  {:else if step === 3}
    <div class="step">
      <div class="field-group">
        <label><strong>1. เป้าหมายหลักของคุณในการใช้แพลตฟอร์มนี้คืออะไร?</strong></label>
        <select bind:value={q1}>
          <option value="" disabled>-- กรุณาเลือก --</option>
          <option>เรียนรู้เพิ่มเติม</option>
          <option>หาที่ฝึกงาน / แนะแนว</option>
          <option>ทำโปรเจกร่วมกับ Host</option>
          <option>อื่นๆ</option>
        </select>
        {#if q1 === 'อื่นๆ'}
          <input type="text" placeholder="โปรดระบุ" bind:value={q1_other} />
        {/if}
      </div>

      <div class="field-group">
        <label><strong>2. ระดับความรู้ในสาขาที่คุณเลือกอยู่ในระดับใด?</strong></label>
        <select bind:value={q2}>
          <option value="" disabled>-- กรุณาเลือก --</option>
          <option>พื้นฐาน</option>
          <option>ปานกลาง</option>
          <option>เชี่ยวชาญ</option>
        </select>
      </div>

      <div class="field-group">
        <label><strong>3. โปรดเขียนเหตุผลสั้น ๆ ว่าทำไมคุณถึงสนใจใช้แพลตฟอร์มนี้</strong></label>
        <textarea bind:value={q3} rows="3"></textarea>
      </div>

      <div class="field-group">
        <label><strong>4. รู้จักแพลตฟอร์มนี้จาก…</strong></label>
        <select bind:value={q4}>
          <option value="" disabled>-- กรุณาเลือก --</option>
          <option>เพื่อน</option>
          <option>โซเชียลมีเดีย</option>
          <option>โรงเรียน / ครูแนะแนว</option>
          <option>อื่นๆ</option>
        </select>
        {#if q4 === 'อื่นๆ'}
          <input type="text" placeholder="โปรดระบุ" bind:value={q4_other} />
        {/if}
      </div>
    </div>
  {/if}

  <div class="step-buttons">
    {#if step > 1}
      <button type="button" on:click={() => step--}>ย้อนกลับ</button>
    {/if}
    {#if step < 3}
      <button type="button" on:click={() => step++}>ถัดไป</button>
    {:else}
      <button on:click={register}>ลงทะเบียน</button>
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
  }
  button:hover {
    background: #6d4ce5;
  }
  .profile-pic-upload {
    text-align: center;
    margin-bottom: 2rem;
  }
  .upload-label {
    font-weight: 600;
    font-size: 1.1rem;
    margin-bottom: 0.5rem;
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
    margin-bottom: 1rem;
  }
  .inline-toggle {
    font-size: 0.85rem;
    color: #444;
    white-space: nowrap;
  }
  .field-group { margin-bottom: 1.5rem; }
</style>
