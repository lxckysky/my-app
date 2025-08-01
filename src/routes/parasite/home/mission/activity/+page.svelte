<script lang="ts">
  import { goto } from '$app/navigation';
  import NavBar from '$lib/components/NavBar.svelte';
  import { onMount } from 'svelte';

  interface Activity {
    id: string;
    title: string;
    img: string;
    organizer: string;
    location: string;
    cost: string;
    date: string;
    tags: string[];
    desc: string;
    detail: string;
    link: string;
  }

  let currentUser: any = {};
  let selected: Activity | null = null;

  onMount(() => {
    const storedUser = localStorage.getItem('currentUser');
    if (storedUser) {
      currentUser = JSON.parse(storedUser);
    }
  });

  const activities: Activity[] = [
    // 🟨 มึงเติมข้อมูลกิจกรรมตรงนี้
    {
  id: "ai-vision-camp",
  title: "AI Vision Camp 2025",
  img: "/images/AiVision.jpg", // ใส่ภาพใน static/images เองนะ
  organizer: "คณะ ICT มหาวิทยาลัยมหิดล",
  location: "คณะเทคโนโลยีสารสนเทศและการสื่อสาร ม.มหิดล ศาลายา",
  cost: "4,900 บาท (จ่ายหลังประกาศรายชื่อผู้มีสิทธิ์เข้าร่วม)",
  date: "เสาร์ 16 – อาทิตย์ 17 สิงหาคม 2568",
  tags: ["ม.3–ม.6", "ปวช.", "AI", "กิจกรรมวันเดียวจบ"],
  desc: "เปิดโลกปัญญาประดิษฐ์กับค่าย AI Vision Camp ณ ม.มหิดล!",
  detail: `🤖 ค่าย AI Vision Camp 2025  
เปิดประสบการณ์สุดเข้มข้นในโลกของปัญญาประดิษฐ์และการประมวลผลภาพ (AI + Computer Vision)  
ภายในค่ายน้อง ๆ จะได้เรียนรู้ผ่านกิจกรรม Workshop, ลงมือทำจริง และพบกับวิทยากรมืออาชีพ

📌 รูปแบบกิจกรรม:  
- จัดที่สถานที่จริง (onsite)  
- ระยะเวลา: 2 วัน 1 คืน (เสาร์ - อาทิตย์)  
- Workshop แบบจัดเต็มทั้งภาคทฤษฎีและปฏิบัติ

👤 คุณสมบัติผู้สมัคร:  
- นักเรียนระดับ ม.3 – ม.6 หรือ ปวช. (ปีการศึกษา 2568)  
- มีความสนใจในเทคโนโลยี AI / ภาพ / คอมพิวเตอร์  
- ไม่จำเป็นต้องมีพื้นฐานมาก่อน

🗓️ วันจัดกิจกรรม:  
- เสาร์ 16 – อาทิตย์ 17 สิงหาคม 2568  
- รับจำนวนจำกัดเพียง 65 คน

📅 ปิดรับสมัคร: อาทิตย์ 10 สิงหาคม 2568  
(หรือปิดทันทีเมื่อมีผู้สมัครครบตามจำนวน)

💰 ค่าใช้จ่าย: 4,900 บาท (จ่ายหลังประกาศรายชื่อผู้มีสิทธิ์)

📍 สถานที่จัด:  
คณะเทคโนโลยีสารสนเทศและการสื่อสาร  
มหาวิทยาลัยมหิดล (ศาลายา, นครปฐม)

📞 ผู้จัดกิจกรรม:  
งานบริการวิชาการและถ่ายทอดเทคโนโลยี  
คณะ ICT ม.มหิดล  
🌐 https://www.ict.mahidol.ac.th/en/event/ai-vision-camp/`,
  link: "https://www.ict.mahidol.ac.th/en/event/ai-vision-camp/"
},
{
  id: "medyssey-series",
  title: "อยากเป็นหมอ รู้ก่อนเรียน The Series",
  img: "/images/medyssey.jpg", // มึงเอารูปนี้เซฟไว้ที่ static/images/medyssey.jpg เองนะ
  organizer: "สมาคม Thai Health Tech",
  location: "ออนไลน์ (Zoom)",
  cost: "เริ่มต้น 990 บาท / เหมาจ่าย 1,990 – 2,490 บาท",
  date: "อาทิตย์ 13 กรกฎาคม – 24 สิงหาคม 2568",
  tags: ["ออนไลน์", "แพทย์", "แนะแนว", "ม.1–ม.6", "GED"],
  desc: "คอร์สแนะแนวเรียนแพทย์ 4 ตอนจบ พร้อมฝึกทำ Portfolio และเตรียมตัวสอบ MMI",
  detail: `🩺 กิจกรรมออนไลน์ “อยากเป็นหมอ รู้ก่อนเรียน The Series”  
คอร์สแนะแนวที่จัดเต็มสำหรับน้อง ๆ ที่อยากเข้าคณะแพทย์ หรือสายสุขภาพ  
พบกับแพทย์ผู้เชี่ยวชาญ ถ่ายทอดความรู้และประสบการณ์จริง พร้อมฝึกทำ Portfolio และเตรียมตัวสอบสัมภาษณ์ MMI

📌 รูปแบบกิจกรรม:  
- จัดออนไลน์ ผ่าน Zoom  
- ทุกวันอาทิตย์ เวลา 09:00 – 12:00 น.  
- รวม 4 ครั้ง: 13 ก.ค., 27 ก.ค., 10 ส.ค., 24 ส.ค. 2568

👤 คุณสมบัติผู้สมัคร:  
- ม.1 – ม.6 / เด็กซิ่ว / GED หรือเทียบเท่า (ปีการศึกษา 2568)  
- สนใจเรียนต่อแพทย์ / สายสุขภาพ

📅 วันจัดกิจกรรม:  
- อาทิตย์ 13 กรกฎาคม – 24 สิงหาคม 2568  
- จำนวนที่รับ: 300 คน  
- ปิดรับสมัคร: เสาร์ 23 สิงหาคม 2568

💰 ค่าใช้จ่าย:  
- ครั้งละ 990 บาท  
- Early Bird (4 ตอน): 1,990 บาท (ถึง 1 ก.ค.)  
- Package ปกติ 4 ตอน: 2,490 บาท

🎓 สิ่งที่ได้รับ:  
- เกียรติบัตร (เมื่อเข้าครบ 4 ตอน)  
- คลิปย้อนหลัง + เอกสารประกอบ  
- โอกาสเจอวิทยากรแพทย์ตัวจริง

📞 ผู้จัดกิจกรรม: สมาคม Thai Health Tech  
📝 สมัครที่: https://forms.gle/s8xeR7DUB684aGZ18`,
  link: "https://forms.gle/s8xeR7DUB684aGZ18"
},
{
  id: "sciday-kmitl",
  title: "นิทรรศการวันวิทยาศาสตร์ SCI & TECH @KMITL",
  img: "/images/sciday.png",
  organizer: "คณะวิทยาศาสตร์ สจล.",
  location: "คณะวิทยาศาสตร์ สถาบันเทคโนโลยีพระจอมเกล้าเจ้าคุณทหารลาดกระบัง (KMITL)",
  cost: "ฟรี",
  date: "ศุกร์ 1 – เสาร์ 2 สิงหาคม 2568",
  tags: ["วิทยาศาสตร์", "กิจกรรมในสถานที่จริง", "ม.ปลาย", "ปวช.", "Workshop", "Walk-in"],
  desc: "กิจกรรม Workshop และนิทรรศการวิทยาศาสตร์ พร้อม certificate หลังจบกิจกรรม",
  detail: `🔬 นิทรรศการวันวิทยาศาสตร์ SCI & TECH @KMITL  
กิจกรรมในสถานที่จริง จัดขึ้นเนื่องในวันวิทยาศาสตร์แห่งชาติ  
พบกับกิจกรรม Workshop, การประกวดแข่งขัน และนิทรรศการจากทุกภาควิชาในคณะวิทยาศาสตร์

📌 รายละเอียดกิจกรรม:
- จัดเต็ม 2 วัน: ศุกร์ 1 – เสาร์ 2 สิงหาคม 2568  
- Walk-in ได้เลย! มี Workshop ให้เข้าร่วม พร้อมรับเกียรติบัตร  
- รวมกิจกรรมแข่งขันเช่น:
  • ประกวดโครงงานวิทยาศาสตร์  
  • ประกวดสิ่งประดิษฐ์  
  • แข่งตอบปัญหาวิทย์  
  • แข่ง Micro:bit  
  • แข่ง E-sport GEOGUESSR  
  • Open House & Workshop กับทุกภาควิชา

👤 คุณสมบัติผู้เข้าร่วม:
- นักเรียนระดับ ม.ปลาย / ปวช. ปีการศึกษา 2568  
- ไม่จำกัดสาย ไม่จำกัดเกรด  
- Walk-in ได้โดยไม่ต้องสมัครล่วงหน้า (แต่ละกิจกรรมมีจำนวนจำกัด)

🎯 จุดประสงค์:
- เพื่อเทิดพระเกียรติ ร.4 พระบิดาแห่งวิทยาศาสตร์ไทย  
- ส่งเสริมการเรียนรู้และนวัตกรรมทางวิทยาศาสตร์  
- สร้างแรงบันดาลใจให้เยาวชนไทย

🌐 รายละเอียดกิจกรรมเพิ่มเติม: https://sciday.kmitl.ac.th/`,
  link: "https://sciday.kmitl.ac.th/"
},

  ];

  function goBack() {
    goto('/parasite/home');
  }

  $: {
    if (typeof document !== 'undefined') {
      document.body.style.overflow = selected ? 'hidden' : '';
    }
  }
</script>

<NavBar currentPage="activity" />

<main class="container">
  <button class="back-btn" on:click={goBack}>
    <img src="/icons/back.png" alt="back" />
    <span>ย้อนกลับ</span>
  </button>

  <div class="header">
    <h2>📌 กิจกรรม / ค่ายน่าสนใจ</h2>
    <p class="subtitle">รวมกิจกรรมและค่ายที่เปิดรับสมัครในช่วงนี้</p>
  </div>

  <div class="list">
    {#each activities as a}
      <div class="card">
        <img class="thumb" src={a.img} alt={a.title} />
        <div class="info">
          <h3>{a.title}</h3>
          <p class="desc">{a.desc}</p>
          <p><strong>จัดโดย:</strong> {a.organizer}</p>
          <p><strong>สถานที่:</strong> {a.location}</p>
          <p><strong>วันที่:</strong> {a.date}</p>
          <p><strong>ค่าใช้จ่าย:</strong> {a.cost}</p>
          <div class="tags">
            {#each a.tags as tag}
              <span class="tag">{tag}</span>
            {/each}
          </div>
          <button class="details-btn" on:click={() => selected = a}>ดูรายละเอียดเพิ่มเติม</button>
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
    background: linear-gradient(135deg, #f1f8e9, #fefefe);
    min-height: 100vh;
    font-family: 'Noto Sans', sans-serif;
  }

  .back-btn {
    position: absolute;
    top: 5.5rem; left: 1.5rem;
    display: flex; align-items: center; gap: 0.4rem;
    background: #e0f2f1; border: 1px solid #b2dfdb;
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
  flex-direction: row;
  padding: 1rem;
}

.thumb {
  width: 140px; /* เพิ่มจาก 130 */
  height: 180px;
  object-fit: cover;
  border-radius: 12px;
  flex-shrink: 0;
}

.info {
  flex: 1;
  padding-left: 1rem;
  display: flex;
  flex-direction: column;
  justify-content: space-between;
}


  h3 {
    margin: 0;
    font-size: 1.2rem;
    color: #222;
  }

  .desc {
  color: #555;
  font-size: 0.92rem;
  margin: 0.5rem 0;
  line-height: 1.4;
}

  .tag {
    display: inline-block;
    background: #e8f5e9;
    color: #2e7d32;
    border-radius: 12px;
    padding: 0.3rem 0.6rem;
    font-size: 0.8rem;
    margin-right: 0.4rem;
  }

  .details-btn {
    background: #388e3c;
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
    background: linear-gradient(135deg, #4caf50, #81c784);
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
  @media (max-width: 600px) {
  .card {
    flex-direction: column;
    align-items: center;
  }

  .thumb {
    width: 100%;
    height: auto;
    margin: 0;
    border-radius: 12px 12px 0 0;
  }

  .info {
    padding-left: 0;
    padding-top: 1rem;
  }
}

</style>
