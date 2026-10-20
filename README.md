# Claude Skills Collection

คลัง agent skills สำหรับ Claude Code ที่รวบรวมมาจาก 3 แหล่ง เก็บไว้ใช้ส่วนตัว (private repo)

> ⚠️ **repo นี้ต้องเป็น private เท่านั้น** เพราะ `9arm-skills/` ไม่มี license (สงวนลิขสิทธิ์โดยปริยาย) ห้ามเปิดเป็น public หรือแจกจ่ายต่อโดยไม่ได้รับอนุญาตจากเจ้าของ

## สารบัญ

| โฟลเดอร์ | เนื้อหา |
|---|---|
| [`SKILL-MAP.md`](SKILL-MAP.md) | แผนที่ว่างานแบบไหนควรใช้ skill ตัวไหน (เขียนเอง) |
| [`mattpocock_skill/`](mattpocock_skill/) | Engineering skills: grilling, spec/ticket flow, TDD, code review, domain modeling ฯลฯ |
| [`9arm-skills/`](9arm-skills/) | debug-mantra, post-mortem, scrutinize, qwen-agent, management-talk, qwenchance |
| [`andrej-karpathy-skills/`](andrej-karpathy-skills/) | karpathy-guidelines: แนวปฏิบัติลดข้อผิดพลาดที่ LLM มักทำตอนเขียนโค้ด |
| [`cntagram-skills/`](cntagram-skills/) | ชุดที่ใช้จริงในโปรเจค cntagram (Django): รวม/แก้จาก 3 แหล่งข้างบน พร้อม `AGENTS.md` ที่กำหนดว่าใช้ skill ไหนตอนไหน และกฎ tune skill ทุกครั้งที่พลาด อ่านวิธีใช้ที่ [`skills/README.md`](cntagram-skills/skills/README.md) |

เริ่มจาก [`SKILL-MAP.md`](SKILL-MAP.md) ก่อน

## Credits

skills ในแต่ละโฟลเดอร์เป็นผลงานของเจ้าของต้นฉบับ ผมแค่รวบรวมไว้ (และอาจแก้บางส่วน ดูหัวข้อ "การแก้ไข")

| โฟลเดอร์ | ต้นฉบับ | ผู้สร้าง | License | Upstream commit |
|---|---|---|---|---|
| `mattpocock_skill/` | [mattpocock/skills](https://github.com/mattpocock/skills) | [Matt Pocock](https://www.aihero.dev) | MIT — ดู [`LICENSE`](mattpocock_skill/LICENSE) | `49dd158` |
| `9arm-skills/` | [thananon/9arm-skills](https://github.com/thananon/9arm-skills) | thananon (9arm) | **ไม่มี license** — สงวนลิขสิทธิ์ | `a1fc303` |
| `andrej-karpathy-skills/` | [multica-ai/andrej-karpathy-skills](https://github.com/multica-ai/andrej-karpathy-skills) | forrestchang, อิงจากข้อสังเกตของ Andrej Karpathy | MIT (ระบุใน `plugin.json`) | `2c60614` |

ขอบคุณเจ้าของทุก repo ที่แบ่งปันงานดีๆ ครับ 🙏

## การแก้ไขจากต้นฉบับ

- `9arm-skills/skills/engineering/{debug-mantra,post-mortem,scrutinize}/` — เปลี่ยนชื่อ `SKILL.md` เป็น `<ชื่อ>_SKILL.md`
- `mattpocock_skill/skills/CLAUDE.md` — เพิ่มไฟล์ใหม่
- `cntagram-skills/` — สำเนาจาก `cntagram/.claude/skills/` และ `cntagram/AGENTS.md` (cntagram commit `5ff1cfe` + การแก้ที่ยังไม่ commit) ไม่ใช่สำเนา upstream ให้อัปเดตด้วยการ copy จากโปรเจค cntagram ทับ

## อัปเดตจาก upstream

โฟลเดอร์เหล่านี้เป็นสำเนาที่ copy มาไว้ ไม่ได้เป็น submodule ถ้าจะดึงเวอร์ชันใหม่ ให้ clone ต้นฉบับมาไว้ที่อื่นแล้ว copy ไฟล์ทับ จากนั้นอัปเดตคอลัมน์ "Upstream commit" ด้านบน
