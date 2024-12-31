# ICPanda Audit Report

**Index**

- [**Introduction**](https://www.notion.so/ICPanda-Audit-Report-1b491ddaad5b465cb1dd981c81225a97?pvs=21)
    - Purpose
    - Auditing Agency
    - Audit Team
- [**Disclaimers & Scope**](https://www.notion.so/ICPanda-Audit-Report-1b491ddaad5b465cb1dd981c81225a97?pvs=21)
    - Review watermarks
    - General Disclaimers
    - Within scope
- [**Detailed List of Findings**](https://www.notion.so/ICPanda-Audit-Report-1b491ddaad5b465cb1dd981c81225a97?pvs=21)

# **Audit Report Body**

<aside>
⚠️ **Disclaimer**

This security report has been prepared at the request of the project team. Its scope and depth were defined by the budget allocated by the project team and the time constraints associated with it. As such, this report is not intended to serve as a full, comprehensive audit of the project's codebase or its underlying systems.

The findings and recommendations presented in this report are based on the specific areas and components reviewed during the engagement period. It is possible that additional issues may exist in areas or aspects of the project that were not included in the scope of this assessment.

Stakeholders are encouraged to seek additional audits or reviews to ensure the security and reliability of the project as part of their due diligence. The authors of this report accept no liability for any decisions or outcomes arising from the use of the information provided herein.

</aside>

## Introduction

This document is the official code security audit report by [Solidstate](https://www.solidstateauditing.com/) for the **ICPanda DAO**. It represents a professional security review by a team of industry-native [Internet Computer Protocol](https://icpguide.com/) (”ICP”) experts.

The initial audit review was started on September 16, 2024, and completed __________. Here are the team's latest code updates since the audit began, along with their resolutions and responses to the [detailed list of initial audit findings](https://www.notion.so/ICPanda-Audit-Report-1b491ddaad5b465cb1dd981c81225a97?pvs=21).

### Purpose

This security audit was mainly focused on NFID's delegation flow but also extended to the non-deprecated parts of the Identity Manager canister and some other immediately adjacent areas. Our assessment includes a detailed review of the smart contracts, delegation processes, and associated code to ensure robustness against potential vulnerabilities, with particular attention to the integrity of the delegation logic and potential attack vectors, as well as a general review of the canisters for security, upgradeability, and SNS readiness. The review of the client code was restricted to the delegation flows.

### Auditing Agency

Solidstate is the first ICP-native code security auditing agency. It’s a venture by [Code & State](https://www.codeandstate.com/), which is an independent venture studio devoted to making it easier to build and earn on the Internet Computer. Solidstate contracts independent ICP protocol experts, with extensive experience building secure ICP decentralized applications to execute professional code security audits for ICP projects. Our audits follow accepted industry standards while expanding them to cover the unique security considerations of ICP decentralized applications.

### Integrity Directives

Solidstate conducts audits with the highest standards of professional integrity and deep respect for the ICP ecosystem.

We work with our clients to allow them some control over the scope and duration of the audits, but clients do not have any control over the content of the final audit report. Our audit reports must be an accurate representation of all findings during the audit.

Compensation for Solidstate audits is determined by a fixed USD-based daily rate contracted with the client upfront. To avoid any potential conflicts of interest, Solidstate does not accept native tokens or equity from a project as compensation for executing its audit.
Neither Solidstate nor Code & State hold any equity stake in Identity Labs.

Solidstate auditors are instructed to follow these directives:

- The [scope](https://www.notion.so/ICPanda-Audit-Report-1b491ddaad5b465cb1dd981c81225a97?pvs=21) of the audit must be made clear in the audit report.
- The audit report must list any important security considerations that were not covered during the audit as [out of scope](https://www.notion.so/ICPanda-Audit-Report-1b491ddaad5b465cb1dd981c81225a97?pvs=21).
- All known identified and verified issues must be included in the [detailed list of findings](https://www.notion.so/ICPanda-Audit-Report-1b491ddaad5b465cb1dd981c81225a97?pvs=21) on the audit report.
- Issues will remain listed as open or “not fixed” on the audit report until an appropriate resolution is verified during a post-audit review.
- In general, any significant considerations that might impact prospective investors and users of the decentralized application should be included in the final audit report.

### Audit Team

### Security Researcher: [Nima Rasooli](https://x.com/0xNimaRa)

With a history of leading the development of multiple notable ICP projects, Nima now works full time as a Security Researcher at Solidstate, bringing a strong blend of technical knowledge and hands-on experience in the Web3 space.

### Project Manager: [Esteban Suarez](https://x.com/EstebanSuarez)

As Project Lead at Solidstate, Esteban oversees the audit’s progress, ensuring clear communication between the audit team and the client, and assists in the organization and delivery of the final report.

### Technical Writer: [Robin Moore](https://twitter.com/RobinMooreWeb3)

Robin edited the raw input of the auditors into this final report. Leveraging her expertise in editing, technical content creation, SEO, social media, and deep research, she strives to make technology accessible and engaging from beginners to enthusiasts.

### Risk Assessment Priorities

| **Likelihood \ Impact** | **Low**      | **Medium**   | **High**     | **Critical**  |
|-------------------------|--------------|--------------|--------------|---------------|
| **Highly unlikely**     | Low          | Low          | Medium       | High          |
| **Possible**            | Low          | Medium       | High         | Very High     |
| **Highly likely**       | Medium       | High         | Very High    | Very High     |


## Disclaimers & Scope

### Review watermarks

The initial review performed in the code was based on commit `ae4d0069808220fb196c9f5f7332e09f6155e8b6` of the [ic-panda](https://github.com/ldclabs/ic-panda/commit/ae4d0069808220fb196c9f5f7332e09f6155e8b6) GitHub repository.

### **General Disclaimers**

The security of decentralized applications on the Internet Computer (ICP) is still experimental, with evolving risks and many unknowns. Users should exercise caution, conduct thorough research, and use applications at their own risk, as neither Solidstate, Code & State nor the auditors are liable for any losses or damages. This report provides technical insights but does not constitute financial or legal advice, and its scope is inherently limited, covering only the work performed during the audit. While the audit was conducted in good faith by experienced experts, no guarantees can be made about the future security of the projects, as new vulnerabilities may emerge on such novel technology. Additionally, since ICP canisters have mutable code, this audit is only applicable to the specific commit hashes reviewed, and updates to the code invalidate its findings, making incremental reviews necessary.

### Within Scope

The following directories are in scope:

- `/src/ic_message`
- `/src/ic_message_profile`
- `/src/ic_message_channel`
- `/src/ic_message_types`

### Tally of issues by severity

| Category           | Findings |
| ---                | --- |
| Very High          | 1 |  
| High               | 3 |  
| Medium             | 6 |
| Low                | 7 |  
| Total | 17  | 

## Detailed List of Findings

## Detailed List of Findings

### **SS-ICPANDA-001: Use of Test Keys in Production Environment for Schnorr Signatures**

**Component:** `ic_message`  
**Severity:** **Very High**

**Details:**  
The canister is currently utilizing test keys (`dfx_test_key` or `test_key_1`) for Schnorr signatures within a production environment on the Internet Computer mainnet. These keys are intended exclusively for testing and development purposes, not for production, as specified in [api_init.rs#38](https://github.com/ldclabs/ic-panda/blob/8c838b2b21ffca4314072c871338648b836f01b4/src/ic_message/src/api_init.rs#L38):

```rust
#[derive(Clone, Debug, CandidType, Deserialize)]
pub struct InitArgs {
    name: String,
    managers: BTreeSet<Principal>,
    schnorr_key_name: String, // Use "dfx_test_key" for local replica and "test_key_1" for testnet/mainnet
}

#[derive(Clone, Debug, CandidType, Deserialize)]
pub struct UpgradeArgs {
    name: Option<String>,
    managers: Option<BTreeSet<Principal>>,
    schnorr_key_name: Option<String>, // Use "dfx_test_key" for local replica and "test_key_1" for testnet/mainnet
}

#[ic_cdk::init]
fn init(args: Option<ChainArgs>) {
    store::state::with_mut(|s| {
        s.price = types::Price {
            channel: 1000 * types::TOKEN_1,
            name_l1: 1_000_000 * types::TOKEN_1,
            name_l2: 200_000 * types::TOKEN_1,
            name_l3: 50_000 * types::TOKEN_1,
            name_l5: 20_000 * types::TOKEN_1,
            name_l7: 5000 * types::TOKEN_1,
        };
        s.schnorr_key_name = "dfx_test_key".to_string();
    });

    match args {
        None => {}
        Some(ChainArgs::Init(args)) => {
            store::state::with_mut(|s| {
                s.name = args.name;
                s.managers = args.managers;
                s.schnorr_key_name = args.schnorr_key_name;
            });
        }
        Some(ChainArgs::Upgrade(_)) => {
            ic_cdk::trap(
                "cannot initialize the canister with an Upgrade args. Provide an Init args instead.",
            );
        }
    }

    ic_cdk_timers::set_timer(Duration::from_secs(0), || {
        ic_cdk::spawn(store::state::try_init_public_key())
    });
}
```

**Implications:**  
- **Weakened Security:** Test keys are deployed on subnets with only 13 nodes, which offer lower replication compared to production keys on high-replication subnets. This reduction in replication diminishes the security guarantees provided.
- **Risk of Access Loss:** According to the Threshold Signatures documentation, test keys may be removed in the future. If these keys are relied upon in a production environment, their removal could result in the loss of access to essential data or functionalities.

**Recommendation:**  
- **Immediate Key Replacement:** Replace all instances of `dfx_test_key` or `test_key_1` in production canisters with the appropriate production keys. For Schnorr signatures, adhere to the Threshold Signatures documentation by using either `(bip340secp256k1, key_1)` or `(ed25519, key_1)`.

---

### **SS-ICPANDA-002: Panics in the Pre-Upgrade Hooks Could Result in Permanent Unupgradability**

**Components:** `ic_message`, `ic_message_channel`, `ic_message_profile`  
**Severity:** **High**

**Details:**  
A panic occurring within the pre-upgrade hook can permanently lock a canister. Since the existing code governs the hook, consistent panics will block any future upgrades. References can be found in [store.rs#201](https://github.com/ldclabs/ic-panda/blob/ae4d0069808220fb196c9f5f7332e09f6155e8b6/src/ic_message_profile/src/store.rs#L201), [store.rs#382](https://github.com/ldclabs/ic-panda/blob/ae4d0069808220fb196c9f5f7332e09f6155e8b6/src/ic_message_channel/src/store.rs#L382), and [store.rs#210](https://github.com/ldclabs/ic-panda/blob/ae4d0069808220fb196c9f5f7332e09f6155e8b6/src/ic_message/src/store.rs#L210).

**Implications:**  
A canister may become irreversibly locked, preventing any further upgrades or modifications.

**Recommendation:**  
- **Implement Graceful Error Handling:** Ensure that the pre-upgrade hook gracefully handles decoding and encoding errors. The code should be panic-resistant to avoid causing permanent lockouts.

---

### **SS-ICPANDA-003: OOM Vulnerability Due to Unchecked Channel Alias and Tags**

**Component:** `ic_message_profile`  
**Severity:** **High**

**Details:**  
The system does not validate the length of channel tags or aliases provided by users, leading to potential Out of Memory (OOM) issues and vulnerability to resource exhaustion attacks. Refer to [store.rs#326](https://github.com/ldclabs/ic-panda/blob/ae4d0069808220fb196c9f5f7332e09f6155e8b6/src/ic_message_profile/src/store.rs#L326-L327):

```rust
pub fn update(
    user: Principal,
    now_ms: u64,
    mut input: types::UpdateProfileInput,
) -> Result<types::ProfileInfo, String> {
    PROFILE_STORE.with(|r| {
        let mut m = r.borrow_mut();
        match m.get(&user) {
            Some(mut p) => {
                if let Some(bio) = input.bio {
                    p.bio = bio;
                }
                if !input.unfollow.is_empty() {
                    for u in input.unfollow {
                        p.following.remove(&u);
                    }
                }
                if !input.follow.is_empty() {
                    p.following.append(&mut input.follow);
                }
                if p.following.len() > types::MAX_PROFILE_FOLLOWING {
                    return Err("following limit exceeded".to_string());
                }
                if !input.remove_channels.is_empty() {
                    for k in input.remove_channels {
                        p.channels.remove(&k);
                    }
                }
                if !input.upsert_channels.is_empty() {
                    for (k, v) in input.upsert_channels {
                        p.channels.insert(
                            k,
                            ChannelSetting {
                                pin: v.pin,
                                alias: v.alias,
                                tags: v.tags,
                            },
                        );
                    }
                }

                p.active_at = now_ms;
                m.insert(user, p.clone());
                Ok(p.into_info(user, ic_cdk::id(), true))
            }
            None => Err("profile not found".to_string()),
        }
    })
}
```

**Implications:**  
Attackers can exploit the Internet Computer’s reverse gas model to flood the system with excessively long aliases or a large number of tags at minimal cost. This can lead to Denial of Service (DoS) or resource exhaustion, degrading system performance or causing outages.

**Recommendation:**  
- **Enforce Strict Limits:** Implement stringent restrictions on both the number and length of channel tags and aliases to prevent abuse.

---

### **SS-ICPANDA-004: Attacker Can Impersonate Others by Associating Another Public Key**

**Component:** `ic_message_profile`  
**Severity:** **Medium**

**Details:**  
The `update_profile_ecdh_pub` function does not verify the ownership of the provided public key. It merely stores the `ecdh_pub` value, enabling attackers to associate someone else’s public key with their profile. See [api_update.rs#L17](https://github.com/ldclabs/ic-panda/blob/ae4d0069808220fb196c9f5f7332e09f6155e8b6/src/ic_message_profile/src/api_update.rs#L17) and [store.rs#L271](https://github.com/ldclabs/ic-panda/blob/ae4d0069808220fb196c9f5f7332e09f6155e8b6/src/ic_message_profile/src/store.rs#L271).

```rust
#[ic_cdk::update]
fn update_profile_ecdh_pub(ecdh_pub: ByteArray<32>) -> Result<(), String> {
    let caller = ic_cdk::caller();
    let now_ms = ic_cdk::api::time() / MILLISECONDS;
    store::profile::update_profile_ecdh_pub(caller, now_ms, ecdh_pub)
}
```

**Implications:**  
Attackers can impersonate other users by associating a different user’s public key with their own profile. This allows them to act on behalf of the victim without possessing the corresponding private key, potentially leading to unauthorized actions or access.

**Recommendation:**  
- **Implement Proof-of-Possession:** Require users to provide a signed challenge (such as a timestamp or nonce) using the private key corresponding to the submitted `ecdh_pub`. Verify this signature before accepting and storing the public key.

Example (simplified):

```rust
fn update_profile_ecdh_pub(ecdh_pub: ByteArray<32>, signature: Vec<u8>) -> Result<(), String> {
    let challenge = "server-provided-nonce";
    if !verify_signature(ecdh_pub, signature, challenge) {
        return Err("Invalid proof of possession".to_string());
    }
    // Proceed to update the profile
}
```

---

### **SS-ICPANDA-005: Insecure State Management via Serialization**

**Component:** `ic_message_profile`  
**Severity:** **Medium**

**Details:**  
The current implementation of `STATE` utilizes both `StableCell` and `StableBTreeMap` while depending on `pre_upgrade` and `post_upgrade` hooks to save and load state. Any corruption during the save process or failures during loading can cause the canister to panic, rendering it inaccessible. Additionally, this approach negates the benefits of stable structures, which are designed to function without manual serialization hooks.

**Implications:**  
- **Canister Lockout:** If deserialization fails during an upgrade, the canister can become trapped and unresponsive.
- **Data Loss:** Corrupted or overwritten state cannot be recovered, leading to potential loss of critical data.
- **Performance Degradation:** Large state serializations can negatively impact performance and scalability.

**Recommendation:**  
- **Adopt Direct Stable Memory Storage:** Utilize stable memory storage as outlined in [Stable Structures](https://mmapped.blog/posts/14-stable-structures). This approach eliminates the need for manual serialization in hooks and leverages the inherent stability of these structures.

---

### **SS-ICPANDA-006: Any User Can Change or Steal Others' Usernames**

**Component:** `ic_message`  
**Severity:** **High**

**Details:**  
A malicious user can assign an arbitrary username to another user without the victim's consent. This action replaces the victim's username and frees up the original username for the attacker to claim. The attacker can purchase the stolen username, and if the original user attempts to reclaim it later, they will need to pay again.

**Implications:**  
- **Loss of Usernames:** Users may lose ownership of their chosen usernames.
- **Financial Loss:** Users incur additional costs if they need to repurchase their usernames.
- **Unauthorized Access:** Attackers gain unauthorized control over user profile settings related to usernames.

**Recommendation:**  
- **Restrict Username Modifications:** Only allow users with explicit permissions from the account owner to change or assign usernames.

---

### **SS-ICPANDA-007: Unencrypted System Messages Leak User Activities**

**Component:** `ic_message_channel`  
**Severity:** **Medium**

**Details:**  
Although data within the canister on the Internet Computer is considered public if accessible through canister methods without access controls, node operators inherently have access to all stored data. System messages are stored in plaintext, meaning actions like channel creation or file uploads can reveal user identities to node operators, thereby compromising user privacy.

**Implications:**  
- **Privacy Violations:** Unauthorized access to channel information and user activities.
- **Exposure of Sensitive Data:** User identities and actions are exposed to node operators without encryption.

**Recommendation:**  
- **Encrypt System Messages:** Ensure that all system messages are encrypted before storage. Avoid storing any plaintext or unencrypted private information on the canister, especially when relying on the trustworthiness of node provider identities.

---

### **SS-ICPANDA-008: Unencrypted Channel Members Lists Lead to User Identity Leaks**

**Component:** `ic_message_channel`  
**Severity:** **Medium**

**Details:**  
User identities are stored as plain principal IDs rather than being hashed using a secure algorithm. This allows node providers to access comprehensive lists of each channel’s administrators and members, exposing user identities.

**Implications:**  
- **Unauthorized Data Access:** Complete channel member lists are accessible to node providers, leading to potential privacy breaches.
- **Exposure of User Identities:** Plain principal IDs can reveal sensitive information about users.

**Recommendation:**  
- **Secure Storage of Identities:** Encrypt user identities where possible and use hashing algorithms for authentication purposes. Avoid storing plain principal IDs to minimize reliance on the trustworthiness of node operators.

---

### **SS-ICPANDA-009: No Data Validation on Channel IDs**

**Component:** `ic_message_profile`  
**Severity:** **Low**

**Details:**  
Users can subscribe to channels that do not exist because the input is not validated in `update_profile` and `admin_upsert_profile` functions.

**Implications:**  
The canister state may accumulate invalid or non-existent channel IDs, leading to inconsistencies and potential errors in channel management.

**Recommendation:**  
- **Validate Channel IDs:** Ensure that any channel ID provided by users exists before accepting and processing it.

---

### **SS-ICPANDA-010: Incorrect Length Limit Applied to Usernames**

**Components:** `ic_message`, `ic_message_types`  
**Severity:** **Low**

**Details:**  
The username length validation incorrectly uses `MAX_USER_SIZE` instead of `MAX_USER_NAME_SIZE`:

```rust
#[ic_cdk::update(guard = "is_authenticated")]
async fn register_username(username: String, name: Option<String>) -> Result<UserInfo, String> {
    if username.len() > types::MAX_USER_SIZE {
        Err("username is too long".to_string())?;
    }
    ...
}
```

**Implications:**  
Usernames may be subject to incorrect length restrictions, either being too short or too long, which can cause unexpected behavior or usability issues.

**Recommendation:**  
- **Correct Length Validation:** Replace `MAX_USER_SIZE` with `MAX_USER_NAME_SIZE` (or the appropriate constant) when enforcing username length limits.

---

### **SS-ICPANDA-011: Burned Gas Amount Can Be Misrepresented**

**Component:** `ic_message_channel`  
**Severity:** **Low**

**Details:**  
The burned gas value caps at `u64::MAX` instead of using checked addition or a larger data type. Once this maximum value is reached, additional gas consumption is not recorded, leading to underreporting of total burned gas.

**Implications:**  
- **Inaccurate Gas Usage Reporting:** The canister may display lower than actual gas usage, resulting in misleading statistics and potential mismanagement of resources.

**Recommendation:**  
- **Use Appropriate Arithmetic Handling:** Implement checked arithmetic or utilize a larger integer type (e.g., `Nat` or `U256`) to prevent saturation at `u64::MAX` and ensure accurate tracking of burned gas.

---

### **SS-ICPANDA-012: No Check on Channel Remove and Upsert Lists**

**Component:** `ic_message_profile`  
**Severity:** **Low**

**Details:**  
The system does not verify whether channels listed in both the remove and upsert lists are handled consistently. Refer to [store.rs#315-L331](https://github.com/ldclabs/ic-panda/blob/ae4d0069808220fb196c9f5f7332e09f6155e8b6/src/ic_message_profile/src/store.rs#L315-L331).

**Implications:**  
Channels appearing in both the remove and upsert lists may be removed and re-added simultaneously, leading to unpredictable and unintended behavior within the canister.

**Recommendation:**  
- **Ensure Consistent Channel Operations:** Validate within `UserProfileInput` that the same channel does not appear in both the remove and upsert lists to maintain consistent channel management.

---

### **SS-ICPANDA-013: No Check on Follow and Unfollow Lists**

**Component:** `ic_message_profile`  
**Severity:** **Low**

**Details:**  
The code does not prevent a user from being included in both the follow and unfollow lists simultaneously. Refer to [store.rs#303-313](https://github.com/ldclabs/ic-panda/blob/ae4d0069808220fb196c9f5f7332e09f6155e8b6/src/ic_message_profile/src/store.rs#L303-L313).

**Implications:**  
A user may unintentionally follow and unfollow the same profile within a single transaction, resulting in inconsistent or unexpected outcomes in their follow status.

**Recommendation:**  
- **Validate Follow/Unfollow Inputs:** Ensure that a principal cannot appear in both the follow and unfollow sets within `UserProfileInput` to maintain consistent follow statuses.

---

### **SS-ICPANDA-014: No Data Validation on User-Provided URIs**

**Component:** `ic_message_types`  
**Severity:** **Medium**

**Details:**  
The system does not verify whether the provided data is a valid URI, as seen in [profile.rs#91-93](https://github.com/ldclabs/ic-panda/blob/ae4d0069808220fb196c9f5f7332e09f6155e8b6/src/ic_message_types/src/profile.rs#L91-L93).

**Implications:**  
Invalid or malicious URIs may enable Cross-Site Scripting (XSS) attacks or other harmful behaviors if such URIs are rendered on a frontend or utilized by other consumers.

**Recommendation:**  
- **Implement URI Validation:** Ensure that all input strings conform to valid URI formats by validating them before acceptance and processing.

---

### **SS-ICPANDA-015: No Length Check on User-Provided Link Images**

**Component:** `ic_message_types`  
**Severity:** **Medium**

**Details:**  
There is no upper limit on the length of user-submitted image strings. Attackers can exploit the reverse gas model to upload excessively large image data at minimal cost, potentially leading to resource exhaustion. Refer to [profile.rs#87-95](https://github.com/ldclabs/ic-panda/blob/ae4d0069808220fb196c9f5f7332e09f6155e8b6/src/ic_message_types/src/profile.rs#L87-L95):

```rust
pub fn validate(&self) -> Result<(), String> {
    if self.title.is_empty() || self.title.trim() != self.title || self.title.len() > 128 {
        return Err("invalid title".to_string());
    }
    if self.uri.is_empty() || self.uri.trim() != self.uri || self.uri.len() > 128 {
        return Err("invalid uri".to_string());
    }
    Ok(())
}
```

**Implications:**  
A malicious user could deplete canister storage resources by submitting extremely large image data, leading to potential service degradation or denial of service.

**Recommendation:**  
- **Set Explicit Size Limits:** Define and enforce a maximum allowable size for images, rejecting any input that exceeds this limit to prevent resource exhaustion.

---

### **SS-ICPANDA-016: Channels Can Share the Same Pin Number**

**Component:** `ic_message_profile`  
**Severity:** **Low**

**Details:**  
Multiple channels are allowed to use the same pin number, which can lead to confusion or unintended behavior within the system.

**Implications:**  
Users may expect each pin to uniquely identify a channel. Allowing duplicate pin numbers can result in ambiguity and incorrect associations between pins and channels.

**Recommendation:**  
- **Enforce Pin Uniqueness:** Ensure that each pin value is unique across all channels to prevent overlapping and maintain clear channel identification.

---

### **SS-ICPANDA-017: Usernames Can Be Transferred to Anonymous Identities**

**Component:** `ic_message`  
**Severity:** **Low**

**Details:**  
There is no mechanism to prevent the transfer of usernames to anonymous identities.

**Implications:**  
Usernames may be inadvertently transferred or "burned," leading to loss of username ownership and potential confusion among users.

**Recommendation:**  
- **Authenticate Receiving Identities:** Validate that any identity receiving a username is authenticated, preventing unauthorized or accidental transfers to anonymous entities.

---

## Post-Audit Review #1

A “post-audit review” is essentially another audit focused on reviewing and incorporating all changes in the code since the previously audited commit. The majority of this report was based on the “initial audit review”, and this section covers the findings of the first and last “post-audit review” required to incorporate code changes leading up to the publication of this final audit report.

### Status of found issues

**Summary**

| **Severity** | **Found** | **Fixed/Partially Fixed** | **Not an Issue** | **Not fixed (see Notes)** |
| --- | --- | --- | --- | --- |
| Very High |  |  |  |  |
| High |  |  |  |  |
| Medium |  |  |  |  |
| Low |  |  |  |  |

**Detailed statuses**

| **Finding** | **Severity** | **Status** | **Commit** | **Notes** |
| --- | --- | --- | --- | --- |
|  |  |  |  |  |