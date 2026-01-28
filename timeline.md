**consolidated project timeline**

*FortuneTell MVP build* — the 12-step plan turned into a sequential roadmap. It shows what happens, who does it, how long it takes, and what output signals completion.  
 All durations are conservative; time can slip, but cost and quality should not.

---

## **FORTUNETELL DESIGN IMPLEMENTATION MASTER TIMELINE**

| Phase | Step | Duration (est.) | Primary AI | Your Role | Key Deliverables | Done When |
| ----- | ----- | ----- | ----- | ----- | ----- | ----- |
| **1** | **Specification Phase – Single Source of Truth** | 5–7 days | ChatGPT \+ Claude | Approve all `/spec` files | `/spec` folder complete (models, services, endpoints, UI, tests) | All six oracles defined and reviewed |
| **2** | **Tooling Assignment – One AI per Job** | 1 day setup | ChatGPT (orchestrator) | Approve repo \+ workflow structure | AI role map, GitHub repo ready | All AIs tested on small prompt tasks |
| **3** | **Minimal Tech Stack – Monolith First** | 5–7 days | ChatGPT \+ DeepSeek | Approve stack & environment | Running scaffold (Node \+ Next \+ Supabase) | Local server responds to `/health` |
| **4** | **UI/UX Production Path – Wireframe → Components → API** | 7–10 days | Gemini \+ Claude \+ ChatGPT | Approve wireframes; test UI | Functional Calendar, Tx list/form, Settings | Playwright tests green |
| **5** | **Backend Build Path – Models → Services → Controllers** | 7–10 days | ChatGPT \+ DeepSeek | Approve data models | Working API, all tests pass | Integration tests 100% green |
| **6** | **Projection Engine MVP – Deterministic Core** | 5 days | DeepSeek | Approve logic/performance | Core cash-flow engine live | p95 ≤ 200 ms, all oracle tests pass |
| **7** | **Testing System – Guardrails First** | 3 days | ChatGPT | Trigger CI runs | Full automated test suite | CI green across all layers |
| **8** | **Reconciliation – 30-Day Baseline** | 7 days | ChatGPT \+ DeepSeek | Verify match behavior | Manual \+ linked tx sync | Review queue functions correctly |
| **9** | **Accessibility – Data, Not Decoration** | 3 days | Gemini \+ ChatGPT | Approve prefs screen | High-contrast/narration functional | 0 critical Axe issues |
| **10** | **DevOps – Simple, Observable, Reversible** | 3 days setup | ChatGPT | Approve deploy \+ rollback test | CI/CD \+ healthchecks live | Manual rollback verified |
| **11** | **Refinement Loops – Evidence → Patch** | ongoing weekly | Claude \+ ChatGPT | Approve weekly fixes | Weekly report \+ issue triage flow | First full loop completes |
| **12** | **Phase Gates – Quality & Cost Control** | 2–3 days per gate | ChatGPT \+ Claude | Approve or hold | Gate A–D reports | Gate C (v1.0.0) passed |

---

## **Approximate Timeline (sequential, not parallel)**

| Month | Milestone |
| ----- | ----- |
| **Month 1** | Steps 1–3 complete → working scaffold |
| **Month 2** | Steps 4–6 → functional Testing MVP |
| **Month 3** | Steps 7–9 → soft-launch-ready build |
| **Month 4** | Steps 10–12 → public-ready MVP & gate process in place |
| **Month 5+** | Continuous refinement \+ scaling (loop steps 11–12 weekly) |

Total ≈ **10–12 weeks** from first spec draft to Public-Ready MVP (assuming 20 h/week).

---

## **Key Decision Points**

| Gate | Question You Answer | Typical Outcome |
| ----- | ----- | ----- |
| **A** | Does the core logic and UI work flawlessly in test? | Move to private testing |
| **B** | Do users find it easy & reliable? | Soft launch |
| **C** | Is multi-account & reconciliation stable under load? | Public MVP release |
| **D** | Are cost and retention within target? | Begin scaling & paid acquisition |

---

## **Ownership summary**

| Domain | Lead AI | You approve |
| ----- | ----- | ----- |
| **Architecture, API, tests** | ChatGPT | ✓ |
| **Business logic & perf** | DeepSeek | ✓ |
| **Documentation & reporting** | Claude | ✓ |
| **UI & accessibility** | Gemini | ✓ |
| **Deployment & CI/CD** | ChatGPT | ✓ |

---

## **Weekly operational rhythm (after Gate B)**

| Day | Task | Responsible |
| ----- | ----- | ----- |
| Monday | Pull logs & feedback → Claude summary | Claude |
| Tuesday | Approve / defer issues | You |
| Wed-Thu | AI fixes & CI verification | ChatGPT / DeepSeek / Gemini |
| Friday | Deploy to staging & smoke test | You |
| Weekend | Optional user analytics review | – |

---

Would you like me to produce this as a **Markdown timeline document** (clean table format, ready for GitHub `/docs/timeline.md`)?
