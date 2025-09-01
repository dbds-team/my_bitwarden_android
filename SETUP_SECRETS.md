# 设置 Android 构建所需的 Secrets

## 📝 需要设置的 GitHub Secrets

在你的私有仓库 `dbds-team/my_bitwarden_android` 中设置以下 secrets：

### 1. 进入仓库设置
- 打开 https://github.com/dbds-team/my_bitwarden_android
- 点击 **Settings** → **Secrets and variables** → **Actions**

### 2. 添加以下 Secrets

#### KEY_JKS（必需）
- **描述**: Base64 编码的 keystore 文件
- **生成方法**:
  ```bash
  # 如果你还没有 keystore，先创建一个
  keytool -genkey -v -keystore release.jks -keyalg RSA -keysize 2048 -validity 10000 -alias bitwarden
  
  # 将 keystore 转为 base64
  base64 -w 0 release.jks > keystore_base64.txt
  
  # 复制 keystore_base64.txt 的内容作为 secret 值
  ```

#### ALIAS（必需）
- **描述**: Keystore 中的别名
- **示例值**: `bitwarden`
- **说明**: 创建 keystore 时使用的 alias

#### ANDROID_KEY_PASSWORD（必需）
- **描述**: Key 的密码
- **说明**: 创建 keystore 时设置的 key password

#### ANDROID_STORE_PASSWORD（必需）
- **描述**: Keystore 的密码
- **说明**: 创建 keystore 时设置的 store password

## 🔐 创建新的 Keystore（如果需要）

```bash
# 创建新的 keystore
keytool -genkey -v \
  -keystore release.jks \
  -keyalg RSA \
  -keysize 2048 \
  -validity 10000 \
  -alias bitwarden

# 系统会提示输入：
# 1. keystore password (记为 ANDROID_STORE_PASSWORD)
# 2. 你的信息（姓名、组织等）
# 3. key password (记为 ANDROID_KEY_PASSWORD)

# 查看 keystore 信息
keytool -list -v -keystore release.jks
```

## 📋 快速设置模板

如果你想快速测试，可以使用以下值（仅用于测试！）：

1. 创建测试 keystore:
```bash
keytool -genkey -v \
  -keystore test.jks \
  -storepass testpass123 \
  -keypass testpass123 \
  -alias bitwarden \
  -keyalg RSA \
  -keysize 2048 \
  -validity 10000 \
  -dname "CN=Test, OU=Test, O=Test, L=Test, S=Test, C=US"
```

2. 转换为 base64:
```bash
base64 -w 0 test.jks
```

3. 设置 secrets:
- `KEY_JKS`: [上面命令的输出]
- `ALIAS`: `bitwarden`
- `ANDROID_KEY_PASSWORD`: `testpass123`
- `ANDROID_STORE_PASSWORD`: `testpass123`

## ⚠️ 安全提醒

- **生产环境**请使用强密码
- 妥善保管 keystore 文件和密码
- 不要在代码中硬编码密码
- 定期备份 keystore 文件（丢失后无法更新应用）

## 🚀 验证设置

设置完成后，重新运行 workflow 即可自动签名 APK。