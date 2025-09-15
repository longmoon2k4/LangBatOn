# LANGBATON SMP — Máy chủ Paper 1.21.8

Dự án này là một máy chủ Minecraft (Paper) cấu hình sẵn cho Survival (SMP) với bộ plugin tập trung vào RPG/MMO (MMOCore, MMOItems, MythicLib), quản lý bảo mật đăng nhập/chống bot (nLogin, nAntiBot), hỗ trợ giao thức (ProtocolLib) và hiển thị TAB nâng cao.

## Thông tin nhanh
- Nền tảng: Paper `1.21.8` (`versions/1.21.8/paper-1.21.8.jar`), khởi chạy qua `love.jar` (trình tải Paper/jar).
- Java: Khuyến nghị Java 21 (LTS). Trong workspace có terminal "JavaSE-21 LTS".
- Chế độ: `survival`, `difficulty=hard` (xem `server.properties`).
- Online mode: `false` (Bungee/Velocity hoặc offline mode; đang dùng nLogin để xác thực). Hãy đảm bảo cấu hình proxy/nLogin đúng để an toàn.
- Cổng: Server `25565`, Query `25565` (tắt), RCON `25575` (tắt). `server-ip` để trống.
- MOTD: Tuỳ biến màu cho "LANGBATON SMP" (xem `server.properties`).
- EULA: Đã chấp nhận (`eula=true`).

## Cấu trúc thư mục
- `start.bat` — script khởi chạy (Windows): `java -jar love.jar`.
- `love.jar` — tệp jar máy chủ (loader). Thông thường liên kết tới Paper.
- `versions/1.21.8/paper-1.21.8.jar` — file Paper cụ thể theo phiên bản.
- `config/` — cấu hình Paper toàn cục và mặc định thế giới: `paper-global.yml`, `paper-world-defaults.yml`.
- `bukkit.yml`, `spigot.yml` — cấu hình Bukkit/Spigot (được Paper áp dụng).
- `server.properties`, `eula.txt` — cấu hình máy chủ Vanilla.
- `plugins/` — plugin jars và thư mục cấu hình.
- `world/`, `world_nether/`, `world_the_end/` — dữ liệu thế giới.
- `logs/` — nhật ký chạy máy chủ (`latest.log`, các `.log.gz`).

## Plugin cài đặt
- MMO & RPG:
  - `MMOCore-1.13.1-SNAPSHOT.jar`
  - `MMOItems-6.10.1-SNAPSHOT.jar`
  - `MythicLib-dist-1.7.1-20250911.223617-39.jar`
- Tiện ích/Hệ thống:
  - `ProtocolLib.jar` — thư viện giao thức cho nhiều plugin phụ thuộc.
  - `TAB v5.2.5.jar` — tuỳ biến giao diện TAB.
  - `spark` — profiling/đo hiệu năng (đã bật trong `paper-global.yml` -> `spark.enabled: true`).
- Bảo mật/Chống bot/Đăng nhập:
  - `nLogin.jar` — xác thực tài khoản trong offline-mode/proxy.
  - `nAntiBot.jar` — bảo vệ chống spam/bot.
  - `nCore/` — core lib của bộ n* (thư mục cấu hình).

Ghi chú: Các thư mục cấu hình tương ứng đã được tạo trong `plugins/` (MMOCore, MMOItems, MythicLib, nAntiBot, nLogin, TAB, ProtocolLib, spark, ...).

## Yêu cầu hệ thống
- Java 21 (khuyến nghị): Cài đặt từ Adoptium/Oracle/OpenJDK.
- RAM: Tuỳ quy mô người chơi và plugin; khởi chạy nhớ set `-Xms`/`-Xmx` phù hợp (ví dụ 4–8 GB cho bộ plugin RPG).
- Băng thông ổn định nếu mở công khai Internet.

## Khởi chạy nhanh (Windows)
1. Đảm bảo `eula.txt` đã là `eula=true` (đã thiết lập).
2. Cài Java 21 và thêm vào PATH.
3. Mở `start.bat` hoặc chạy thủ công:

```bat
cd /d e:\[LangBatOnFix]
java -Xms4G -Xmx8G -jar love.jar
```

4. Máy chủ lắng nghe tại `0.0.0.0:25565` (nếu `server-ip` để trống). Chỉnh `server.properties` nếu cần.

Mẹo: Bạn có thể cập nhật `start.bat` để thêm tham số JVM:

```bat
@echo off
set JAVA_ARGS=-Xms4G -Xmx8G -XX:+UseG1GC -XX:+ParallelRefProcEnabled -XX:MaxGCPauseMillis=200 -XX:+UnlockExperimentalVMOptions -XX:+AlwaysPreTouch -Dusing.aikars.flags=https://mcflags.emc.gs -Daikars.new.flags=true
java %JAVA_ARGS% -jar love.jar nogui
pause
```

## Cấu hình chính đáng chú ý
- `server.properties`
  - `gamemode=survival`, `difficulty=hard`, `max-players=10`, `online-mode=false`.
  - `view-distance=10`, `simulation-distance=10`.
- `bukkit.yml`
  - Giới hạn spawn: quái 70, động vật 10; autosave 6000 tick.
- `spigot.yml`
  - `restart-on-crash: true` (script restart mặc định là `./start.sh` — trên Windows nên quản lý bằng `.bat` hoặc service riêng).
  - Nhiều tối ưu cho entity tracking/activation, tick-per, merge radius, v.v.
- `config/paper-global.yml`
  - Bật `spark.enabled: true` (profiling). Nhiều giới hạn packet và tối ưu I/O.
  - Proxy: `proxies.velocity.enabled: false`, `bungee-cord.online-mode: true` (nếu dùng proxy, cập nhật mục này và `server.properties` tương ứng).
- `config/paper-world-defaults.yml`
  - `anti-xray.enabled: false` (có thể bật nếu cần).
  - Rất nhiều tuỳ chọn về spawn/tick/despawn/hopper/collisions… áp dụng mặc định cho tất cả thế giới.

## Thế giới và dữ liệu
- Thế giới chính: `world/` (Overworld), `world_nether/`, `world_the_end/`.
- Mỗi thư mục thế giới có `paper-world.yml` để override cấu hình mặc định. Bạn có thể chỉnh theo nhu cầu.

## Nhật ký và theo dõi hiệu năng
- `logs/latest.log` và các log luân phiên `.log.gz` theo ngày.
- `spark` có thể tạo báo cáo qua lệnh trong console (xem tài liệu spark) để phân tích TPS, heap, tick time.

## Bảo mật và đăng nhập
- Máy chủ ở `online-mode=false`. Hãy đảm bảo:
  - Cấu hình `nLogin` đúng (chính sách đăng nhập, đăng ký, mật khẩu, bảo vệ grief).
  - Nếu đặt sau proxy (Bungee/Velocity), cấu hình forwarding IP và bảo vệ firewall; bật `velocity.enabled` nếu dùng Velocity và set `secret`.
  - Đóng `query`/`rcon` nếu không sử dụng (hiện đã tắt).

## Nâng cấp phiên bản
- Đặt jar Paper mới vào `versions/<phiên-bản>/paper-<phiên-bản>.jar` và cập nhật loader (`love.jar`) hoặc `start.bat` trỏ đến đúng jar.
- Sao lưu toàn bộ `world/`, `plugins/`, `config/`, `server.properties` trước khi nâng cấp.

## Sự cố thường gặp
- Không vào được server khi `online-mode=false`: kiểm tra cấu hình `nLogin`, proxy, và firewall.
- Lag/TPS thấp: dùng `spark` để lấy báo cáo; giảm `view-distance`, `simulation-distance`, xem lại cấu hình tick/entity; xem plugin nặng (MMOItems/MMOCore).
- Crash/restart: do `spigot.yml` có `restart-on-crash: true`, hãy đảm bảo script Windows phù hợp (không phải `./start.sh`).

## Ghi công & giấy phép
- Nền tảng: PaperMC (GPLv3). Xem tài liệu tại https://docs.papermc.io/
- Plugin bên thứ ba thuộc quyền tác giả tương ứng (MMOCore, MMOItems, MythicLib, nLogin, nAntiBot, ProtocolLib, TAB, spark). Hãy tuân thủ giấy phép của từng plugin.
- Cấu hình/tuỳ biến trong repo này thuộc về chủ server LANGBATON SMP.

---

### Phụ lục A — Danh sách plugin (theo thư mục `plugins/`)
- `MMOCore-1.13.1-SNAPSHOT.jar`
- `MMOItems-6.10.1-SNAPSHOT.jar`
- `MythicLib-dist-1.7.1-20250911.223617-39.jar`
- `nAntiBot.jar`
- `nLogin.jar`
- `ProtocolLib.jar`
- `TAB v5.2.5.jar`
- Thư mục cấu hình: `.paper-remapped/`, `bStats/`, `MMOCore/`, `MMOItems/`, `MythicLib/`, `nAntiBot/`, `nCore/`, `nLogin/`, `ProtocolLib/`, `spark/`, `TAB/`.

### Phụ lục B — Tham số JVM gợi ý
Tham khảo bộ cờ của Aikar cho máy chủ Paper:

```bat
-XX:+UseG1GC -XX:+ParallelRefProcEnabled -XX:MaxGCPauseMillis=200 -XX:+UnlockExperimentalVMOptions -XX:+DisableExplicitGC -XX:+AlwaysPreTouch -XX:G1NewSizePercent=30 -XX:G1MaxNewSizePercent=40 -XX:G1HeapRegionSize=8M -XX:G1ReservePercent=20 -XX:G1HeapWastePercent=5 -XX:G1MixedGCCountTarget=4 -XX:InitiatingHeapOccupancyPercent=15 -XX:G1MixedGCLiveThresholdPercent=90 -XX:G1RSetUpdatingPauseTimePercent=5 -XX:SurvivorRatio=32 -XX:+PerfDisableSharedMem -XX:MaxTenuringThreshold=1
```

Điều chỉnh `-Xms`/`-Xmx` tuỳ RAM.
