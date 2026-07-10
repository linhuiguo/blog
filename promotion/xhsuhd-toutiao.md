# 标题（三选一，发布时挑一个）
1. 世界杯看球神器！Docker 一键部署本地 4K 直播源，免 KEY 还能拖拽回放
2. 不用花一分钱看 4K 世界杯！本地部署 IPTV 聚合，拖拽回放丝滑到飞起
3. 小白也会：5 分钟用 Docker 部署 XHSUHD，把小红书直播变成你的专属 4K 频道

---

> 作者：先生靠谱
> 说明：本文基于开源项目 XHSUHD 实测整理，部署命令、端口、M3U 地址均来自项目官方说明，可放心照抄。仅供个人学习交流，请遵守当地法律法规，勿用于商业分发。

---

# 世界杯看球神器：Docker 一键部署本地 4K 直播源，免 KEY 还能拖拽回放

2026 世界杯打得正酣，最痛苦的莫过于：直播源找不到、找到了卡成 PPT、号称高清其实只有 720P、进球错过了回放还转圈。

今天给你一个**开源神器 XHSUHD**——用 Docker 在本地一键部署，把小红书的世界杯直播聚合成标准 IPTV 源。支持 **4K 超高清**、自带**时移回放**（进度条随便拖）、**不用任何 API Key**、零成本。

你只需要有一台能跑 Docker 的设备（电脑、NAS、树莓派都行），5 分钟搞定。

---

## 一、先搞清楚它能干啥

```
小红书世界杯直播 ──▶ XHSUHD(本地服务) ──▶ 标准 M3U 直播源
                                      │
                  你的播放器(PotPlayer/VLC/TiviMate) 直接播 4K + 回放
```

简单说：本地起个服务，自动把直播聚合成一个 `.m3u` 播放列表，你把它填进任意 IPTV 播放器，就能看 4K 直播 + 丝滑回放。

**核心亮点：**
- **真 4K 画质**：直连超高清流，大屏看球毛级体验
- **免 KEY 部署**：不注册、不配 Token，即开即用
- **极速回放**：进度条随意拖拽，全网最丝滑
- **全架构支持**：amd64（PC/服务器）、arm/v7、arm64（玩客云、树莓派、香橙派、各类 NAS）

---

## 二、前置条件（先确认你有这些）

| 条件 | 说明 |
|------|------|
| 一台能跑 Docker 的设备 | 电脑 / NAS（群晖、威联通）/ 树莓派 / 玩客云 / VPS 都行 |
| 装好 Docker | 没装去 docker.com 下桌面版，或 NAS 套件中心装 Container Manager |
| 设备开着、能 SSH（NAS 用自带终端） | 用来敲命令 |

> ✅ 确认 Docker 装好：命令行敲 `docker -v`，有版本号就 OK。

---

## 三、第 1 步：一键部署（推荐 CLI 方式）

打开设备的 SSH 终端（或群晖/威联通的终端），复制执行这一行：

```bash
docker run -d --name xhsuhd --restart=always -p 34567:34567 iptvtop/xhsuhd:latest
```

看到返回一长串容器 ID，说明启动成功。

> 小提示：如果之前部署过旧版想更新，用这行彻底重拉：
> ```bash
> docker rm -f xhsuhd 2>/dev/null; docker run -d --pull always --name xhsuhd --restart=always -p 34567:34567 iptvtop/xhsuhd:latest
> ```

---

## 四、第 2 步（可选）：用 Docker Compose 部署

如果你习惯用 Compose 统一管理（Portainer / 群晖 Container Manager），新建目录，建 `docker-compose.yml`：

```yaml
version: '3.8'
services:
  xhsuhd:
    image: iptvtop/xhsuhd:latest
    container_name: xhsuhd
    restart: always
    ports:
      - "34567:34567"
```

启动：`docker compose up -d`
更新：`docker compose pull && docker compose up -d --force-recreate`

> CLI 和 Compose 二选一即可，效果一样。小白直接抄第 1 步那行命令最省事。

---

## 五、第 3 步：拿到播放列表，填进播放器

部署成功后，你的专属播放列表地址是：

```
http://你的服务器IP:34567/xhslist.m3u
```

⚠️ 把"你的服务器IP"换成你设备的实际 IP：
- 本机看：填 `127.0.0.1` 或电脑局域网 IP（如 `192.168.1.100`）
- NAS / VPS：填那台机器的 IP 或域名

**推荐播放器：**
- Windows / macOS：PotPlayer（强烈推荐）、VLC、IINA
- iOS / Apple TV：Infuse、iPlayTV、APTV
- Android TV / 电视盒子：TiviMate（神级 IPTV 壳）、Perfect Player

把上面的 M3U 链接填进播放器的"网络串流 / 订阅地址"，就能解锁 4K 世界杯直播 + 回放列表。

---

## 六、常用维护命令（建议收藏）

| 想干啥 | 命令 |
|--------|------|
| 检查是否在跑 | `docker ps --filter name=xhsuhd` |
| 看实时日志（排查问题） | `docker logs -f xhsuhd` |
| 重启服务 | `docker restart xhsuhd` |
| 停止并删除 | `docker rm -f xhsuhd` |

---

## 七、常见问题

**Q：端口 34567 被占用了？**
A：改映射，比如 `-p 34568:34567`，地址相应改成 `:34568`。

**Q：播放器填了 M3U 没频道？**
A：先 `docker logs -f xhsuhd` 看日志，确认服务正常拉到源；再确认 IP 填对（同局域网用局域网 IP，外网用公网 IP 或域名）。

**Q：回放能拖拽吗？**
A：能，项目自带时移回放优化，进度条随便拖。

**Q：手机能看吗？**
A：能，用 Infuse / TiviMate 等填同一个 M3U 地址即可。

---

高码率 4K + 丝滑拖拽回放，才是看球的正确打开方式。赶紧给 NAS 或小主机安排上。

更多折腾教程见博客 **0149.cc.cd**。部署遇到问题评论区甩日志，我看到回你。

---

*本文由「先生靠谱」整理，命令与端口均来自 XHSUHD 项目官方说明。*
