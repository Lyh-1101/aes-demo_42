# aes-demo_42

一个用于教学演示的 AES 基本变换实现，每一步都输出 4×4 状态矩阵，可直接与 FIPS-197 标准中间值逐字节对照。

## 项目简介

本项目实现了 AES 加密第一轮中的四个基本变换：

- 字节替代（SubBytes）
- 行移位（ShiftRows）
- 列混合（MixColumns）
- 轮密钥加（AddRoundKey）

网上多数 AES 教学代码只给程序、不给中间结果，跑出来的数对不对很难判断。本项目的做法是每执行一步就打印当前状态矩阵，配合 FIPS-197 的标准样例，任何人都能核对每一步的结果。

## 环境要求

- C 编译器（GCC / MinGW / MSVC 均可）
- 标准 C 库，无需第三方依赖

## 编译与运行

Linux / macOS：

```bash
gcc -Wall -o aes_42 src/aes_42.c
./aes_42
```

Windows（MinGW）：

```bash
gcc -Wall -o aes_42.exe src/aes_42.c
aes_42.exe
```

Windows（VS Code + C/C++ 插件）：打开 `src/aes_42.c`，直接按运行按钮即可。

程序会先提示输入明文，再提示输入密钥，每次输入 16 个十六进制字节，用空格分隔，不带 `0x`。

## 使用示例

输入（FIPS-197 标准样例）：

```text
32 43 F6 A8 88 5A 30 8D 31 31 98 A2 E0 37 07 34
2B 7E 15 16 28 AE D2 A6 AB F7 15 88 09 CF 4F 3C
```

输出：

```text
After AddRoundKey:
19 A0 9A E9
3D F4 C6 F8
E3 E2 8D 48
BE 2B 2A 08

After SubBytes:
D4 E0 B8 1E
27 BF B4 41
11 98 5D 52
AE F1 E5 30

After ShiftRows:
D4 E0 B8 1E
BF B4 41 27
5D 52 11 98
30 AE F1 E5

After MixColumns:
04 E0 48 28
66 CB F8 06
81 19 D3 26
E5 9A 7A 4C
```

上面四组结果与 FIPS-197 附录 B 给出的中间值完全一致。

## 目录结构

```text
aes-demo_42/
├── src/
│   └── aes_42.c        # 四个基本变换与主程序
├── README.md
├── CONTRIBUTING.md
├── CHANGELOG.md
└── LICENSE
```

## 函数命名

按课程要求，函数名和内部变量均带学号后两位 `_42`：

| 函数 | 作用 |
|---|---|
| `SubBytes_42` | 查 S 盒完成字节替代 |
| `ShiftRows_42` | 第 i 行循环左移 i 个字节 |
| `MixColumns_42` | 列方向上做 GF(2^8) 矩阵乘法 |
| `AddRoundKey_42` | 状态矩阵与子密钥逐字节异或 |
| `gfmultby02_42` / `gfmultby03_42` | GF(2^8) 上乘 2、乘 3 |

## 已知限制

- 只实现单轮变换，未实现 10 轮完整加密与密钥扩展
- 暂未实现逆变换，因此不支持解密
- 暂无单元测试与 CI

上面三项已分别登记为 issue，欢迎参与。

## 许可证

本项目采用 [MIT License](LICENSE)。

## 参与贡献

提交代码前请阅读 [CONTRIBUTING.md](CONTRIBUTING.md)，分支命名、提交信息格式和 PR 流程都在里面。
