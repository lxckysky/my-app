<script lang="ts">
  import { goto } from '$app/navigation';
  import NavBar from '$lib/components/NavBar.svelte';
  import { onMount } from 'svelte';

    let currentUser: any = {};


    onMount(() => {
    const storedUser = localStorage.getItem('currentUser');
    if (storedUser) {
        currentUser = JSON.parse(storedUser);
    }
    });



  interface Competition {
    id: string;
    title: string;
    img: string;
    organizer: string;
    teamSize: number;
    tags: string[];
    desc: string;
    link: string;
    detail: string;
  }

  let selected: Competition | null = null;

  const competitions: Competition[] = [
    {
      id: "mrta",
      title: "MRTA Innovation Camp 2025",
      img: "/images/MRT.jpg",
      organizer: "การรถไฟฟ้าขนส่งมวลชนแห่งประเทศไทย (รฟม.)",
      teamSize: 4,
      tags: ["ม.ปลาย", "ปวช.", "ทีม 4 คน"],
      desc: "พัฒนาไอเดียเพื่อบริการ MRT ดีขึ้น",
      link: "https://bit.ly/MRTA2025_Register",
      detail: `เปิดรับสมัครแล้ว! โครงการ MRTA Innovation Camp 2025! 🚆\n\nรฟม. ชวนชาวรุ่นใหม่หัวใจนวัตกรรมมาร่วมคิด สร้าง และลงมือทำภายใต้โจทย์\n\"เราจะสร้างนวัตกรรมหรือแนวคิดใหม่ ที่ยกระดับบริการรถไฟฟ้า MRT และสร้างประโยชน์ต่อสังคม สิ่งแวดล้อม และ รฟม. ได้อย่างไร?\"\n\n✏️ คุณสมบัติผู้สมัคร:\n- นักเรียนระดับมัธยมศึกษาตอนปลาย\n- นักศึกษาระดับประกาศนียบัตรวิชาชีพ (ปวช.)\n- สมัครเป็นทีม ทีมละ 4 คน\n\n🎓 สิ่งที่น้องๆ จะได้รับ:\n✅ Design Thinking Workshop\n✅ เปิดโลกนวัตกรรมติวเข้ม\n✅ เทคนิคการนำเสนอผลงาน\n\n🏆 ชิงเงินรางวัลรวมกว่า 55,000 บาท!!\n📌 วันที่กิจกรรม: 2-3 ส.ค., 9 ส.ค., 16 ส.ค. 2568\n📝 สมัครได้ถึง 18 ก.ค. 2568 เวลา 23:00 น.\n📣 ประกาศผล 25 ก.ค. 2568\n🌐 รายละเอียดเพิ่มเติม >> https://bit.ly/MRTAInnoCamp`,
    },
    {
      id: "quickathon",
      title: "Quickathon #4",
      img: "/images/Quickathon.jpg",
      organizer: "RISE Academy",
      teamSize: 1,
      tags: ["ป.ตรี", "อายุ 18–25 ปี", "เดี่ยว"],
      desc: "Hackathon 3 ชั่วโมง สร้างไอเดียเพื่อผู้สูงอายุ",
      link: "https://docs.google.com/forms/d/e/1FAIpQLSdAcrwprDjov-3FAxf_w0bnqVeZcNgf5lkzg0YcQJyMOCjrBA/viewform?usp=header",
      detail: `กลับมาอีกครั้งกับ Quickathon #4!\nเวทีระดมไอเดียธุรกิจสุดเข้มข้นในเวลาเพียง 3 ชั่วโมง!\n🔥 ครั้งนี้เราท้าคุณมาคิดไอเดียธุรกิจ เพื่อให้ ผู้สูงอายุสามารถมีงานทำ และมีรายได้อย่างยั่งยืน\n💡 คิด – ต่อยอด – นำเสนอจริงต่อหน้าคณะกรรมการจากวงการธุรกิจและนวัตกรรม\n📌 สิ่งที่คุณจะได้รับ\n✅ เวิร์กชอปที่ช่วยให้คุณกล้าท้าชน Hackathon ทั่วประเทศ\n✅ ประสบการณ์ Hackathon ภายใน 3 ชั่วโมง\n✅ ฝึก Pitching และได้ใบ Certificate\n✅ เจอเพื่อนใหม่จากหลายมหาวิทยาลัย\n📂 เปิดรับเฉพาะนิสิต–นักศึกษาระดับปริญญาตรี (อายุ 18–25 ปี)\n🗓️ สมัครได้ถึงวันที่ 25 กรกฎาคม 2568 เวลา 23.59 น.\n📣 ประกาศผลคัดเลือก: 26 กรกฎาคม 2568 ทางอีเมล\n📍 จัดกิจกรรมวันที่ 9 ส.ค. 2568 ที่ Clazy Cafe, The Seasons Mall (BTS สนามเป้า ทางออก 1)\n🕗 เวลา 08.00 – 17.30 น.\n📍 Elevator Pitching Day: 10 ส.ค. 2568 เวลา 08.00 – 12.00 น.\n💸 ผู้ผ่านคัดเลือกต้องวางมัดจำ 500 บาท (คืนหลังจบงาน)\n📌 รับจำนวนจำกัด`,
    },
    {
      id: "bigcseed2025",
      title: "BIG’s SEED Talent Camp 2025",
      img: "/images/bigc.jpg",
      organizer: "Big C SuperCenter",
      teamSize: 5,
      tags: ["ป.ตรี", "ปี 1–4", "ทีม 5 คน", "ทุกมหาวิทยาลัย"],
      desc: "Hackathon ธุรกิจโจทย์จริงจาก Big C ชิงรางวัล 190,000 บาท",
      link: "https://forms.gle/hdK8STXB4JHJS9EV9",
      detail: `🌱 Now, Your Turn! กลับมาอีกครั้งกับ Hackathon สุดปังจาก Big C!\n💚 BIG’s SEED Talent Camp 2025: The Next Gen Hackathon\nเวทีสร้างสรรค์ไอเดียธุรกิจ สำหรับนิสิต นักศึกษา ป.ตรี ปี 1-4 ทุกมหาวิทยาลัย\nชิงรางวัลรวมกว่า 190,000 บาท พร้อมโอกาสลับสมอง ลับไอเดีย\nกับโจทย์จริงจาก Big C Supercenter! 🧠💼\n👥 รวมทีม 5 คน ต่างมหาวิทยาลัย ต่างคณะ ต่างชั้นปีได้\n🗓️ สมัครได้ตั้งแต่วันนี้ – 4 สิงหาคม 2568\n🔗 สมัครเลยที่: https://forms.gle/hdK8STXB4JHJS9EV9\n🚀 มาปลดล็อกพลังความคิด และเริ่มต้นเส้นทางนักธุรกิจรุ่นใหม่กับเรา!`,
    },
  ];


  function goBack() {
    goto('/parasite/home');
  }

  onMount(() => {
  $: document.body.style.overflow = selected ? 'hidden' : '';
});

</script>

<NavBar currentPage="competition" />

<main class="container">
  <button class="back-btn" on:click={goBack}>
    <img src="/icons/back.png" alt="back" />
    <span>ย้อนกลับ</span>
  </button>

  <div class="header">
    <h2>🏆 Competition Opportunities</h2>
    <p class="subtitle">รวมการแข่งขันน่าสนใจที่เปิดรับสมัครตอนนี้</p>
  </div>

  <div class="list">
    {#each competitions as c}
      <div class="card">
        <img class="thumb" src={c.img} alt={c.title} />
        <div class="info">
          <h3>{c.title}</h3>
          <p class="desc">{c.desc}</p>
          <p><strong>จัดโดย:</strong> {c.organizer}</p>
          <p><strong>ทีม:</strong> {c.teamSize} คน</p>
          <div class="tags">
            {#each c.tags as tag}
              <span class="tag">{tag}</span>
            {/each}
          </div>
          <button class="details-btn" on:click={() => selected = c}>ดูรายละเอียดเพิ่มเติม</button>
        </div>
      </div>
    {/each}
  </div>

  {#if selected}
    <div class="popup">
      <div class="popup-box">
        <img class="popup-img" src={selected.img} alt={selected.title} />
        <h3>{selected.title}</h3>
        <pre>{selected.detail}</pre>
        <a href={selected.link} class="apply-btn" target="_blank">สมัครที่นี่</a>
        <button class="close-btn" on:click={() => selected = null}>ปิด</button>
      </div>
    </div>
  {/if}
</main>

<style>
  .container {
    padding: 6rem 1.5rem 4rem;
    background: linear-gradient(135deg, #e0f7fa, #fefefe);
    min-height: 100vh;
    font-family: 'Noto Sans', sans-serif;
  }
  .back-btn {
    position: absolute;
    top: 5.5rem; left: 1.5rem;
    display: flex; align-items: center; gap: 0.4rem;
    background: #eef3ff; border: 1px solid #cbd5e1;
    border-radius: 999px; padding: 0.5rem 1rem;
    font-size: 0.9rem; cursor: pointer;
    box-shadow: 0 2px 4px rgba(0,0,0,0.05);
    z-index: 10;
  }
  .back-btn img { width: 18px; height: 18px; }

  .header { text-align: center; margin: 2rem 0 1rem; }
  .header h2 { font-size: 1.8rem; color: #333; }
  .subtitle { font-size: 0.95rem; color: #666; }

  .list { display: flex; flex-direction: column; gap: 1.5rem; max-width: 800px; margin: 0 auto; }

    .card {
    display: flex;
    background: white;
    border-radius: 16px;
    box-shadow: 0 4px 12px rgba(0,0,0,0.08);
    overflow: hidden;
  }

  .thumb {
    width: 130px;
    object-fit: cover;
    margin: 1rem;
    border-radius: 12px;
  }

  .info { padding: 1rem 1.2rem; flex: 1; }

  h3 {
    margin: 0;
    font-size: 1.2rem;
    color: #222;
  }

  .desc { color: #555; font-size: 0.9rem; margin: 0.3rem 0; }

  .tags { margin-top: 0.4rem; }
  .tag {
    display: inline-block;
    background: #dff3f9;
    color: #00695c;
    border-radius: 12px;
    padding: 0.3rem 0.6rem;
    font-size: 0.8rem;
    margin-right: 0.4rem;
  }

  .details-btn {
    background: #00796b;
    color: white;
    padding: 0.4rem 1rem;
    border: none;
    border-radius: 8px;
    margin-top: 0.6rem;
    cursor: pointer;
    font-weight: 600;
  }

  .popup {
    position: fixed;
    top: 0;
    left: 0;
    width: 100vw;
    height: 100vh;
    background: rgba(0, 0, 0, 0.4);
    display: flex;
    align-items: flex-start;
    justify-content: center;
    padding-top: 6rem;
    z-index: 9999;
    overflow: auto;
  }

  .popup-box {
    background: white;
    padding: 2rem;
    border-radius: 16px;
    width: 90%;
    max-width: 650px;
    box-shadow: 0 4px 16px rgba(0, 0, 0, 0.2);
    margin-bottom: 10rem;
  }

  .popup-img {
    width: 100%;
    height: auto;
    display: block;
    border-radius: 12px;
    margin-bottom: 1rem;
  }

  pre {
    white-space: pre-wrap;
    font-size: 0.95rem;
    color: #333;
    margin-bottom: 1rem;
  }

  .apply-btn {
    display: inline-block;
    background: linear-gradient(135deg, #00897b, #00c1a4);
    color: white;
    padding: 0.5rem 1rem;
    border-radius: 8px;
    text-decoration: none;
    font-weight: 600;
    margin-bottom: 1rem;
  }

  .close-btn {
    background: #ccc;
    border: none;
    padding: 0.4rem 1rem;
    border-radius: 8px;
    cursor: pointer;
  }
</style>
