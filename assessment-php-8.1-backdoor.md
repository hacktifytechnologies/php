# Assessment — PHP 8.1.0-dev Supply Chain Backdoor RCE

---

## Question 1 — MCQ

**What is the name of the malicious HTTP header that activates the PHP 8.1.0-dev backdoor?**

- A) `User-Agent` — the standard browser identification header
- B) `X-PHP-Eval` — a custom debug header
- C) **`User-Agentt` — a deliberately misspelled header with a double `t`** ✅
- D) `X-Forwarded-For` — repurposed as a command channel

> **Answer:** C — The misspelling was deliberate: hard to spot in code review and never sent by legitimate clients, ensuring the backdoor only fires for the attacker.

---

## Question 2 — MCQ

**What string must the `User-Agentt` header value begin with to activate code execution?**

- A) `phpexec`
- B) `backdoor`
- C) `evilcode`
- D) **`zerodium`** ✅

> **Answer:** D — The remainder of the header value after the `zerodium` prefix is passed directly to `zend_eval_string()` and executed as PHP code on the server.

---

## Question 3 — MCQ

**What type of attack does this vulnerability represent?**

- A) A zero-day in PHP's JIT compiler
- B) A logic error in input validation
- C) A use-after-free in the HTTP parser
- D) **A supply chain attack — malicious commits pushed to the official PHP Git repository under the names of legitimate maintainers** ✅

> **Answer:** D — Attackers compromised `git.php.net` and injected the backdoor into the source, attributed to real PHP developers Nikita Popov and Rasmus Lerdorf.

---

## Question 4 — Fill in the Blank

**What string prefix must appear at the start of the `User-Agentt` header value to activate the PHP backdoor?**

**Answer:** `zerodium`

> The backdoor checks that the header value starts with `zerodium`. Everything after this prefix is passed to `zend_eval_string()` and executed as PHP code on the server with full application privileges.

---

## Question 5 — Fill in the Blank

**What internal PHP function executes the attacker-supplied code extracted from the `User-Agentt` header?**

**Answer:** `zend_eval_string`

> This is PHP's internal code evaluation engine — the same mechanism that underlies `eval()`. It takes an arbitrary string and executes it as PHP code within the current runtime context.

---

*Lab target:* `http://localhost:8080`
