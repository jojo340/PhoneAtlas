# GitHub 远程数据库

`phones.json` 是 App 可直接下载的机型数据库。

## 发布到 GitHub

1. 将整个项目推送到 GitHub。
2. 打开仓库中的 `database/phones.json`。
3. 点击 **Raw**，复制地址。
4. 在 App 的“更新”页粘贴该地址，然后点击“检查并更新数据库”。

项目中的 GitHub Actions 会在每次修改数据库时自动检查 JSON 结构、重复 ID 和正面外观字段。

Raw 地址格式：

```text
https://raw.githubusercontent.com/<用户名>/<仓库名>/main/database/phones.json
```

App 也接受 `https://github.com/.../blob/.../phones.json` 形式的网页地址，并会自动通过 GitHub Contents API 下载，避免部分网络环境访问 Raw 域名超时。

## 图片

每款机型都有 `previewImageURL` 字段。建议上传宽高比约 4:3、透明背景、包含正反面的 WebP 或 PNG：

```text
database/images/apple-iphone-17-pro-2025.webp
```

然后把对应机型字段改成图片的 Raw URL。字段为空字符串或图片加载失败时，App 会自动绘制预览。

## 更新版本

修改数据后：

- 增加 `databaseVersion`
- 更新 `updatedAt`
- 保持现有机型 `id` 不变，避免用户收藏失效
- 确保 `schemaVersion` 仍为 `1`
- iPhone 正面外观字段 `screenForm` 建议使用：`灵动岛`、`原深感全面屏`、`Touch ID 经典屏`

重新导出内置数据：

```bash
swiftc PhoneAtlas/AppLanguage.swift PhoneAtlas/Phone.swift PhoneAtlas/PhoneCatalog.swift Tools/ExportDatabase.swift -o /tmp/export-phone-database
/tmp/export-phone-database database/phones.json
```
