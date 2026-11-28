# Day 41: Securing Data with AWS KMS

## Overview

In this lab, I created a symmetric customer managed key (`devops-KMS-Key`) in AWS Key Management Service (AWS KMS) for encryption and decryption operations. Using the AWS CLI, I encrypted `/root/SensitiveData.txt` into `/root/EncryptedData.bin`, decrypted the ciphertext back to verify data integrity, and confirmed via `diff` that the decrypted output matched the original file.

---

## Lab Requirements

* **KMS Key Name / Alias:** `devops-KMS-Key` (Symmetric key, `Encrypt and decrypt` usage)
* **Source File:** `/root/SensitiveData.txt`
* **Encrypted Output File:** `/root/EncryptedData.bin`
* **Verification:** Decrypting `/root/EncryptedData.bin` and confirming no differences against `/root/SensitiveData.txt`

---

## Step-by-Step Implementation

### Step 1: Create the Symmetric KMS Key (`devops-KMS-Key`)

* Opened the **AWS KMS Console** and selected **Customer managed keys** from the navigation pane.
* Clicked **Create key**, chose **Symmetric** for key type, and kept **Encrypt and decrypt** selected under key usage.
* Entered **`devops-KMS-Key`** as the alias, reviewed the configuration, and clicked **Finish**.
* *(Optional CLI alternative)* Created the symmetric key directly via AWS CLI:
```bash
aws kms create-key \
  --description "devops-KMS-Key" \
  --key-usage ENCRYPT_DECRYPT \
  --key-spec SYMMETRIC_DEFAULT

```



### Step 2: Encrypt `/root/SensitiveData.txt` into `/root/EncryptedData.bin`

* Encrypted `/root/SensitiveData.txt` using `alias/devops-KMS-Key`, decoded the base64 ciphertext field, and saved the binary output to `/root/EncryptedData.bin`:
```bash
aws kms encrypt \
  --key-id alias/devops-KMS-Key \
  --plaintext fileb:///root/SensitiveData.txt \
  --output text \
  --query CiphertextBlob | base64 --decode > /root/EncryptedData.bin

```



### Step 3: Decrypt `/root/EncryptedData.bin` to Verify

* Decrypted `/root/EncryptedData.bin` via AWS KMS and wrote the decoded plaintext to `/root/DecryptedData.txt`:
```bash
aws kms decrypt \
  --ciphertext-blob fileb:///root/EncryptedData.bin \
  --output text \
  --query Plaintext | base64 --decode > /root/DecryptedData.txt

```



---

## Verification & Results

* Compared the original file against the decrypted file using `diff`:
```bash
diff /root/SensitiveData.txt /root/DecryptedData.txt

```


* Confirmed that `diff` returned no output, verifying that encryption and decryption succeeded with zero data loss or mismatch.

---

## Key Takeaways

* **Centralized Key Management:** Customer managed symmetric keys in AWS KMS let you control access policies, auditing, and key lifecycle centrally while keeping raw key material protected inside KMS hardware security modules.
* **Binary vs. Base64 Handling:** `aws kms encrypt` and `aws kms decrypt` return base64-encoded strings in JSON/text output, so decoding with `base64 --decode` and passing binary files with `fileb://` ensures exact byte-level round-trips.
