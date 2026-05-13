# aws-lc-support.patch

OTP 28.5 の `lib/crypto` (NIF) を AWS-LC v1.73.0 でビルドできるようにするためのパッチ。
OTP 標準の crypto NIF は OpenSSL / LibreSSL を前提に書かれており、AWS-LC では一部の API が
存在しない / 仕様が異なるため、そのままではビルドが通らない。

## 対象ファイル

- `lib/crypto/c_src/cipher.c`
- `lib/crypto/c_src/crypto.c`
- `lib/crypto/c_src/ec.c`
- `lib/crypto/c_src/openssl_config.h`
- `lib/crypto/c_src/openssl_version.h`

## 何をしているか

### `HAS_AWSLC` の導入 (`openssl_version.h`)

AWS-LC は `OPENSSL_IS_AWSLC` を定義しているので、それを検出して `HAS_AWSLC` を立てる。
以降のすべての分岐はこのマクロで切り替える。

### `unload_thread` / `OPENSSL_thread_stop` の除外 (`crypto.c`)

AWS-LC には `OPENSSL_thread_stop` がないため、`unload_thread` の宣言・定義・登録を `HAS_AWSLC`
の場合は無効化する。

### `EC_GROUP_set_seed` の戻り値を見ない (`ec.c`)

AWS-LC の `EC_GROUP_set_seed` は「何もせず 0 を返す」と公式ヘッダに明記されている。
そのまま OpenSSL と同じくエラー扱いするとビルド済みでも実行時に失敗してしまうので、
AWS-LC では戻り値をチェックせず呼び出すだけにする。

### DES CFB8 (`cipher.c`, `openssl_config.h`)

AWS-LC は `EVP_des_cfb8` / `EVP_des_ede3_cfb8` を提供しない。
`HAVE_DES_CFB8` マクロを導入し、`cipher_types[]` で `des_cfb` を空エントリにする。

### `openssl_config.h` のマクロ群

AWS-LC の API 差分を吸収するために、以下のマクロを追加・調整：

| 項目 | 対応 |
|---|---|
| `EVP_CTRL_CCM_SET_IVLEN/GET_TAG/SET_TAG` | `EVP_CTRL_AEAD_*` にマッピング |
| `EVP_CIPHER_type` | `EVP_CIPHER_nid` にマッピング |
| `OPENSSL_ECC_MAX_FIELD_BITS` | `661` でフォールバック定義 |
| `BN_FLG_CONSTTIME` | AWS-LC は constant-time が常時有効で削除済。`0` を定義 |
| `EVP_PKEY_CMAC` | `NID_cmac` にマッピング |
| `<openssl/modes.h>` | AWS-LC には存在しないので include しない |
| `HAVE_EVP_PKEY_new_CMAC_key` | AWS-LC では未定義にする |
| `HAVE_DES_ede3_cfb` | AWS-LC では未定義にする |
| `HAVE_X25519` / `HAVE_X448` / `HAVE_ED25519` / `HAVE_ED448` | AWS-LC では X25519 / Ed25519 のみ有効 (X448/Ed448 は header にあるが実装は失敗する) |
| `HAVE_CHACHA20` | AWS-LC は `EVP_chacha20_poly1305` のみで、単独の `EVP_chacha20()` がないので無効化 |
| `HAVE_POLY1305` | AWS-LC は `EVP_PKEY_POLY1305` を持たないので無効化 |

## バージョン整合性

- このパッチは **OTP 28.5** のソースに対して `git apply --check` がクリーンに通ることを確認済み。
- AWS-LC は **v1.73.0** のヘッダ (`include/openssl/`) を実機で grep して、各分岐の前提を確認済み。
- 上位バージョン (OTP 28.x のマイナーアップ、AWS-LC の次バージョン) に上げる際は、対象 5 ファイル
  の差分と AWS-LC ヘッダの差分を毎回確認すること。
