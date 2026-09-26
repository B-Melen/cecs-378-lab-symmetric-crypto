---
# WHO THIS IS. Fill both in before you submit.
#
# This repository is PRIVATE — you and I are the only people who can read it.
# I need these two lines to put your grade in Canvas against the right person:
# GitHub knows you as a username, Canvas knows you as a student, and this is
# the only place those two meet. A blank or wrong ID means a grade that lands
# on nobody, and I have to come find you to fix it.
name: "Brandon Melendez"
student_id: "029578599"

# The autograder reads only the honor flag below. Each ward is graded by
# running your exploit and checking its proof with the oracle, so there is no
# ward flag to paste. The lines below are for your own record: ward II and
# ward IV recover a CECS378 flag; ward I and ward III proofs are a duplicated
# block and a forged token, not flags.
honor: CECS378{honor_61089eb5a18ea0a6a8454a04}
ward2: CECS378{ward2_...}
ward4: CECS378{ward4_...}   # OMEGA WARD (Ω stretch)
---

# Grimoire of the Spellbreaker

> Every Spellbreaker keeps a grimoire, a record of how each ward fell, so the
> next break comes faster. This one is yours. Write an entry for every ward you
> defeated: comment your exploit code, cite at least one source, and tell me
> *how* the ward broke, not merely that it did. "I ran the attack" earns
> nothing; "the ward leaked X, which let me do Y" earns everything.

## The Oath

Speak the SOLDIER's Oath, run `python pledge.py`, and paste the honor flag it
yields into the frontmatter above. No honor flag, no marks: a Spellbreaker who
won't sign their work doesn't get paid.

## Ward I — The Wisp (ECB detection)

- **How I made the pattern flicker:** I split each ciphertext into 16 byte blocks and looked for a duplicate. ECB encrypts each block independently, so identical plaintext blocks convert into identical ciphertext blocks while other modes don’t. The ciphertext with a repeat was the ECB one.
- **The real-world sin this is (name the CVE class):** CWE-327, which is broken or risky crypto use, specifically ECB misuse, the "ECB penguin" flaw where repeated plaintext blocks leak structure through the ciphertext.

## Ward II — The Rune Golem (ECB byte-at-a-time)

- **The block size I measured, and how I measured it:** I measured 16 bytes. Found by growing input length 1 byte at a time. The ciphertext length jumped from 48 to 64 bytes once the input reached 9 bytes, a 16 byte jump.
- **How prying one rune at a time recovers the whole word:** Pad input so the next unknown secret byte lands as the last byte of a block. I try all 256 values there and compare ciphertext blocks to the oracle output. The match reveals the byte. Go forward one byte at a time to recover the whole secret.
- **Where this same flaw bites real systems:** Any system that appends attacker input next to a secret before ECB encrypting it lets an attacker extract the secret without the key. Some examples would be tokens or cookies with a fixed key.

## Ward III — The Mirror Knight (CBC bit-flipping)

- **Which ciphertext byte(s) I flipped, and what each plaintext byte became:** Block 2 (0-based), positions 0–11, via `old_cipher ^ known ^ desired, turning the next block into `;admin=true;`.
- **Why CBC let me forge a sigil the ward couldn't question:** CBC has no integrity check so flipping a ciphertext byte deterministically flips the same position plaintext byte in the next block, undetected.
- **Where this same flaw bites real systems:** Classic CBC bit flipping cookie or session forgery. This is why AEAD is preferred over unauthenticated CBC.

## Ward IV — OMEGA WARD (CBC padding oracle)  *(optional Ω stretch)*

- **What the one leaked bit told me, and how it cascades into full plaintext:**
- **The real-world reckoning (POODLE / Lucky 13):**

## Behind the curtain  *(optional, for the curious)*

- **Read the published oracle source: which primitives derive per-session
  secrets, and why does their one-wayness keep those proofs unforgeable?**

## Sources

-
