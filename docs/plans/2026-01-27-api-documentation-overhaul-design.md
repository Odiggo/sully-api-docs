# Sully.ai API Documentation Overhaul

**Date:** 2026-01-27
**Goal:** World-class, developer-first documentation that takes users from 0 → production

---

## Executive Summary

Transform the current API-reference-heavy docs into use-case-driven guides with:
- Clear navigation (Get Started → Guides → API Reference → Alpha)
- Production-grade code examples in TypeScript SDK, Python SDK, and raw HTTP
- Deep-dive guides for every major workflow
- First-class SDK documentation

**Target audience:** Developers evaluating and integrating Sully (greenfield)
**Mental model:** Use case first — developers pick their goal, get everything they need

---

## Part 1: Navigation Structure

### Current Problems
| Issue | Impact |
|-------|--------|
| Versions as top-level navigation | Forces users to pick "v2" or "alpha" before seeing content |
| Anemic Documentation tab | Only 2 pages — users bounce to API Reference |
| No quickstart | Developers piece together workflow themselves |
| SDKs invisible | Published SDKs not in navigation at all |
| Single "notes" guide | Covers 7+ concepts in one page |

### New Structure (Mintlify Best Practices)

```
Navigation (root = tabs):
│
├── Tab: "Get Started"
│   ├── Group: "Introduction"
│   │   ├── overview.mdx              # What is Sully?
│   │   ├── quickstart.mdx            # 5-min: audio → note
│   │   └── authentication.mdx        # Get your keys
│   │
│   └── Group: "SDKs"
│       ├── typescript.mdx            # @sullyai/sullyai
│       └── python.mdx                # sullyai on PyPI
│
├── Tab: "Guides"
│   ├── Group: "Core Workflows"
│   │   ├── transcription.mdx         # File upload + streaming
│   │   ├── clinical-notes.mdx        # SOAP + custom notes
│   │   ├── note-customization.mdx    # Styles vs templates deep dive
│   │   └── webhooks.mdx              # Async event handling
│   │
│   ├── Group: "Integrations"
│   │   ├── ehr-integration.mdx       # Structured JSON for EHR
│   │   ├── medical-coding.mdx        # ICD-10, CPT, SNOMED
│   │   ├── multi-language.mdx        # 35+ languages
│   │   └── context-enrichment.mdx    # Patient info, meds, history
│   │
│   └── Group: "Production"
│       ├── error-handling.mdx        # Retries, resilience
│       └── security.mdx              # HIPAA, webhook verification
│
├── Tab: "API Reference"
│   ├── Group: "Audio Transcriptions"
│   ├── Group: "Notes"
│   ├── Group: "Note Styles"
│   ├── Group: "Codings"
│   ├── Group: "Macros"
│   ├── Group: "Utils"
│   ├── Group: "Events"
│   └── Group: "Schemas"
│
└── Tab: "Alpha"
    ├── Group: "Overview"
    │   └── overview.mdx              # What's alpha, expectations
    │
    ├── Group: "Alpha Guides"
    │   ├── medical-consensus.mdx
    │   └── template-generation.mdx
    │
    └── Group: "Alpha API Reference"
        ├── Authentication
        ├── Medical Consensus
        └── Note Templates
```

### Anchors (Persistent Sidebar)
- Dashboard (https://dashboard.api.sully.ai)
- GitHub Demo
- Support (mailto:support@sully.ai)

---

## Part 2: Guide Specifications

### Standard Guide Template

Every guide follows this structure:
```
├── Overview (what & why, 2-3 sentences)
├── Prerequisites (what you need first)
├── Quick example (working code in <30 lines)
├── Deep dive sections (concepts, options, variations)
├── SDK examples (TypeScript + Python + HTTP tabs)
├── Error handling (common failures, how to recover)
├── Best practices (production tips)
└── Next steps (related guides)
```

### Code Example Format

All code examples use three tabs:
```mdx
<CodeGroup>
  <Tab title="TypeScript SDK">
    // SDK-native code using @sullyai/sullyai
  </Tab>
  <Tab title="Python SDK">
    # SDK-native code using sullyai
  </Tab>
  <Tab title="HTTP">
    // Raw fetch/requests for those who can't use SDKs
  </Tab>
</CodeGroup>
```

---

## Part 3: Guide Details

### Get Started Tab

#### 1. quickstart.mdx
**Goal:** 0 → working integration in 5 minutes

**Content:**
1. Install SDK (`npm install @sullyai/sullyai` / `pip install sullyai`)
2. Set API keys (environment variables)
3. Upload sample audio file
4. Get transcription back
5. Generate SOAP note from transcription
6. Print the result

**Deliverable:** Complete copy-paste-run example in TS and Python

---

#### 2. typescript.mdx
**Goal:** Master the TypeScript SDK

**Sections:**
- Installation and setup
- Client configuration
- Audio transcriptions (`client.audio.transcriptions.create()`)
- Notes (`client.notes.create()`, `client.notes.retrieve()`)
- Error handling (SDK error classes)
- Built-in retry behavior
- TypeScript benefits (types, autocompletion)

---

#### 3. python.mdx
**Goal:** Master the Python SDK

**Sections:**
- Installation and setup
- Sync vs async clients (`SullyAI` vs `AsyncSullyAI`)
- Audio transcriptions
- Notes
- Response handling (Pydantic models, `.to_json()`, `.to_dict()`)
- Error handling and retries

---

### Guides Tab — Core Workflows

#### 4. transcription.mdx
**Goal:** Master audio capture — file upload AND real-time streaming

**Sections:**
| Section | Content |
|---------|---------|
| File upload | Supported formats (WAV, MP3, FLAC, OGG, WebM, MP4, M4A, AAC, Opus), 100MB limit, async polling |
| Real-time streaming | WebSocket setup, token auth, connection lifecycle |
| Production streaming | Reconnection logic, error recovery, buffering strategies |
| Language support | BCP47 tags, multilingual mode (`multi`) |
| Choosing upload vs stream | Decision matrix |

**Code depth:** Production-grade WebSocket implementation with:
- Connection retry with exponential backoff
- Heartbeat/keepalive
- Graceful degradation
- Error event handling

---

#### 5. clinical-notes.mdx
**Goal:** Generate notes from transcriptions

**Sections:**
| Section | Content |
|---------|---------|
| SOAP notes | Built-in format, when to use |
| Custom styles | Creating from sample note |
| Note templates | Structured sections, precise control |
| Choosing the right approach | Decision flowchart |
| Polling vs webhooks | Async retrieval patterns |

**Code:** Side-by-side examples of SOAP, style-based, and template-based generation

---

#### 6. note-customization.mdx
**Goal:** Deep dive into styles, templates, and generation

**Sections:**
| Section | Content |
|---------|---------|
| Styles explained | How Sully learns from your sample |
| Building effective samples | What makes a good reference note |
| Templates explained | Section types (heading, text, list), hierarchy |
| Template schema reference | Full schema with examples |
| Alpha: AI template generation | Using the generation API |
| Migrating styles → templates | When and how to upgrade |

**Code:** Complete template examples for common specialties

---

#### 7. webhooks.mdx
**Goal:** Production-grade async event handling

**Sections:**
| Section | Content |
|---------|---------|
| Why webhooks | vs polling, when to use |
| Setup | Dashboard configuration |
| Event types | All 6 events with payload examples |
| Signature verification | HMAC-SHA256 validation (CRITICAL) |
| Handling events | Idempotency, ordering |

**Event types to document:**
- `audio_transcription.succeeded`
- `audio_transcription.failed`
- `note_generation.succeeded`
- `note_generation.failed`
- `coding.succeeded`
- `coding.failed`

**Code:** Production webhook handler with signature verification

---

### Guides Tab — Integrations

#### 8. ehr-integration.mdx
**Goal:** Extract structured JSON for EHR field mapping

**Sections:**
| Section | Content |
|---------|---------|
| JSON output modes | SOAP structure, template structure |
| Field mapping | How Sully JSON maps to common EHR fields |
| Text-to-JSON utility | Converting free text to structured data |
| Integration patterns | Webhooks → transform → EHR write |

**Code:** Full pipeline from audio → structured JSON → field mapping

---

#### 9. medical-coding.mdx
**Goal:** Extract ICD-10, CPT, SNOMED codes

**Sections:**
| Section | Content |
|---------|---------|
| What codes are extracted | ICD-10-CM, CPT, SNOMED CT |
| Creating a coding request | From transcription or note text |
| Understanding results | Diagnoses array, procedures array, text spans |
| Coding + notes workflow | Combining both in one pipeline |
| Validation & review | Using codes as suggestions |

**Code:** Complete coding workflow with result parsing

---

#### 10. multi-language.mdx
**Goal:** Handle international and multilingual encounters

**Sections:**
| Section | Content |
|---------|---------|
| Supported languages | Full list with BCP47 tags |
| Single-language mode | Specifying language, filtering |
| Multilingual mode | The `multi` tag, automatic detection |
| Language + streaming | WebSocket language parameter |

**Code:** Examples for Spanish, Mandarin, and multilingual

---

#### 11. context-enrichment.mdx
**Goal:** Improve note quality with patient context

**Sections:**
| Section | Content |
|---------|---------|
| Why context matters | How it improves accuracy |
| Available fields | `patientInfo`, `medicationList`, `previousNote`, `instructions`, `context`, `date` |
| Patient info | Name, DOB, gender |
| Medication list | Format and usage |
| Previous notes | Follow-up visits, continuity |
| Custom instructions | Specialty-specific guidance |

**Code:** Full example with all context fields

---

### Guides Tab — Production

#### 12. error-handling.mdx
**Goal:** Build resilient integrations

**Sections:**
| Section | Content |
|---------|---------|
| Error response format | Standard error schema |
| HTTP status codes | What each means, how to respond |
| Transient vs permanent | Which errors to retry |
| Retry strategies | Exponential backoff with jitter |
| WebSocket errors | Connection drops, reconnection |
| SDK error handling | Using SDK error classes |

**Code:**
- Retry wrapper function (TS + Python)
- WebSocket reconnection with state recovery

---

#### 13. security.mdx
**Goal:** Ship a secure, compliant integration

**Sections:**
| Section | Content |
|---------|---------|
| API key management | Storage, rotation, never in code |
| Webhook verification | HMAC-SHA256 signature validation |
| Data in transit | TLS requirements |
| Access control | Account IDs, multi-tenant patterns |

**Code:**
- Secure key loading from env
- Complete webhook signature verification (already exists, reference it)

---

### Alpha Tab

#### 14. alpha/overview.mdx
**Goal:** Set expectations for alpha features

**Content:**
- What "alpha" means (experimental, may change)
- Different auth (Bearer token vs X-API-KEY headers)
- How to get alpha access
- Stability expectations
- Feedback channels

---

#### 15. alpha/medical-consensus.mdx
**Goal:** Use consensus API for expert opinions

**Sections:**
| Section | Content |
|---------|---------|
| What it does | Expert opinions on clinical queries |
| Use cases | Second opinions, validating diagnoses |
| Creating a request | POST with query |
| Polling for results | Status flow |
| Understanding results | `consensus_response` field |

**Code:** Complete workflow with polling

---

#### 16. alpha/template-generation.mdx
**Goal:** AI-assisted template creation

**Sections:**
| Section | Content |
|---------|---------|
| Two modes | Generation vs refinement |
| Generation mode | Describe what you want |
| Refinement mode | Submit template + feedback |
| Template output | Returns NoteTemplate schema |
| Iteration workflow | Generate → test → refine |

**Code:** Generation and refinement examples

---

## Part 4: Implementation Plan

### Phase 1: Foundation (Structure + Get Started)
1. Update `docs.json` with new navigation structure
2. Create `quickstart.mdx`
3. Create `typescript.mdx`
4. Create `python.mdx`
5. Move existing `authentication.mdx` to Get Started group

### Phase 2: Core Guides
6. Create `transcription.mdx` (expand from existing snippets)
7. Create `clinical-notes.mdx` (refactor from notes.mdx)
8. Create `note-customization.mdx`
9. Create `webhooks.mdx` (expand from existing)

### Phase 3: Integration Guides
10. Create `ehr-integration.mdx`
11. Create `medical-coding.mdx`
12. Create `multi-language.mdx` (expand from languages.mdx)
13. Create `context-enrichment.mdx`

### Phase 4: Production Guides
14. Create `error-handling.mdx`
15. Create `security.mdx` (consolidate existing webhook security)

### Phase 5: Alpha Documentation
16. Create `alpha/overview.mdx`
17. Create `alpha/medical-consensus.mdx`
18. Create `alpha/template-generation.mdx`

### Phase 6: Code Examples Overhaul
19. Add TypeScript SDK examples to all guides
20. Add Python SDK examples to all guides
21. Convert existing JS snippets to three-tab format

---

## Part 5: File Changes Summary

### New Files to Create (17)
```
documentation/
├── quickstart.mdx
├── guides/
│   ├── transcription.mdx
│   ├── clinical-notes.mdx
│   ├── note-customization.mdx
│   ├── webhooks.mdx
│   ├── ehr-integration.mdx
│   ├── medical-coding.mdx
│   ├── multi-language.mdx
│   └── context-enrichment.mdx
├── production/
│   ├── error-handling.mdx
│   └── security.mdx
├── sdks/
│   ├── typescript.mdx
│   └── python.mdx
└── alpha/
    ├── overview.mdx
    ├── medical-consensus.mdx
    └── template-generation.mdx
```

### Files to Modify
- `docs.json` — Complete navigation overhaul
- `documentation/overview.mdx` — Enhance with links to new guides
- `documentation/notes.mdx` — Deprecate/redirect to new guides

### Files to Keep As-Is
- All `api-reference/` endpoint docs (OpenAPI-generated)
- All `api-reference-alpha/` endpoint docs
- `api-reference/schemas/` — Already good
- `api-reference/events/webhooks.mdx` — Already comprehensive

---

## Success Criteria

1. **New developer can go 0 → working integration in <10 minutes** using quickstart
2. **Every code example has TS SDK, Python SDK, and HTTP tabs**
3. **Navigation follows Mintlify best practices** (tabs → groups → pages, max 3 levels)
4. **SDKs are prominently featured** in Get Started tab
5. **Production guides exist** for error handling and security
6. **Alpha is clearly separated** with appropriate expectations set
