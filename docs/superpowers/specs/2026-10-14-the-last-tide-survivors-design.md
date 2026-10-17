# The Last Tide (web survivors): Thiết kế

- **Ngày:** 2026-10-14
- **Trạng thái:** chờ duyệt bản viết
- **Tài liệu tham khảo:** [idea.md](../../../idea.md). Đây là bản ý tưởng SLG ban đầu, chỉ để tham khảo. Khi idea.md mâu thuẫn với spec này thì làm theo spec.

## 1. Mục tiêu

Làm một game web **một người chơi**, thể loại **sinh tồn đánh quái theo đợt** (dòng Survivor.io / Vampire Survivors), dùng theme The Last Tide (thế giới ngập nước, năm 2042). Phát hành trên CrazyGames và các portal web khác, kiếm tiền bằng quảng cáo.

Game cũng dùng để kiểm chứng theme, pipeline art bằng AI và khả năng vận hành của một người làm, trước khi tính tới bản SLG.

**Thước đo thành công:**
- Qua Basic Launch của CrazyGames để lên Full Launch.
- Playtime trung bình ≥ 10 phút.
- D1 ≥ 15–20%.
- Bản đầu ra trong khoảng 4–6 tuần.

**Không làm ở bản đầu:**
- Backend, tài khoản, IAP, multiplayer.
- Chế độ khó, chế độ vô tận, nhiệm vụ hằng ngày, skin, leaderboard online.

## 2. Vòng chơi

### 2.1 Một lượt chơi (một chương, khoảng 8 phút)

- **Điều khiển:** lái bè bằng WASD/phím mũi tên trên máy tính, joystick ảo trên mobile. Màn hình ngang 16:9.
- **Vũ khí:** tự bắn vào mục tiêu theo luật riêng của từng vũ khí.
- **Quái chết rơi ra:**
  - **XP:** đủ XP thì lên cấp. Game dừng lại cho chọn **1 trong 3 thẻ**: vũ khí mới, nâng cấp vũ khí đang có, hoặc chỉ số phụ.
  - **Scrap:** tiền dùng ngoài lượt chơi.
- **Giới hạn trang bị:** tối đa **6 vũ khí và 6 chỉ số phụ**.
- **Tiến hóa:** vũ khí đạt cấp tối đa và đang có chỉ số phụ tương ứng thì có thể tiến hóa thành vũ khí mạnh hơn.
- **Thùng hàng trôi nổi:** hồi máu, nam châm (hút toàn bộ XP trên màn), bom (dọn quái trên màn).
- **Nhịp chương:** mật độ quái tăng dần theo timeline. Phút 4 có boss nhỏ, phút 8 có boss cuối. Hạ boss cuối là qua chương.
- **Nguy hiểm môi trường:** mỗi chương có một loại riêng (xem mục 3.4).
- **Khi chết:**
  - Được hồi sinh **1 lần mỗi lượt** nếu xem rewarded ad.
  - Không xem thì kết thúc lượt, nhưng **vẫn giữ toàn bộ scrap đã nhặt**.
- **Màn kết quả:** hiển thị scrap kiếm được, kèm nút xem rewarded ad để **nhân đôi scrap**.

**Luật rút thẻ:**
- Rút có trọng số, dùng RNG có seed.
- 3 lần lên cấp đầu tiên luôn có ít nhất 1 thẻ vũ khí.
- Đã đủ 6 vũ khí thì không ra thẻ vũ khí mới. Đã đủ 6 chỉ số phụ thì không ra thẻ chỉ số phụ mới.
- Món đã đạt cấp tối đa thì không ra thẻ của món đó nữa.
- Nếu không còn thẻ nào hợp lệ thì ra thẻ dự phòng: "+scrap" hoặc "hồi máu".

### 2.2 Ngoài lượt chơi: màn hình Platform (menu chính)

- **Nâng cấp vĩnh viễn bằng scrap**, gồm 6 chỉ số: máu, sát thương, tốc độ, phạm vi hút, lượng scrap nhận được, giáp. Mỗi chỉ số có nhiều cấp, giá tăng dần.
- **Chọn thuyền trưởng** và **chọn chương** (chỉ chọn được chương đã mở khóa).
- Midgame ad hiện khi quay về Platform sau một lượt chơi. Không hiện sau lượt chơi đầu tiên của phiên.

## 3. Nội dung bản đầu

Mọi tên và con số là bản nháp. Số liệu được chỉnh bằng công cụ mô phỏng cân bằng (mục 4.6).

### 3.1 Vũ khí (8 loại, mỗi loại 5 cấp)

| id | Tên | Cách hoạt động |
|---|---|---|
| `harpoon` | Harpoon Gun | Bắn lao thẳng về quái gần nhất, xuyên qua N quái |
| `scrap_saw` | Scrap Saw | Lưỡi cưa quay quanh bè |
| `water_cannon` | Water Cannon | Phun nước hình nón theo hướng di chuyển, đẩy lùi quái |
| `flare_mortar` | Flare Mortar | Bắn vào cụm quái đông nhất, nổ thành vùng |
| `depth_charge` | Depth Charge | Thả mìn phía sau bè, nổ khi có quái chạm vào |
| `tesla_buoy` | Tesla Buoy | Thả phao phát sét, sét lan sang N quái |
| `scout_drone` | Scout Drone | Drone bay quanh và tự đi tìm quái để đánh |
| `net_launcher` | Net Launcher | Bắn lưới làm chậm quái trong vùng |

### 3.2 Chỉ số phụ (6 loại, mỗi loại 5 cấp)

| id | Tên | Hiệu ứng |
|---|---|---|
| `hull_plating` | Hull Plating | Giáp |
| `engine` | Engine | Tốc độ di chuyển |
| `magnet` | Magnet | Phạm vi hút XP |
| `ammo_crate` | Ammo Crate | Tốc độ bắn |
| `scope` | Scope | Tầm bắn và kích thước vùng ảnh hưởng |
| `desalinator` | Desalinator | Hồi máu theo thời gian |

### 3.3 Tiến hóa (bản đầu có 3)

| Vũ khí cấp tối đa | Chỉ số phụ cần có | Kết quả |
|---|---|---|
| `harpoon` | `scope` | `leviathan_lance` |
| `scrap_saw` | `engine` | `maelstrom_saw` |
| `tesla_buoy` | `ammo_crate` | `storm_beacon` |

### 3.4 Quái, boss, chương

**Quái thường:**

| id | Hành vi |
|---|---|
| `drowned` | Bơi chậm, đi thành đám |
| `piranha` | Nhanh, máu ít, đi theo bầy |
| `jellyfish` | Khi chết để lại vùng độc |
| `shark` | Lao thẳng tới bè rồi nghỉ một lúc |
| `crab_tank` | Máu trâu, di chuyển chậm |
| `raider_skiff` | Giữ khoảng cách và bắn từ xa |
| `drift_mine` | Trôi lững lờ, nổ khi chạm vào |
| `mutant_eel` | Lượn ngoằn ngoèo |
| `gull_flock` | Bay ngang qua màn hình theo đường thẳng |

**Boss:**

| id | Tên |
|---|---|
| `hermit_leviathan` | Cua khổng lồ |
| `raider_warlord` | Tàu cướp biển |
| `the_maw` | Quái vật biển sâu |

**Chương:**

| Chương | Bối cảnh | Nguy hiểm môi trường | Boss nhỏ (phút 4) | Boss cuối (phút 8) |
|---|---|---|---|---|
| 1 | Drowned Harbor | Mảnh vỡ trôi chặn đường | `crab_tank` phiên bản lớn | `hermit_leviathan` |
| 2 | Oil Rig Field | Vệt dầu cháy lan rộng | `raider_skiff` phiên bản lớn | `raider_warlord` |
| 3 | The Red Ocean | Vùng nước độc dịch chuyển | `hermit_leviathan` | `the_maw` |

### 3.5 Thuyền trưởng (3 người)

| id | Tên | Vũ khí khởi đầu | Passive | Mở khóa |
|---|---|---|---|---|
| `rook` | Rook, cựu lính thủy | `harpoon` | +10% sát thương | Có sẵn |
| `maya` | Maya Chen, nhà sinh vật biển | `water_cannon` | Hồi máu theo thời gian | Mua bằng scrap |
| `tavi` | Tavi, kỹ sư | `scout_drone` | +15% scrap | Qua chương 1 |

## 4. Kiến trúc

**Stack:** TypeScript, Phaser 3, Vite, vitest. Deploy lên Cloudflare Workers, phát hành trên itch.io và CrazyGames. Pipeline build dùng lại pattern từ repo billiard.

```
input ──► intent {moveX, moveY} ──► sim/ (TS thuần, fixed 60Hz, seeded RNG)
content/*.json ──────────────────────►   │            │
                                state (read-only)   events
                                         ▼            ▼
                                 render/ (Phaser)   audio, analytics
```

### 4.1 `src/sim/`: lõi game, không import Phaser và không dùng API của trình duyệt

- **`World`** chứa toàn bộ state của một lượt chơi. Quái, đạn và vật phẩm được lấy từ **object pool**.
- **Bước thời gian cố định 1/60 giây.** Mỗi frame, `dt` thật được cộng dồn và chạy tối đa 5 bước để chống "spiral of death". Khi tab bị treo lâu rồi quay lại, phần thời gian dư bị bỏ đi.
- **RNG có seed** (ví dụ mulberry32). Cùng seed và cùng chuỗi intent thì cho cùng kết quả.
- **System là các hàm thuần**, chạy theo thứ tự cố định: `spawner` → `movement` → `weapons` → `projectiles` → `collision` → `damage` → `pickups` → `leveling` → `hazards` → `boss` → `cleanup`.
- **`spatialGrid`** (spatial hash) dùng cho truy vấn quái gần nhất và kiểm tra va chạm. Mục tiêu hiệu năng: 300–500 quái chạy 60fps trên mobile tầm trung.
- **Vũ khí:** mỗi vũ khí là một behavior đăng ký theo `id`, cài theo interface `WeaponBehavior { update(world, weapon, dt) }`. Chỉ số của từng cấp lấy từ JSON.
- **Sự kiện gửi ra ngoài:** sim ghi vào hàng đợi các sự kiện như `enemyKilled`, `playerHit`, `levelUp`, `bossSpawned`, `runEnded`. Render, audio và analytics đọc từ hàng đợi này.
- **Lên cấp:** khi lên cấp, sim chuyển sang trạng thái `choosingCard` và dừng tick cho tới khi nhận được lựa chọn thẻ.

### 4.2 `src/content/`: dữ liệu game

- Các file JSON: `chapters`, `enemies`, `bosses`, `weapons`, `passives`, `evolutions`, `captains`, `metaUpgrades`.
- Mỗi chương có **timeline sinh quái**: danh sách các mốc `{ từ giây, đến giây, loại quái, tốc độ sinh, kiểu sinh }`.
- Kiểu dữ liệu TypeScript cho từng file, kèm **test kiểm tra dữ liệu**: id không trùng, mọi tham chiếu đều trỏ tới id có thật, cấp và giá trị nằm trong khoảng hợp lệ.

### 4.3 `src/meta/`: tiến trình ngoài lượt chơi

- `SaveData` có `version` kèm chuỗi hàm `migrate`. Nội dung save: scrap, cấp các nâng cấp vĩnh viễn, thuyền trưởng đã mở, chương đã mở, chỉ số thống kê, cài đặt (âm lượng, ngôn ngữ).
- Logic cửa hàng (mua nâng cấp, kiểm tra đủ scrap) và mở khóa là hàm thuần, có test.
- Lúc bắt đầu lượt chơi, chỉ số vĩnh viễn và passive của thuyền trưởng được gộp thành chỉ số khởi đầu, truyền vào `World`.

### 4.4 `src/render/`: Phaser

- **Scene:** `Boot` (tải asset), `Platform` (menu chính), `Run`, lớp phủ `LevelUp`, `Results`, `Pause`.
- Mỗi entity trong sim có một sprite tương ứng, ghép theo id và lấy từ pool. Vị trí được nội suy giữa hai tick.
- **Hình ảnh:** gom vào **texture atlas** và tham chiếu qua `asset manifest`. Mỗi id nội dung trỏ tới một texture key. Lúc chưa có art thì dùng hình khối tạm vẽ bằng `Graphics`.
- **Animation:** dùng tween (nhún, lắc, xoay, co giãn). Hiệu ứng nổ, bọt nước, tia sét dùng particle. Boss được ghép từ nhiều phần.
- **Mặt biển:** dùng texture nước lặp liền mạch cuộn chậm. Texture này **vẽ bằng code hoặc shader**, không dùng AI.
- **HUD:** thanh máu, thanh XP, đồng hồ, số scrap, icon vũ khí và chỉ số phụ, joystick ảo trên thiết bị cảm ứng.
- **Tự dừng game** khi chuyển tab (`visibilitychange`) hoặc cửa sổ mất focus.

### 4.5 `src/platform/`: lớp kết nối với nơi phát hành

```ts
interface Platform {
  init(): Promise<void>;
  loadingStart(): void; loadingStop(): void;
  gameplayStart(): void; gameplayStop(): void;
  showMidgame(): Promise<void>;
  showRewarded(): Promise<boolean>; // true = xem hết, được nhận thưởng
  load(): Promise<string | null>; save(data: string): Promise<void>;
}
```

- **`crazygames`:** dùng CrazyGames HTML5 SDK (v3). Save qua module data của SDK, nếu không dùng được thì chuyển sang localStorage. Trong lúc chạy quảng cáo thì dừng game và tắt tiếng.
- **`web`:** không có quảng cáo. `showRewarded` trả về `true`, để bản chơi thử nhận thưởng mà không phải xem quảng cáo. Save bằng localStorage. Đo lường bằng PostHog nếu có `VITE_POSTHOG_KEY`.
- **Chọn bản lúc build bằng `VITE_TARGET`.** Bản CrazyGames không có link ra ngoài và không có PostHog.

### 4.6 `tools/balance-sim.ts`: mô phỏng cân bằng

- Chạy sim không cần giao diện, với một **bot**: chạy vòng quanh để tránh quái, ưu tiên nhặt XP, chọn thẻ theo một chiến lược cố định.
- Tham số chạy: số seed, chương, thuyền trưởng, cấp nâng cấp vĩnh viễn.
- **Output:** tỷ lệ sống sót, thời điểm chết, cấp đạt được theo phút, DPS theo phút, scrap kiếm được.
- **Mục tiêu cân bằng ban đầu:**
  - Chương 1 với 0 nâng cấp: bot sống tới khoảng phút 5–7.
  - Chương 1 sau 2–3 lượt nâng cấp: bot qua được chương.

## 5. Đồ họa và âm thanh

- **Style:** cartoon 2D nét đậm (viền đen, màu phẳng, hơi chibi). Quái, bè, đạn nhìn từ trên xuống. Chân dung thuyền trưởng nhìn chính diện, tới ngang ngực.
- **Tạo ảnh:**
  - Dùng **Kaggle Notebooks** có GPU miễn phí, chạy **SDXL base 1.0** (license CreativeML OpenRAIL++-M, cho phép dùng thương mại) qua thư viện diffusers. Không dùng Flux.1 [dev].
  - Giữ style đồng nhất bằng IP-Adapter với 3–5 ảnh mẫu đã duyệt. Nếu style vẫn bị lệch thì mới train LoRA.
  - Input chuẩn bị sẵn ở `art/` (xem `docs/MANUAL-TASKS.md`).
- **Xử lý ảnh:** script `tools/art/` chạy trên máy: tách nền bằng rembg → cắt viền thừa → resize → đóng gói atlas bằng free-tex-packer. Quy trình: `art/raw/` → `art/processed/` → `public/assets/`.
- **Âm thanh:** hiệu ứng lấy từ freesound.org (CC0) và tạo bằng jsfxr. Nhạc nền lấy từ OpenGameArt (CC0) hoặc Pixabay. Không dùng gói free của Suno hay Udio.
- **`CREDITS.md`** ghi nguồn và license của mọi asset.

## 6. Kiếm tiền, đo lường, phát hành

**Quảng cáo** (chỉ có ở bản CrazyGames):
- Rewarded ad dùng cho hồi sinh và nhân đôi scrap.
- Midgame ad hiện khi về Platform. Không hiện ở lượt đầu tiên, và tôn trọng giới hạn tần suất của SDK.
- Nếu quảng cáo lỗi hoặc không có quảng cáo: hiện thông báo nhẹ nhàng, không cho thưởng, game vẫn chạy tiếp bình thường.

**Sự kiện gửi lên PostHog** (bản web/itch):

| Sự kiện | Thuộc tính |
|---|---|
| `run_started` | `chapter`, `captain` |
| `run_ended` | `result`, `duration_s`, `level`, `kills`, `death_cause`, `scrap` |
| `card_picked` | `card_id`, `level` |
| `chapter_cleared` | `chapter` |
| `upgrade_bought` | `upgrade_id`, `level` |
| `ad_requested` / `ad_completed` | `placement` |

Mọi sự kiện đều kèm `platform` và `app_version`.

**Yêu cầu cho portal:**
- Mặc định tiếng Anh, có thêm tiếng Việt.
- Không có link ra ngoài.
- Bản build ban đầu dưới 15MB.
- Chạy được trên máy tính và mobile, xử lý đúng khi đổi kích thước cửa sổ.
- Bắt lỗi toàn cục. Asset tải lỗi thì thử tải lại.

**Lộ trình phát hành:**
1. Link Cloudflare và itch.io để chơi thử.
2. CrazyGames Basic Launch.
3. Đọc số liệu và sửa game.
4. Full Launch.
5. Sau đó nộp Poki và GameDistribution.

## 7. Xử lý lỗi

| Tình huống | Cách xử lý |
|---|---|
| Đọc hoặc ghi save lỗi | Bọc try/catch. Lần ghi save mới không ghi đè lên bản dự phòng |
| Save hỏng | Khôi phục từ bản dự phòng. Hỏng cả hai thì tạo save mới và ghi log, không crash |
| `dt` quá lớn (tab bị treo lâu) | Giới hạn số bước chạy mỗi frame, bỏ phần thời gian dư |
| SDK khởi tạo lỗi | Chuyển sang dùng bản `web` (không quảng cáo, save bằng localStorage) |
| Lỗi không bắt được | Handler toàn cục. Bản web gửi lỗi lên PostHog. Game hiện thông báo "Có lỗi, tải lại" |

## 8. Test

- **vitest cho sim:** va chạm, sát thương, xuyên mục tiêu, nhặt vật phẩm, lên cấp, luật rút thẻ, tiến hóa, timeline sinh quái, kết quả giống hệt nhau khi chạy lại cùng seed.
- **vitest cho meta:** mua nâng cấp, mở khóa, migrate save, khôi phục save hỏng.
- **vitest cho content:** kiểm tra dữ liệu như mục 4.2.
- **Smoke test:** chạy balance-sim với vài seed ngắn trong `npm test`, đảm bảo không lỗi và không vượt ngân sách thời gian.
- **Render:** kiểm tra bằng cách chơi thử theo checklist trong `docs/MANUAL-TASKS.md`.

## 9. Mốc

| Mốc | Tuần | Nội dung |
|---|---|---|
| **M1** | 1–2 | Khung dự án và lõi sim. Chương 1 chơi được bằng hình tạm. Có balance-sim |
| **M2** | 3–4 | Đủ 3 chương, 8 vũ khí, 3 tiến hóa, boss, nguy hiểm môi trường, meta, save |
| **M3** | 5–6 | Thay art và âm thanh, CrazyGames SDK, i18n, đánh bóng, đóng gói lên portal |

Art làm song song từ M1, xem `docs/MANUAL-TASKS.md`.
