# AnyBar Updates

该仓库用于托管 AnyBar 桌面应用的 Sparkle 更新 Feed 以及相关说明，方便分发新版本与快速回滚。

## 仓库内容

- `appcast.xml`：主 Sparkle feed，包含每次发布的版本号、构建日期、下载地址和 DSA 签名。

## Feed 地址

```
https://raw.githubusercontent.com/baorsmile/AnyBar-Updates/main/appcast.xml
```

## 维护流程

1. **准备二进制文件**：完成 AnyBar 新版本打包后，将 `.zip` 或 `.dmg` 上传到稳定的可公开访问的地址（建议使用 GitHub Releases 或 CDN）。
2. **生成 DSA 签名**：使用 Sparkle 提供的 `generate_appcast` 或 `sign_update` 工具生成签名，确保 `appcast.xml` 中的 `sparkle:dsaSignature` 与二进制文件匹配。
3. **更新 feed**：在 `appcast.xml` 中新增 `<item>`，填写版本、构建日期、下载链接、release notes 链接以及签名信息，并按时间倒序排列。
4. **验证**：在本地使用 Sparkle 自带的验证脚本或测试构建手动触发更新，确认能够下载并校验。
5. **发布**：将更改 push 到 `main`，等待客户端读取最新 feed 即可。

## 常见问题

- **Sparkle 自动化**：可考虑在 CI 中运行脚本自动写入 `appcast.xml`，但仍需人工确认签名。
- **回滚版本**：将旧版本 `<item>` 移到最上方即可回滚；如需彻底弃用某个版本，直接删除对应 `<item>`。
- **缓存刷新**：某些 CDN（含 GitHub raw）可能存在缓存，确保客户端读取到最新 feed，可以在客户端侧短期缩短缓存时间或在服务器端添加版本化参数。

> 该仓库仅包含更新 Feed，不包含 AnyBar 应用源代码。
