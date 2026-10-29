# oran

本仓库保存 `/home/oran/deploy` 的完整加密备份。

- 备份时间（Asia/Shanghai）：20261029-105927
- 归档条目：121,660 个文件、目录或链接
- 压缩加密后大小：6,548,019,256 字节（约 6.10 GiB）
- 内容包含隐藏文件、Git 历史、Conda 环境、源码、编译结果和目录中的运行数据。没有按 `.gitignore` 排除文件。
- 文件权限、数字所有者、ACL、扩展属性及符号链接均记录在归档中。
- 文件按备份执行期间读取；运行中的数据库需要另行确认一致性。操作系统及目录外的软件不属于此目录备份。

## 下载与恢复

分卷由 Git LFS 保存。需要安装 Git LFS、GnuPG、zstd 和 tar。请保管好单独留存在本机的 `restore-key.txt`；仓库内不包含解密口令。

```bash
git clone https://github.com/zhangchengl595-tech/oran.git
cd oran
git lfs install
git lfs pull
cd backups/deploy-full-20261029-105927
sha256sum -c SHA256SUMS
cat deploy-full-20261029-105927.tar.zst.gpg.part-* > deploy-full-20261029-105927.tar.zst.gpg
sha256sum -c ARCHIVE_SHA256SUM
mkdir -p recovered
gpg --batch --pinentry-mode loopback --passphrase-file /path/to/restore-key.txt --decrypt deploy-full-20261029-105927.tar.zst.gpg | zstd -dc | sudo tar --acls --xattrs --numeric-owner -xpf - -C recovered
```

将 `/path/to/restore-key.txt` 替换为你单独保存的密钥文件路径。恢复后完整目录位于 `recovered/deploy`。请恢复到新的空目录，检查结果后再使用。

GitHub 网页中的 LFS 指针或未包含 LFS 数据的 ZIP 文件不能替代完整的备份分卷。下载后请执行上面的校验。
