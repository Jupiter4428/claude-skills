# Skills ของ cntagram: ใช้ตัวไหน ตอนไหน (Claude Code)

skill ทั้งหมดอยู่ใน `.claude/skills/` ซึ่ง Claude Code โหลดให้เองเมื่อเปิดโปรเจคนี้ พิมพ์ `/` เพื่อดูรายชื่อทั้งหมด

skill มี 2 แบบ
- **อัตโนมัติ**: Claude เลือกใช้เองจาก description หรือจะพิมพ์ `/ชื่อ` เรียกเองก็ได้
- **🔒 ต้องพิมพ์เอง**: Claude เริ่มให้ไม่ได้ ต้องพิมพ์ `/ชื่อ` (ตามด้วยข้อความได้ เช่น `/handoff ไปทำ US-24 ต่อ`)

---

## 1. skill ที่รวมกันแล้ว

| เดิม | รวมเป็น | ได้อะไรเพิ่ม |
|---|---|---|
| `debug-mantra` (9arm) + `diagnosing-bugs` | **`diagnosing-bugs`** | กฎ 4 ข้อคุมตลอด session, ทดสอบ disproof ก่อน, **experiment ledger** (บันทึกทุกการรัน แล้วเช็คสมมติฐานกับทุกครั้ง), การไล่ knob ทีละตัว, วิธี debug Django (`breakpoint()`, `manage.py shell`) และรองรับตอนวาง stack trace มาตรงๆ |
| `scrutinize` (9arm) + `code-review` | **`code-review`** (3 แกน) | เพิ่มแกน **Scrutiny**: มีวิธีที่ง่ายกว่าไหม, trace path จริง URL → view → service → model → template, คำตัดสิน ship / fix-then-ship / rework / reject และ review **แผนหรือ design doc** ที่ยังไม่มีโค้ดได้ |
| `karpathy-guidelines` | **`CLAUDE.md`** ส่วน "Coding behaviour" | เป็นกฎที่ต้องมีผลทุกครั้ง ใส่ใน CLAUDE.md จึงบังคับใช้เสมอโดยไม่ต้องรอให้ skill ทำงาน |
| `grill-me` + `grilling` + `grill-with-docs` | ใช้ **`/grill-with-docs`** ตัวเดียว | `grill-me` ไม่ได้ติดตั้ง เพราะในโปรเจคที่มี repo ตัว `grill-with-docs` ทำได้มากกว่า ส่วน `grilling` ติดตั้งไว้เพราะ `grill-with-docs` เรียกใช้ (เดิมไม่มี จึงพังอยู่) |

**ไม่ได้ติดตั้ง:**
- `qwen-agent`: ต้องใช้คำสั่ง `claude-9arm` ซึ่งเครื่องนี้ไม่มี ให้ใช้ subagent ของ Claude Code แทน (ดูข้อ 6)
- `teach`, `misc/*`: `teach` ไม่เกี่ยวกับโปรเจคนี้ ส่วน `misc/*` ทำมาสำหรับ Node/TS ใช้กับ Django ไม่ได้

---

## 2. skill ที่ดูซ้ำกันแต่รวมไม่ได้: แต่ละสถานการณ์ต้องทำยังไง

### `/implement` vs `/implement-spec` 🔒
ทั้งสองตัวสร้างโค้ดจาก ticket แต่**ผู้ควบคุมงาน**ต่างกัน
- **ทำ US ทีละอัน แล้วอยากดูเองทุกขั้น** (กรณีส่วนใหญ่ของ sprint):
  ```
  /implement #24
  ```
  พอเสร็จแต่ละ ticket ให้ `/clear` แล้วค่อยเริ่ม ticket ถัดไป
- **spec ใหญ่ที่มีหลาย ticket ไม่ผูกกัน และอยากให้ Claude จัดการเอง:**
  ```
  /implement-spec #30
  ```
  Claude จะแตก subagent ทำ ticket ที่พร้อมแบบขนาน แล้วรวมทุกอย่างไว้ใน integration branch เดียว

### `post-mortem` vs `management-talk`
ทั้งสองตัวสรุปงาน แต่**ผู้อ่าน**ต่างกัน ปกติใช้ต่อกัน
1. แก้บั๊กเสร็จและ test ผ่านแล้ว → "เขียน post-mortem ของบั๊กนี้"
   - ได้ RCA สำหรับวิศวกร ระบุไฟล์และโค้ดได้
   - skill จะ**ปฏิเสธ**ถ้ายังไม่มี repro, root cause หรือ fix ที่ validate แล้ว
2. ต่อจากนั้น → "เขียนเวอร์ชันสำหรับ PM / อาจารย์ เป็นข้อความ Slack (หรือ email / standup)"
   - `management-talk` ตัดศัพท์เทคนิคออก แล้วจัดรูปให้เข้ากับช่องทางนั้น

### `/handoff` 🔒 vs `qwenchance` vs คำสั่งของ Claude Code
ต้นเหตุต่างกัน: `qwenchance` ทำงานเอง**เมื่อ Claude เริ่มวน**หรือ context ใกล้เต็ม แล้ว**แนะนำ**ให้คุณพิมพ์คำสั่ง เพราะ Claude พิมพ์ `/compact` หรือ `/handoff` เองไม่ได้ ส่วนจะใช้คำสั่งไหน ให้ดูตามสถานการณ์

| สถานการณ์ | พิมพ์ |
|---|---|
| ทำงานต่อใน repo เดิม แต่ context เริ่มเยอะ | `/compact เรากำลังจะ QA หน้า dashboard` ← **ใช้บ่อยที่สุด** |
| งานเก่าใช้ต่อไม่ได้แล้ว | `/clear` (ย้อนกลับมาได้ด้วย `/resume`) |
| ส่งงานให้เพื่อน / ย้ายไป repo อื่น / ไปใช้เครื่องมืออื่น / แยก side task | `/handoff <session ถัดไปจะทำอะไร>` |
| อยากรู้ว่า context เหลือเท่าไร | `/context` |
| Claude วน อ่านไฟล์เดิมซ้ำ | พิมพ์ "วนแล้ว" ให้ `qwenchance` ทำงาน หรือกด `Esc` เพื่อหยุดแล้วสั่งใหม่ |

**ข้อควรระวัง:** อย่า compact หรือ clear ระหว่าง `/grill-with-docs` → `/to-spec` → `/to-tickets` เพราะทั้งสามขั้นต้องใช้บทสนทนาเต็มๆ

### `code-review` (ของโปรเจค) vs `/code-review` และ `/security-review` (ที่มากับ Claude Code)
- **ตรวจงานตามมาตรฐานโปรเจค + spec + scrutiny:** พิมพ์ "review branch นี้เทียบ main" Claude จะใช้ skill ของโปรเจค ตามที่ AGENTS.md สั่งไว้
- **หาเฉพาะบั๊ก correctness แบบละเอียด:** ใช้ตัวที่มากับ Claude Code ได้ แต่ชื่อชนกัน ถ้าเมนู `/` แสดงสองตัว ให้เลือกตัวที่มีป้าย project
- **ก่อน merge งานที่แตะ login, สิทธิ์ หรือไฟล์อัปโหลด** (เช่น US-7 protected preview, US-23 hide document): `/security-review`

### `diagnosing-bugs` vs `tdd`
- **มีบั๊กแล้วแต่ยังไม่รู้สาเหตุ** → `diagnosing-bugs` (วางข้อความ error มาได้เลย)
- **รู้แล้วว่าต้องแก้อะไร หรือเป็นฟีเจอร์ใหม่** → `tdd`
- `diagnosing-bugs` ใช้ Project standards ของ `tdd` ตอนเขียน fix อยู่แล้ว

### `/grill-with-docs` 🔒 vs `prototype` vs `research` vs `/to-questionnaire` 🔒
ทั้งสี่ตัวช่วยตอบคำถามก่อนลงมือ เลือกตาม**ว่าคำตอบอยู่ที่ไหน**

| คำตอบอยู่ที่… | ใช้ |
|---|---|
| หัวคุณเอง (ต้องตัดสินใจ) | `/grill-with-docs` |
| ต้องเห็นของจริงถึงจะรู้ (state, UI) | "ทำ prototype หน้า dashboard 3 แบบ" → `prototype` |
| เอกสารภายนอก (Django docs, API) | "research เรื่อง X" → `research` (ทำงานใน background แล้วได้ไฟล์ .md ที่มีแหล่งอ้างอิง) |
| หัวคนอื่น (อาจารย์, PO) | `/to-questionnaire` |

---

## 3. Flow หลัก: User Story → merge

```
/grill-with-docs ขัดเกลา US-24 audit log      ← plan mode (Shift+Tab) ช่วยกันไม่ให้ Claude รีบเขียนโค้ด
   └ ตอบบนกระดาษไม่ได้? → prototype
/to-spec                                      ← สร้าง GitHub issue
/to-tickets                                   ← แตกเป็น sub-issue พร้อม blocking edges
/clear
/implement #<ticket>                          ← tdd + code-review ทำในตัว
"เขียน PR"                                     ← pr skill ทำงานเอง
/retro                                        ← รันก่อน /clear เพื่อปรับ CLAUDE.md, CONTRIBUTING.md และ skill ให้ดีขึ้น
```

ระหว่างทำงาน ถ้าเจอ error, check ไม่ผ่าน หรือคุณแก้ Claude แล้วต้นเหตุมาจาก skill (ขั้นตอนผิด ขาด หรืออ่านแล้วเข้าใจผิดง่าย) Claude จะแก้ `SKILL.md` ของ skill นั้นทันที แยกเป็น commit `chore(skills): ...` และบอกคุณว่าแก้อะไร (กฎอยู่ใน `AGENTS.md`) ส่วน `/retro` ยังใช้ทบทวนภาพรวมตอนจบ session

งานเล็กที่เสร็จใน session เดียว ให้ข้าม `/to-spec` กับ `/to-tickets` แล้วพิมพ์ `/implement` ต่อจากการ grill ได้เลย

## 4. Flow บั๊ก

```
วาง error หรืออธิบายอาการ           → diagnosing-bugs (ทำงานเอง)
"review fix นี้"                     → code-review (แกน Scrutiny ดูว่า fix ทำงานได้จริง)
"เขียน post-mortem"                  → post-mortem
"สรุปให้ PM ทาง Slack"               → management-talk
/retro                               → อะไรจะกันบั๊กนี้ได้? ถ้าคำตอบคือ "ไม่มี seam ให้เขียน test" → /improve-codebase-architecture
```

## 5. งานอื่นๆ

| สถานการณ์ | ทำ |
|---|---|
| issue จากคนอื่นกองเต็ม | `/triage` |
| งานใหญ่มากที่ยังมองไม่ออก (เช่น ทั้ง sprint) | `/wayfinder` |
| ว่างอยากเก็บกวาดสถาปัตยกรรม | `/improve-codebase-architecture` (ได้รายงาน HTML) |
| ศัพท์ในโดเมนสับสน (document / material / file?) | `domain-modeling` (สร้าง `CONTEXT.md` ให้) |
| ตั้ง secret ใน CI หรือ deploy (TD-1 CI) | "ทำ wizard สำหรับตั้ง GitHub secrets" → `wizard` |
| merge conflict | `resolving-merge-conflicts` |
| Claude พูดอะไรไม่รู้เรื่อง | `/wait-what` |
| ไม่รู้จะใช้ตัวไหน | `/ask-matt` |

## 6. ใช้ subagent แทน `qwen-agent`

งานจุกจิกและแคบ เช่น ค้นหาทั่ว repo, rename หลายไฟล์ หรือรัน test แล้วสรุปผล ให้สั่ง Claude ตรงๆ ว่า **"ใช้ subagent ทำ…"** หรือ **"ใช้ Explore agent หา…"**
- งานจะไปทำใน context แยก แล้วส่งกลับมาแค่ผลสรุป context หลักจึงไม่บวม
- อยากประหยัดเพิ่ม ให้ระบุ "ใช้ haiku"
