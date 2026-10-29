# iOS 设置 URL Scheme 备忘

> 整理日期：2026-10-29
> 适用设备：iPhone18,2（iPhone 17 系列），iOS 26.6.2

## 一、三种 scheme 对比

| scheme | 状态 | 说明 |
|--------|------|------|
| `prefs:` | 老旧，iOS 26 基本失效 | 最早写法，`prefs:root=WIFI` 这种 |
| `App-prefs:` | 过渡格式 | 你的系统只认它，且只到第一层 |
| `settings-navigation://` | iOS 26 新引入 | 仅系统内部 / 快捷指令可用，第三方 App 被拒 |

## 二、实测结论（本机）

- ✅ `App-prefs:root=General` → 能打开「通用」页
- ❌ `App-prefs:root=General&path=STORAGE_MGMT` → 子路径失效，跳错
- ❌ `prefs:root=General` → opened:false
- ❌ `settings-navigation://com.apple.Settings.General` → opened:false

## 三、新 scheme 的变化要点

1. **格式变了**：从键值对（`root=` + `path=`）变成 URL 路径风格（用 `/` 分层）
2. **命名变了**：
   - 旧 `Privacy` → 新 `PrivacyAndSecurity`
   - 旧 `ACCOUNTS_AND_PASSWORDS` → 新 `PASSWORDS`
3. **限制收紧**：新 scheme 从第三方 App 调用会被拒，只在系统内部和快捷指令里有效

## 四、明确失效的入口（iOS 26+）

- Wi-Fi：`prefs:root=WIFI` 已不可靠
- 个人热点：`INTERNET_TETHERING` 行为已变
- 墙纸：只开设置首页，进不了子页
- iPhone 存储空间：`STORAGE_MGMT` 无对应新路径，已失效

## 五、使用约定

以后遇到「新的、和以前不一样」的 scheme 一律不用；已失效/未验证的私有深链接不再反复跳转，直接改用可靠方式（手动路径、主屏幕长按等）。

## 参考

- 权威清单：https://github.com/1mehdifaraji/ios-settings-url-schemes
