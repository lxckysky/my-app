<script lang="ts">
  import { goto } from '$app/navigation';
  let step = 1;

  let name = '', username = '', email = '', password = '', confirmPassword = '';
  let tel = '', dob = '', gender = '';
  let university = '', major = '', year = '';
  let showPassword = false;
  let showConfirmPassword = false;

  let docStudentId: File | null = null;
  let docCertificate: File | null = null;
  let profilePic: File | null = null;

  let q1 = '', q2 = '', q3 = '', q4 = '', q4_other = '';

  let profilePicPreview = '';
  let studentIdPreview = '';
  let certificatePreview = '';

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
    name && username && email && password && confirmPassword && tel && dob && gender &&
    !emailError && !passwordError && !confirmPasswordError && !telError;

  $: isStep2Valid = university && major && year && docStudentId;

  $: isStep3Valid =
    q1 && q2 && q3 && q4 && (q4 !== 'อื่นๆ' || q4_other);

  function handleFileChange(e: Event, type: 'studentId' | 'certificate' | 'profilePic') {
    const file = (e.target as HTMLInputElement)?.files?.[0] || null;
    if (!file) return;

    const url = URL.createObjectURL(file);

    if (type === 'studentId') {
      docStudentId = file;
      studentIdPreview = url;
    } else if (type === 'certificate') {
      docCertificate = file;
      certificatePreview = url;
    } else if (type === 'profilePic') {
      profilePic = file;
      profilePicPreview = url;
    }
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
    const users = JSON.parse(localStorage.getItem('users_host') || '[]');
    if (users.some((u: any) => u.email === email)) {
      alert("อีเมลนี้ถูกใช้งานแล้ว");
      return;
    }

    const studentIdBase64 = await toBase64(docStudentId!);
    const certificateBase64 = docCertificate ? await toBase64(docCertificate) : '';
    const profilePicBase64 = profilePic ? await toBase64(profilePic) : '';

    const newUser = {
      name, username, email, password, tel, dob, gender,
      university, major, year,
      role: 'host',
      approved: false,
      profilePic: profilePicBase64,
      verification: {
        studentId: studentIdBase64,
        certificate: certificateBase64
      },
      qna: {
        q1, q2, q3,
        q4: q4 === 'อื่นๆ' ? q4_other : q4
      }
    };

    users.push(newUser);
    localStorage.setItem('users_host', JSON.stringify(users));
    localStorage.setItem('currentUser', JSON.stringify(newUser));
    goto('/register/host/success');
  }
</script>

<main class="register-container">
  <h2>สมัครสมาชิก (Host)</h2>
  {#if step === 1}
    <div class="step">
      <div class="profile-pic-upload">
        <label class="upload-label">📸 รูปโปรไฟล์</label>
        <input type="file" accept="image/*" on:change={(e) => handleFileChange(e, 'profilePic')} />
        {#if profilePicPreview}<img src={profilePicPreview} alt="ตัวอย่างโปรไฟล์" class="preview-image" />{/if}
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
      <input type="date" bind:value={dob} />
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
      <input bind:value={major} placeholder="สาขา / คณะ" />
      <input bind:value={year} placeholder="ระดับชั้นปี" />
      <div class="field-group">
        <label>📎 บัตรนักศึกษา/พนักงาน *</label>
        <input type="file" accept="image/*,.pdf" on:change={(e) => handleFileChange(e, 'studentId')} />
        {#if studentIdPreview}<img src={studentIdPreview} alt="บัตรนักศึกษา" class="preview-image" />{/if}
      </div>
      <div class="field-group">
        <label>📎 ใบรับรอง / ประวัติการสอน</label>
        <input type="file" accept="image/*,.pdf" on:change={(e) => handleFileChange(e, 'certificate')} />
        {#if certificatePreview}<img src={certificatePreview} alt="ใบรับรอง" class="preview-image" />{/if}
      </div>
    </div>
  {:else if step === 3}
    <div class="step">
      <div class="field-group">
        <label><strong>1. คุณมีประสบการณ์ในการสอนมากี่ปี?</strong></label>
        <select bind:value={q1}>
          <option value="" disabled>-- กรุณาเลือก --</option>
          <option>ไม่มีเลย</option>
          <option>น้อยกว่า 1 ปี</option>
          <option>1-3 ปี</option>
          <option>มากกว่า 3 ปี</option>
        </select>
      </div>
      <div class="field-group">
        <label><strong>2. เหตุผลหลักที่คุณเลือกใช้เว็บไซต์นี้?</strong></label>
        <select bind:value={q2}>
          <option value="" disabled>-- กรุณาเลือก --</option>
          <option>แชร์ความรู้</option>
          <option>หารายได้</option>
          <option>สร้างโปรเจกร่วม</option>
          <option>อื่นๆ</option>
        </select>
      </div>
      <div class="field-group">
        <label><strong>3. ทำไมคุณถึงอยากเป็น Host?</strong></label>
        <textarea bind:value={q3}></textarea>
      </div>
      <div class="field-group">
        <label><strong>4. รู้จักแพลตฟอร์มนี้จาก…</strong></label>
        <select bind:value={q4}>
          <option value="" disabled>-- กรุณาเลือก --</option>
          <option>เพื่อนแนะนำ</option>
          <option>โซเชียลมีเดีย</option>
          <option>กิจกรรมในโรงเรียน</option>
          <option>อื่นๆ</option>
        </select>
        {#if q4 === 'อื่นๆ'}
          <input bind:value={q4_other} placeholder="โปรดระบุ" />
        {/if}
      </div>
    </div>
  {/if}

  <div class="step-buttons">
    {#if step > 1}
      <button type="button" on:click={() => step--}>ย้อนกลับ</button>
    {/if}
    {#if step === 1}
      <button type="button" on:click={() => step++} disabled={!isStep1Valid}>ถัดไป</button>
    {:else if step === 2}
      <button type="button" on:click={() => step++} disabled={!isStep2Valid}>ถัดไป</button>
    {:else}
      <button on:click={register} disabled={!isStep3Valid}>ลงทะเบียน</button>
    {/if}
  </div>
</main>

<style>
  @import url('https://fonts.googleapis.com/css2?family=Segoe+UI&display=swap');

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

  input,
  select,
  textarea {
    display: block;
    width: 100%;
    padding: 0.75rem;
    margin-bottom: 1rem;
    border: 1px solid #ccc;
    border-radius: 12px;
    font-size: 1rem;
  }

  .input-group {
    display: flex;
    align-items: center;
    gap: 0.75rem;
  }

  .inline-toggle {
    font-size: 0.85rem;
    color: #444;
    white-space: nowrap;
  }

  .step-buttons {
    display: flex;
    justify-content: space-between;
    gap: 1rem;
    margin-top: 1.5rem;
  }

  button {
    flex: 1;
    padding: 0.85rem;
    background: #64d1c3;
    color: white;
    font-weight: bold;
    border: none;
    border-radius: 14px;
    font-size: 1rem;
    cursor: pointer;
    transition: background 0.3s ease;
  }

  button:hover {
    background: #4bc0b0;
  }

  button:disabled {
    background: #ccc;
    cursor: not-allowed;
  }

  .error-text {
    color: #e53935;
    font-size: 0.85rem;
    margin-top: -0.6rem;
    margin-bottom: 0.8rem;
    padding-left: 4px;
  }

  .preview-image {
    display: block;
    margin: 1rem auto;
    max-width: 120px;
    max-height: 120px;
    object-fit: cover;
    border: 1px solid #ccc;
    border-radius: 12px;
    box-shadow: 0 4px 10px rgba(0, 0, 0, 0.05);
  }
</style>