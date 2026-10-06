---

copyright:
  years: 2026
lastupdated: "2026-10-06"

keywords: PKCS11, PKCS #11, cryptographic functions, supported mechanisms, key attributes, Key Protect

subcollection: key-protect

---

{{site.data.keyword.attribute-definition-list}}

# PKCS #11 capabilities and support in {{site.data.keyword.keymanagementserviceshort}}
{: #pkcs11-functions}

{{site.data.keyword.keymanagementservicefull}} supports a broad set of PKCS #11 functions, mechanisms, key types, and attributes, enabling you to integrate standard cryptographic operations into your applications.
{: shortdesc}

PKCS #11 is a standard API (also known as Cryptoki) that defines a platform-independent interface to cryptographic token devices such as hardware security modules (HSMs). The following sections describe the functions, mechanisms, key attributes, and elliptic curves that are supported in {{site.data.keyword.keymanagementserviceshort}}.

## Supported PKCS #11 functions
{: #pkcs11-supported-functions}

The following table lists all PKCS #11 functions that are supported by {{site.data.keyword.keymanagementserviceshort}}, organized by functional category.

| Category | Functions |
| -------- | --------- |
| Initialization & info | `C_Initialize`, `C_Finalize`, `C_GetInfo`, `C_GetFunctionList` |
| Slot & token | `C_GetSlotList`, `C_GetSlotInfo`, `C_GetTokenInfo`, `C_GetMechanismList`, `C_GetMechanismInfo`, `C_InitToken` |
| PIN management | `C_SetPIN` |
| Session management | `C_OpenSession`, `C_CloseSession`, `C_CloseAllSessions`, `C_GetSessionInfo`, `C_GetOperationState`, `C_SetOperationState`, `C_Login`, `C_Logout` |
| Object management | `C_CreateObject`, `C_CopyObject`, `C_DestroyObject`, `C_GetObjectSize`, `C_GetAttributeValue`, `C_SetAttributeValue` |
| Object search | `C_FindObjectsInit`, `C_FindObjects`, `C_FindObjectsFinal` |
| Encryption | `C_EncryptInit`, `C_Encrypt`, `C_EncryptUpdate`, `C_EncryptFinal` |
| Decryption | `C_DecryptInit`, `C_Decrypt`, `C_DecryptUpdate`, `C_DecryptFinal` |
| Digest | `C_DigestInit`, `C_Digest`, `C_DigestUpdate`, `C_DigestFinal` |
| Signing | `C_SignInit`, `C_Sign`, `C_SignUpdate`, `C_SignFinal` |
| Verification | `C_VerifyInit`, `C_Verify`, `C_VerifyUpdate`, `C_VerifyFinal` |
| Dual operations | `C_DigestEncryptUpdate`, `C_DecryptDigestUpdate`, `C_SignEncryptUpdate`, `C_DecryptVerifyUpdate` |
| Key management | `C_GenerateKey`, `C_GenerateKeyPair`, `C_WrapKey`, `C_UnwrapKey`, `C_DeriveKey` |
| Random number | `C_GenerateRandom` |
{: caption="Supported PKCS #11 functions by category" caption-side="bottom"}

`C_CreateObject` supports the following object types: secret key objects, private key objects, public key objects, data objects, X.509 Public Key Certificate objects, WTLS public key certificate objects, and X.509 attribute certificate objects.
{: note}

When you use `C_InitToken`, the `pPin` value must be the PIN of the Security Officer (SO) user that was created by using the CLI command `ibmcloud kp crypto-unit user add --type keystoreSO`.
{: note}

## Supported mechanisms
{: #pkcs11-supported-mechanisms}

The following table lists the cryptographic mechanisms that are supported by {{site.data.keyword.keymanagementserviceshort}}, grouped by operation type.

| Operation | Supported mechanisms |
| --------- | -------------------- |
| Encrypt and decrypt | `CKM_RSA_PKCS`[^1], `CKM_RSA_PKCS_OAEP`[^1], `CKM_AES_ECB`, `CKM_AES_CBC`, `CKM_AES_CBC_PAD` |
| Sign and verify | `CKM_RSA_PKCS`[^1], `CKM_RSA_X9_31`[^1], `CKM_SHA224_RSA_PKCS`, `CKM_SHA256_RSA_PKCS`, `CKM_SHA384_RSA_PKCS`, `CKM_SHA512_RSA_PKCS`, `CKM_SHA224_RSA_PKCS_PSS`, `CKM_SHA256_RSA_PKCS_PSS`, `CKM_SHA384_RSA_PKCS_PSS`, `CKM_SHA512_RSA_PKCS_PSS`, `CKM_ECDSA`[^1], `CKM_ECDSA_SHA224`, `CKM_ECDSA_SHA256`, `CKM_ECDSA_SHA384`, `CKM_ECDSA_SHA512`, `CKM_SHA224_HMAC`, `CKM_SHA256_HMAC`, `CKM_SHA384_HMAC`, `CKM_SHA512_HMAC` |
| Digest | `CKM_SHA224`, `CKM_SHA256`, `CKM_SHA384`, `CKM_SHA512` |
| Generate key or key pair | `CKM_RSA_PKCS_KEY_PAIR_GEN`, `CKM_RSA_X9_31_KEY_PAIR_GEN`, `CKM_EC_KEY_PAIR_GEN` (`CKM_ECDSA_KEY_PAIR_GEN`), `CKM_GENERIC_SECRET_KEY_GEN`, `CKM_AES_KEY_GEN` |
| Wrap and unwrap | `CKM_RSA_PKCS`, `CKM_RSA_PKCS_OAEP`, `CKM_AES_ECB`, `CKM_AES_CBC`, `CKM_AES_CBC_PAD` |
| Derive | `CKM_ECDH1_DERIVE`, `CKM_ECDH1_COFACTOR_DERIVE` |
{: caption="Supported PKCS #11 mechanisms by operation" caption-side="bottom"}

[^1]: This mechanism supports only single-part operations. Multi-part update functions such as `C_EncryptUpdate`, `C_DecryptUpdate`, and `C_DigestUpdate` cannot be used with this mechanism.

## Supported attributes and key types
{: #pkcs11-supported-attributes}

The following table lists the PKCS #11 attributes supported for each key type and object class in {{site.data.keyword.keymanagementserviceshort}}.

| Key name | Attribute list |
| -------- | -------------- |
| AES keys | `CKA_CHECK_VALUE`, `CKA_CLASS`, `CKA_COPYABLE`, `CKA_DECRYPT`, `CKA_DERIVE`, `CKA_ENCRYPT`, `CKA_END_DATE`, `CKA_EXTRACTABLE`, `CKA_ID`, `CKA_KEY_TYPE`, `CKA_LABEL`, `CKA_LOCAL`, `CKA_MODIFIABLE`, `CKA_PRIVATE`, `CKA_SENSITIVE`, `CKA_SIGN`, `CKA_SIGN_RECOVER`, `CKA_START_DATE`, `CKA_TOKEN`, `CKA_TRUSTED`, `CKA_UNWRAP`, `CKA_VALUE`, `CKA_VALUE_LEN`, `CKA_VERIFY`, `CKA_WRAP`, `CKA_WRAP_WITH_TRUSTED` |
| EC private keys | `CKA_CLASS`, `CKA_COPYABLE`, `CKA_DECRYPT`, `CKA_DERIVE`, `CKA_EC_PARAMS` (`CKA_ECDSA_PARAMS`), `CKA_END_DATE`, `CKA_EXTRACTABLE`, `CKA_ID`, `CKA_KEY_TYPE`, `CKA_LABEL`, `CKA_LOCAL`, `CKA_MODIFIABLE`, `CKA_PRIVATE`, `CKA_SENSITIVE`, `CKA_SIGN`, `CKA_START_DATE`, `CKA_SUBJECT`, `CKA_TOKEN`, `CKA_UNWRAP`, `CKA_VALUE`, `CKA_WRAP`, `CKA_WRAP_WITH_TRUSTED` |
| EC public keys | `CKA_CLASS`, `CKA_COPYABLE`, `CKA_DERIVE`, `CKA_EC_PARAMS` (`CKA_ECDSA_PARAMS`), `CKA_EC_POINT`, `CKA_ENCRYPT`, `CKA_END_DATE`, `CKA_ID`, `CKA_KEY_TYPE`, `CKA_LABEL`, `CKA_LOCAL`, `CKA_MODIFIABLE`, `CKA_NEVER_EXTRACTABLE`, `CKA_PRIVATE`, `CKA_PUBLIC_KEY_INFO`, `CKA_SIGN_RECOVER`, `CKA_START_DATE`, `CKA_SUBJECT`, `CKA_TOKEN`, `CKA_TRUSTED`, `CKA_VERIFY`, `CKA_WRAP_WITH_TRUSTED` |
| RSA private keys | `CKA_CLASS`, `CKA_COEFFICIENT`, `CKA_COPYABLE`, `CKA_DECRYPT`, `CKA_DERIVE`, `CKA_END_DATE`, `CKA_EXTRACTABLE`, `CKA_EXPONENT_1`, `CKA_EXPONENT_2`, `CKA_ID`, `CKA_KEY_TYPE`, `CKA_LABEL`, `CKA_LOCAL`, `CKA_MODIFIABLE`, `CKA_MODULUS`, `CKA_PRIME_1`, `CKA_PRIME_2`, `CKA_PRIVATE`, `CKA_PRIVATE_EXPONENT`, `CKA_PUBLIC_EXPONENT`, `CKA_SENSITIVE`, `CKA_SIGN`, `CKA_START_DATE`, `CKA_SUBJECT`, `CKA_TOKEN`, `CKA_TRUSTED`, `CKA_UNWRAP`, `CKA_VERIFY`, `CKA_WRAP`, `CKA_WRAP_WITH_TRUSTED` |
| RSA public keys | `CKA_CLASS`, `CKA_COPYABLE`, `CKA_DERIVE`, `CKA_ENCRYPT`, `CKA_END_DATE`, `CKA_ID`, `CKA_KEY_TYPE`, `CKA_LABEL`, `CKA_LOCAL`, `CKA_MODIFIABLE`, `CKA_MODULUS_BITS`, `CKA_NEVER_EXTRACTABLE`, `CKA_PRIVATE`, `CKA_PUBLIC_EXPONENT`, `CKA_PUBLIC_KEY_INFO`, `CKA_SIGN_RECOVER`, `CKA_START_DATE`, `CKA_SUBJECT`, `CKA_TOKEN`, `CKA_TRUSTED`, `CKA_WRAP_WITH_TRUSTED` |
| Generic keys | `CKA_CLASS`, `CKA_COPYABLE`, `CKA_DECRYPT`, `CKA_DERIVE`, `CKA_ENCRYPT`, `CKA_EXTRACTABLE`, `CKA_ID`, `CKA_KEY_TYPE`, `CKA_LABEL`, `CKA_LOCAL`, `CKA_MODIFIABLE`, `CKA_PRIVATE`, `CKA_SENSITIVE`, `CKA_SIGN`, `CKA_SIGN_RECOVER`, `CKA_START_DATE`, `CKA_TOKEN`, `CKA_UNWRAP`, `CKA_VERIFY`, `CKA_VERIFY_RECOVER`, `CKA_WRAP`, `CKA_WRAP_WITH_TRUSTED` |
| Certificates | `CKA_CERTIFICATE_CATEGORY`, `CKA_CERTIFICATE_TYPE`, `CKA_HASH_OF_ISSUER_PUBLIC_KEY`, `CKA_HASH_OF_SUBJECT_PUBLIC_KEY`, `CKA_ISSUER`, `CKA_JAVA_MIDP_SECURITY_DOMAIN`, `CKA_NAME_HASH_ALGORITHM`, `CKA_SERIAL_NUMBER`, `CKA_URL`, `CKA_OBJECT_ID`, `CKA_OWNER`, `CKA_AC_ISSUER`, `CKA_ATTR_TYPES` |
{: caption="Supported PKCS #11 attributes by key type" caption-side="bottom"}

## Supported elliptic curves for EC key generation
{: #pkcs11-supported-curves}

The following elliptic curves are supported when you use `CKM_EC_KEY_PAIR_GEN` to generate EC key pairs in {{site.data.keyword.keymanagementserviceshort}}.

| Curve group | Supported curves |
| ----------- | ---------------- |
| NIST curves | `P-192` (aka `secp192r1`, `prime192v1`), `P-224`, `P-256`, `P-384`, `P-521` |
| Regular Brainpool curves | `BP-160R` (aka `brainpool160r1`), `BP-192R`, `BP-224R`, `BP-256R`, `BP-320R`, `BP-384R`, `BP-512R` |
| Twisted Brainpool curves | `BP-160T` (aka `brainpool160t1`), `BP-192T`, `BP-224T`, `BP-256T`, `BP-320T`, `BP-384T`, `BP-512T` |
| Standards for Efficient Cryptography | `secp256k1` |
{: caption="Elliptic curves supported for CKM_EC_KEY_PAIR_GEN" caption-side="bottom"}

## Setting up keystores and IAM access for PKCS #11
{: #pkcs11-keystores-iam}

Before you can perform PKCS #11 operations, you must create a keystore within your {{site.data.keyword.keymanagementserviceshort}} instance by using the following CLI command. Each instance supports a maximum of 5 keystores.

```sh
ibmcloud kp keystore create --name my-key-store --label my-keystore-label
```

After you create a keystore, use {{site.data.keyword.iamlong}} (IAM) access policies to control which users and service IDs can perform operations against it. Two resource attributes are available for fine-grained keystore access control:

**`keystoreId`**
:   Scopes a policy to a specific keystore. When specified, the policy applies only to operations targeting that keystore.

**`pkcsoperation`**
:   Scopes a policy to a specific PKCS #11 function. The following operators are supported:

    | Operator | Behavior |
    | -------- | -------- |
    | `stringEquals` | Grants access to the exact function specified, for example `C_Decrypt`. |
    | `stringMatch` | Grants access to all functions matching a wildcard pattern, for example `C_Digest*`. |
    | `stringExists` | Grants access to all PKCS #11 functions (any value present). |
    {: caption="Supported operators for the pkcsOperation attribute" caption-side="bottom"}

For information on how to construct an IAM policy that uses these attributes, see [Granting access to keystores and PKCS #11 operations](/docs/key-protect?topic=key-protect-grant-access-keys#grant-access-keystore-level) and [Roles and IAM policies](/docs/key-protect?topic=key-protect-manage-access#manage-access-roles-policies).
