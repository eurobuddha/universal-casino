# Timeout claim parity (APK 0.7.1 / MDS 2.9.1 / desktop casino-timeout-parity branch)

My Bets lists every owned bet across Minima and MxUSD, irrespective of the currency toggle. Cards retain the coin's currency. Play, new offers and History keep their currency filter.

`timeout-claims.js` ports APK `Bet.canClaimTimeout` and `TimeoutClaims`: phase 1 player, phase 2 house, strictly `age > timeout`, no eligibility at an unknown age. The banner reports both currencies and opens My Bets. Claims remain manual. The service still automatically reveals/resolves both currencies.

The service persists sorted claim IDs, suppresses repeat alerts across restarts, and cancels the reminder when a successful scan shows no eligible claims. Failed/non-array scans cannot clear claims. READ mode can show reminders but cannot auto-post transactions.

MDS notification API has no silent update or tab deep-link argument. Foreground claims use the banner; background reminders set `casino_timeout_open` so the next opening routes to My Bets. Partial removals do not make another sound; the last removal cancels. Desktop notifications have a direct My Bets click route and are silent while focused.

`service.js`, `timeout-claims.js`, and `offer-keepalive.js` must be byte-identical to minimaCore desktop `main/casino/`. Change the donor first and copy all three files. From `minimacore-desktop`, run `npm run test:casino`. The gate checks byte parity, both renderers, the real desktop VM, persistence/failure cases, and 160 vectors against the APK Java policy plus the identical covenant. Build the APK (`./gradlew testDebugUnitTest`) first so the gate can use its dependency classes; policy sources are recompiled by the gate. Override source locations using CASINO_DONOR, CASINO_DESKTOP and CASINO_APK when needed.

This is timeout/My Bets parity, not a claim of full transaction, settlement-precision or UI parity. Those remain separate adversarial-review work.

The follow-up keepalive fix is MDS 2.9.2 / APK 0.7.2 / desktop 0.16.66-casino-parity.2. See [OPEN-OFFER-KEEPALIVE.md](OPEN-OFFER-KEEPALIVE.md) for behavior, validation and merge instructions. The integrator must choose the next desktop release version.
