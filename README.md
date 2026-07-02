# Major Project Announcement

**Solana / Anchor Track - AMM (Automated Market Maker)**

---

You've spent the course learning the Solana execution model, Anchor fundamentals, PDAs and advanced constraints, security and auditing, SPL tokens and CPIs, token vaults and share accounting, AMM design, and the eBPF/sBPF foundations that everything runs on.

Now it's time to put all of that together.

The major project is not a tutorial follow-along. The goal is to design and ship a production-grade on-chain program with a well-designed account architecture, sound swap and liquidity math, a real security posture, and a serious test suite.

This project is intended to evaluate your ability to think like a protocol engineer: modeling on-chain state, reasoning about invariants, making architectural tradeoffs, and writing programs that hold up under adversarial conditions.

---

## The Project - Constant-Product AMM

Build a constant-product automated market maker on Solana, in the spirit of Uniswap v2 / Raydium's core pool, implemented as an Anchor program.

A user should be able to create a liquidity pool for a token pair, provide liquidity and receive LP tokens, swap one token for the other against the `x * y = k` invariant, and remove liquidity to redeem their proportional share. The program must enforce slippage protection, charge a swap fee that accrues to LPs, and use canonical PDAs for every authority and vault.

### Core areas

- Pool / market state accounts
- Constant-product invariant (`x * y = k`)
- LP token mint and proportional share accounting
- Swap math, fees, and price impact
- Slippage protection (minimum-out / maximum-in)
- Add / remove liquidity
- PDA-owned vaults and CPIs to the Token Program
- Security hardening and self-audit

---

## Technical Requirements

The following requirements apply to the project.

### Mandatory

- Rust
- Anchor framework
- SPL Token Program (mint, token accounts, ATAs)
- Canonical PDAs for pool authority and vaults
- CPIs with signer seeds for all token movements
- Cross-program invocation into the Token Program
- Checked arithmetic on all swap / share math
- Slippage protection on swaps
- Account discriminators and correct space calculation
- Unit tests
- Integration tests (LiteSVM / Bankrun or local validator)
- Deployable to localnet and devnet
- Program documentation (README + instruction reference)

### Expected Features

- LP token minting on deposit, burning on withdraw
- Swap fee accrual to liquidity providers
- First-deposit / initial-liquidity handling
- Input validation and error enums (Anchor errors)
- `has_one` / `seeds` / `bump` / `constraint` / `close` constraints
- Reinitialization protection on pool accounts
- Missing-signer and authority checks
- Overflow / rounding-direction discipline

### Recommended (if scope allows)

- Multiple pools managed by a single factory / config account
- Protocol fee split (LP fee vs. protocol fee)
- Zero-copy account for large pool state
- TypeScript client SDK around the IDL
- Price oracle / TWAP accumulator
- Concentrated-liquidity exploration writeup

---

## Grading Rubric (100 Points)

The project is evaluated as a protocol, not by the number of instructions implemented.

### 1. Protocol Design (20 Points)

How well was the system designed?

- Account / state modeling
- PDA seed design and canonical bumps
- Architectural decisions and tradeoffs
- Invariant reasoning (why `k` is preserved)
- Design documentation

### 2. AMM & Share Math (15 Points)

- Correct constant-product swap math
- Fee application and accrual to LPs
- Proportional LP mint / burn accounting
- Rounding direction always favors the pool
- Slippage / price-impact handling

You should be able to justify every formula and rounding choice.

### 3. Instruction & API Design (10 Points)

- Clean instruction surface (`init_pool`, `add_liquidity`, `remove_liquidity`, `swap`)
- Context and account-constraint design
- Argument validation
- Error enums and meaningful failures
- IDL quality and documentation

### 4. Program Implementation (15 Points)

- Handler implementation
- CPI and signer-seed correctness
- Business logic organization
- Separation of concerns (state / math / handlers)

We are evaluating maintainability and correctness, not line count.

### 5. Project Structure & Architecture (10 Points)

- Module organization (state, instructions, errors, math)
- Readability and consistency
- Dependency and workspace hygiene

Another developer should still understand it six months later.

### 6. Testing (10 Points)

- Unit tests on math and helpers
- Integration tests over full instruction flows
- Failure and adversarial scenarios

Tests should demonstrate confidence in the invariant.

### 7. Security (15 Points)

- Missing signer / authority checks
- Reinitialization attacks
- PDA collision and seed safety
- Arithmetic overflow / underflow
- First-depositor share-inflation attack
- Anchor protections vs. manual checks

This project handles user funds and is evaluated strictly on security.

### 8. Documentation (5 Points)

- README quality and setup instructions
- Architecture / account diagram
- Instruction and account reference
- Math and invariant writeup

Another developer should be able to build and deploy from the repository alone.

---

## Bonus Marks

The following can push an otherwise good project into distinction territory.

### Live Devnet Deployment (+10)

Deploy your program to devnet with a working client. The deployed program and a demo (swap + add/remove liquidity) must be functional during the viva.

### Exceptional Engineering (+5)

- Zero-copy pool state
- Multi-pool factory architecture
- Protocol-fee accounting
- TWAP / oracle accumulator
- Fuzz / property-based tests on the invariant
- sBPF analysis: disassemble your program, inspect the instruction stream, and report where CU cost or stack limits bite

These marks are discretionary and awarded only when the implementation demonstrates strong engineering quality.

---

## Deadline

Due before your viva slot. Your repository must be final, pushed, and accessible before the viva begins.

What is in the repository at the start of the viva is what gets evaluated. There are no post-viva fixes.

---

## Final Note

The objective of the major project is not to build the biggest protocol.

The objective is to build a program that is thoughtfully designed, correct under its invariant, security-conscious, well-tested, and deployable.

A small AMM with airtight math and a real security posture will score higher than a sprawling one that leaks funds. This rubric intentionally rewards protocol design, math correctness, security, testing, and engineering maturity far more than the raw number of instructions implemented.