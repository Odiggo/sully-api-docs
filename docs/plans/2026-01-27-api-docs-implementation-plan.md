# API Documentation Overhaul Implementation Plan

> **For Claude:** REQUIRED SUB-SKILL: Use superpowers:executing-plans to implement this plan task-by-task.

**Goal:** Transform Sully.ai docs from API-reference-heavy to use-case-driven with proper Mintlify structure, SDK examples, and production guides.

**Architecture:** Tab-based navigation (Get Started → Guides → API Reference → Alpha), each guide follows standard template with TS SDK + Python SDK + HTTP code examples.

**Tech Stack:** Mintlify, MDX, TypeScript SDK (@sullyai/sullyai), Python SDK (sullyai)

---

## Phase 1: Foundation — Navigation Structure

### Task 1.1: Create Directory Structure

**Files:**
- Create: `documentation/guides/` (directory)
- Create: `documentation/production/` (directory)
- Create: `documentation/sdks/` (directory)
- Create: `documentation/alpha/` (directory)

**Step 1: Create directories**

```bash
mkdir -p documentation/guides documentation/production documentation/sdks documentation/alpha
```

**Step 2: Verify directories exist**

```bash
ls -la documentation/
```

Expected: All 4 directories present

**Step 3: Commit**

```bash
git add documentation/
git commit -m "chore: create directory structure for new documentation"
```

---

### Task 1.2: Update docs.json Navigation

**Files:**
- Modify: `docs.json`

**Step 1: Update docs.json with new tab-based navigation**

Replace the entire `navigation` section with the new structure. Key changes:
- Remove version-first navigation
- Add 4 tabs: Get Started, Guides, API Reference, Alpha
- Organize pages into logical groups

```json
{
  "$schema": "https://mintlify.com/docs.json",
  "theme": "mint",
  "name": "Sully.ai",
  "colors": {
    "primary": "#0C57F5",
    "light": "#0C57F5",
    "dark": "#0C57F5"
  },
  "favicon": "/favicon.png",
  "navigation": {
    "tabs": [
      {
        "tab": "Get Started",
        "groups": [
          {
            "group": "Introduction",
            "pages": [
              "documentation/overview",
              "documentation/quickstart",
              "api-reference/getting-started/authentication"
            ]
          },
          {
            "group": "SDKs",
            "pages": [
              "documentation/sdks/typescript",
              "documentation/sdks/python"
            ]
          }
        ]
      },
      {
        "tab": "Guides",
        "groups": [
          {
            "group": "Core Workflows",
            "pages": [
              "documentation/guides/transcription",
              "documentation/guides/clinical-notes",
              "documentation/guides/note-customization",
              "documentation/guides/webhooks"
            ]
          },
          {
            "group": "Integrations",
            "pages": [
              "documentation/guides/ehr-integration",
              "documentation/guides/medical-coding",
              "documentation/guides/multi-language",
              "documentation/guides/context-enrichment"
            ]
          },
          {
            "group": "Production",
            "pages": [
              "documentation/production/error-handling",
              "documentation/production/security"
            ]
          }
        ]
      },
      {
        "tab": "API Reference",
        "groups": [
          {
            "group": "Audio Transcriptions",
            "pages": [
              "api-reference-v2/audio-transcriptions/create",
              "api-reference-v2/audio-transcriptions/get",
              "api-reference-v2/audio-transcriptions/delete",
              "api-reference/audio-transcriptions/streaming",
              "api-reference/audio-transcriptions/stream-token",
              "api-reference/audio-transcriptions/languages"
            ]
          },
          {
            "group": "Notes",
            "pages": [
              "api-reference/notes/create",
              "api-reference/notes/get",
              "api-reference/notes/delete"
            ]
          },
          {
            "group": "Note Styles",
            "pages": [
              "api-reference/note-styles/create"
            ]
          },
          {
            "group": "Codings",
            "pages": [
              "api-reference/codings/create",
              "api-reference/codings/get",
              "api-reference/codings/delete"
            ]
          },
          {
            "group": "Macros",
            "pages": [
              "api-reference/macros/detect"
            ]
          },
          {
            "group": "Utils",
            "pages": [
              "api-reference/utils/text-to-json"
            ]
          },
          {
            "group": "Events",
            "pages": [
              "api-reference/events/overview",
              "api-reference/events/webhooks"
            ]
          },
          {
            "group": "Schemas",
            "pages": [
              "api-reference/schemas/soap-note",
              "api-reference/schemas/note-template"
            ]
          }
        ]
      },
      {
        "tab": "Alpha",
        "groups": [
          {
            "group": "Overview",
            "pages": [
              "documentation/alpha/overview"
            ]
          },
          {
            "group": "Alpha Guides",
            "pages": [
              "documentation/alpha/medical-consensus",
              "documentation/alpha/template-generation"
            ]
          },
          {
            "group": "Alpha API Reference",
            "pages": [
              "api-reference-alpha/getting-started/authentication",
              "api-reference-alpha/consensus/create",
              "api-reference-alpha/consensus/get",
              "api-reference-alpha/consensus/delete",
              "api-reference-alpha/note-templates/create",
              "api-reference-alpha/note-templates/get",
              "api-reference-alpha/note-templates/delete"
            ]
          }
        ]
      }
    ],
    "global": {
      "anchors": [
        {
          "anchor": "Dashboard",
          "href": "https://dashboard.api.sully.ai",
          "icon": "gauge"
        },
        {
          "anchor": "Demo",
          "href": "https://github.com/Odiggo/sully-api-demo",
          "icon": "github"
        },
        {
          "anchor": "Blog",
          "href": "https://www.sully.ai/blog",
          "icon": "newspaper"
        }
      ]
    }
  },
  "logo": {
    "light": "/logo/light.svg",
    "dark": "/logo/dark.svg"
  },
  "navbar": {
    "links": [
      {
        "label": "Support",
        "href": "mailto:support@sully.ai"
      }
    ],
    "primary": {
      "type": "button",
      "label": "Dashboard",
      "href": "https://dashboard.api.sully.ai"
    }
  },
  "footer": {
    "socials": {
      "x": "https://x.com/sully_the_ai",
      "linkedin": "https://www.linkedin.com/company/sully-ai"
    }
  },
  "api": {
    "openapi": "https://raw.githubusercontent.com/Odiggo/sullyai-openapi/refs/heads/main/openapi.yaml"
  }
}
```

**Step 2: Validate JSON syntax**

```bash
cat docs.json | python3 -m json.tool > /dev/null && echo "Valid JSON"
```

Expected: "Valid JSON"

**Step 3: Commit**

```bash
git add docs.json
git commit -m "feat: restructure navigation to tab-based layout

- Replace version-first with tab-based navigation
- Add Get Started, Guides, API Reference, Alpha tabs
- Add Dashboard anchor for quick access"
```

---

## Phase 2: Get Started Tab

### Task 2.1: Create quickstart.mdx

**Files:**
- Create: `documentation/quickstart.mdx`

**Step 1: Create quickstart guide**

```mdx
---
title: 'Quickstart'
description: 'Go from zero to clinical note in 5 minutes'
---

Get your first clinical note from audio in just a few steps.

## Prerequisites

- A Sully.ai account ([sign up here](https://dashboard.api.sully.ai))
- Your API key and Account ID from the dashboard
- Node.js 18+ or Python 3.8+

## 1. Install the SDK

<CodeGroup>

```bash TypeScript
npm install @sullyai/sullyai
```

```bash Python
pip install sullyai
```

</CodeGroup>

## 2. Set Your Credentials

<CodeGroup>

```bash TypeScript
export SULLY_API_KEY="your-api-key"
export SULLY_ACCOUNT_ID="your-account-id"
```

```bash Python
export SULLY_API_KEY="your-api-key"
export SULLY_ACCOUNT_ID="your-account-id"
```

</CodeGroup>

## 3. Transcribe Audio and Generate a Note

<CodeGroup>

```typescript TypeScript SDK
import SullyAI from '@sullyai/sullyai';
import * as fs from 'fs';

const client = new SullyAI();

async function main() {
  // Step 1: Upload audio for transcription
  const audioFile = fs.createReadStream('patient-visit.mp3');
  const transcription = await client.audio.transcriptions.create({
    file: audioFile,
    language: 'en'
  });

  // Step 2: Poll until transcription completes
  let result = await client.audio.transcriptions.retrieve(transcription.id);
  while (result.status === 'pending' || result.status === 'processing') {
    await new Promise(resolve => setTimeout(resolve, 1000));
    result = await client.audio.transcriptions.retrieve(transcription.id);
  }

  // Step 3: Generate a SOAP note
  const note = await client.notes.create({
    transcript: result.transcription,
    noteType: { format: 'soap' }
  });

  // Step 4: Poll until note completes
  let noteResult = await client.notes.retrieve(note.id);
  while (noteResult.status === 'pending' || noteResult.status === 'processing') {
    await new Promise(resolve => setTimeout(resolve, 1000));
    noteResult = await client.notes.retrieve(note.id);
  }

  console.log('Clinical Note:');
  console.log(noteResult.payload.markdown);
}

main();
```

```python Python SDK
import time
from sullyai import SullyAI

client = SullyAI()

# Step 1: Upload audio for transcription
with open('patient-visit.mp3', 'rb') as audio_file:
    transcription = client.audio.transcriptions.create(
        file=audio_file,
        language='en'
    )

# Step 2: Poll until transcription completes
result = client.audio.transcriptions.retrieve(transcription.id)
while result.status in ['pending', 'processing']:
    time.sleep(1)
    result = client.audio.transcriptions.retrieve(transcription.id)

# Step 3: Generate a SOAP note
note = client.notes.create(
    transcript=result.transcription,
    note_type={'format': 'soap'}
)

# Step 4: Poll until note completes
note_result = client.notes.retrieve(note.id)
while note_result.status in ['pending', 'processing']:
    time.sleep(1)
    note_result = client.notes.retrieve(note.id)

print('Clinical Note:')
print(note_result.payload.markdown)
```

```bash HTTP (cURL)
# Step 1: Upload audio
TRANSCRIPTION_ID=$(curl -X POST "https://api.sully.ai/v2/audio/transcriptions" \
  -H "X-API-Key: $SULLY_API_KEY" \
  -H "X-Account-Id: $SULLY_ACCOUNT_ID" \
  -F "file=@patient-visit.mp3" \
  -F "language=en" | jq -r '.data.id')

# Step 2: Poll for transcription (repeat until status is "completed")
curl "https://api.sully.ai/v2/audio/transcriptions/$TRANSCRIPTION_ID" \
  -H "X-API-Key: $SULLY_API_KEY" \
  -H "X-Account-Id: $SULLY_ACCOUNT_ID"

# Step 3: Create note (after transcription completes)
NOTE_ID=$(curl -X POST "https://api.sully.ai/v1/notes" \
  -H "X-API-Key: $SULLY_API_KEY" \
  -H "X-Account-Id: $SULLY_ACCOUNT_ID" \
  -H "Content-Type: application/json" \
  -d '{
    "transcript": "YOUR_TRANSCRIPTION_TEXT",
    "noteType": { "format": "soap" }
  }' | jq -r '.data.id')

# Step 4: Get the note (after note generation completes)
curl "https://api.sully.ai/v1/notes/$NOTE_ID" \
  -H "X-API-Key: $SULLY_API_KEY" \
  -H "X-Account-Id: $SULLY_ACCOUNT_ID"
```

</CodeGroup>

## What Just Happened?

1. **Transcribed** your audio file using Sully's medical-optimized speech recognition
2. **Generated** a structured SOAP note from the conversation
3. **Retrieved** the formatted clinical documentation

## Next Steps

<CardGroup cols={2}>
  <Card title="Real-time Streaming" icon="microphone" href="/documentation/guides/transcription">
    Stream audio live instead of uploading files
  </Card>
  <Card title="Customize Notes" icon="pen" href="/documentation/guides/note-customization">
    Create custom note styles and templates
  </Card>
  <Card title="TypeScript SDK" icon="js" href="/documentation/sdks/typescript">
    Deep dive into the TypeScript SDK
  </Card>
  <Card title="Python SDK" icon="python" href="/documentation/sdks/python">
    Deep dive into the Python SDK
  </Card>
</CardGroup>
```

**Step 2: Verify file exists and has content**

```bash
wc -l documentation/quickstart.mdx
```

Expected: ~150 lines

**Step 3: Commit**

```bash
git add documentation/quickstart.mdx
git commit -m "docs: add quickstart guide

- Complete 0-to-note example in TypeScript, Python, and cURL
- Polling pattern for async operations
- Links to next steps"
```

---

### Task 2.2: Create TypeScript SDK Guide

**Files:**
- Create: `documentation/sdks/typescript.mdx`

**Step 1: Create TypeScript SDK guide**

```mdx
---
title: 'TypeScript SDK'
description: 'Official TypeScript/JavaScript SDK for the Sully.ai API'
---

The official TypeScript SDK provides type-safe access to the Sully.ai API with built-in error handling and automatic retries.

## Installation

```bash
npm install @sullyai/sullyai
```

## Quick Setup

```typescript
import SullyAI from '@sullyai/sullyai';

// Uses SULLY_API_KEY and SULLY_ACCOUNT_ID from environment
const client = new SullyAI();

// Or configure explicitly
const client = new SullyAI({
  apiKey: 'your-api-key',
  accountId: 'your-account-id'
});
```

## Audio Transcriptions

### Upload a File

```typescript
import * as fs from 'fs';

const file = fs.createReadStream('audio.mp3');

const transcription = await client.audio.transcriptions.create({
  file,
  language: 'en'
});

console.log(`Transcription ID: ${transcription.id}`);
console.log(`Status: ${transcription.status}`);
```

### Poll for Completion

```typescript
async function waitForTranscription(id: string) {
  let result = await client.audio.transcriptions.retrieve(id);

  while (result.status === 'pending' || result.status === 'processing') {
    await new Promise(resolve => setTimeout(resolve, 1000));
    result = await client.audio.transcriptions.retrieve(id);
  }

  if (result.status === 'failed') {
    throw new Error(`Transcription failed: ${result.error}`);
  }

  return result;
}

const completed = await waitForTranscription(transcription.id);
console.log(completed.transcription);
```

### Delete a Transcription

```typescript
await client.audio.transcriptions.delete(transcription.id);
```

## Notes

### Create a SOAP Note

```typescript
const note = await client.notes.create({
  transcript: 'Patient presents with...',
  noteType: { format: 'soap' }
});
```

### Create a Note with Context

```typescript
const note = await client.notes.create({
  transcript: transcriptionText,
  noteType: { format: 'soap' },
  date: '2026-01-27',
  patientInfo: {
    name: 'John Doe',
    dateOfBirth: '1980-05-15',
    gender: 'male'
  },
  medicationList: ['Lisinopril 10mg', 'Metformin 500mg'],
  instructions: 'Focus on cardiovascular symptoms'
});
```

### Retrieve a Note

```typescript
const note = await client.notes.retrieve(noteId);

console.log(note.status);
console.log(note.payload?.markdown);
console.log(note.payload?.json);
```

## Error Handling

The SDK provides typed error classes for different failure scenarios:

```typescript
import SullyAI, {
  APIError,
  AuthenticationError,
  RateLimitError,
  BadRequestError,
  NotFoundError
} from '@sullyai/sullyai';

try {
  const note = await client.notes.create({ transcript: '' });
} catch (error) {
  if (error instanceof AuthenticationError) {
    console.error('Invalid API key or account ID');
  } else if (error instanceof RateLimitError) {
    console.error('Rate limited. Retry after:', error.headers?.['retry-after']);
  } else if (error instanceof BadRequestError) {
    console.error('Invalid request:', error.message);
  } else if (error instanceof NotFoundError) {
    console.error('Resource not found');
  } else if (error instanceof APIError) {
    console.error('API error:', error.status, error.message);
  }
}
```

## Automatic Retries

The SDK automatically retries failed requests up to 2 times with exponential backoff. Customize this behavior:

```typescript
const client = new SullyAI({
  maxRetries: 3 // Default is 2
});
```

Retries apply to:
- Connection errors
- 408 Request Timeout
- 429 Rate Limit (with Retry-After header)
- 500+ Server Errors

## Timeouts

Configure request timeouts:

```typescript
const client = new SullyAI({
  timeout: 60000 // 60 seconds (default)
});
```

## TypeScript Benefits

Full type definitions for all requests and responses:

```typescript
import type {
  Note,
  AudioTranscription,
  NoteCreateParams
} from '@sullyai/sullyai';

// Type-safe parameters
const params: NoteCreateParams = {
  transcript: 'Patient visit...',
  noteType: { format: 'soap' }
};

// Typed responses
const note: Note = await client.notes.create(params);
```

## Next Steps

<CardGroup cols={2}>
  <Card title="Transcription Guide" icon="microphone" href="/documentation/guides/transcription">
    File uploads and real-time streaming
  </Card>
  <Card title="Error Handling" icon="triangle-exclamation" href="/documentation/production/error-handling">
    Production error handling patterns
  </Card>
</CardGroup>
```

**Step 2: Commit**

```bash
git add documentation/sdks/typescript.mdx
git commit -m "docs: add TypeScript SDK guide

- Installation and setup
- Audio transcriptions API
- Notes API with context
- Error handling with typed exceptions
- Retry and timeout configuration"
```

---

### Task 2.3: Create Python SDK Guide

**Files:**
- Create: `documentation/sdks/python.mdx`

**Step 1: Create Python SDK guide**

```mdx
---
title: 'Python SDK'
description: 'Official Python SDK for the Sully.ai API'
---

The official Python SDK provides synchronous and asynchronous clients with Pydantic response models and automatic retries.

## Installation

```bash
pip install sullyai
```

## Quick Setup

```python
from sullyai import SullyAI

# Uses SULLY_API_KEY and SULLY_ACCOUNT_ID from environment
client = SullyAI()

# Or configure explicitly
client = SullyAI(
    api_key='your-api-key',
    account_id='your-account-id'
)
```

## Sync vs Async

The SDK provides both synchronous and asynchronous clients:

<CodeGroup>

```python Sync
from sullyai import SullyAI

client = SullyAI()
note = client.notes.create(transcript='...')
```

```python Async
from sullyai import AsyncSullyAI
import asyncio

async def main():
    client = AsyncSullyAI()
    note = await client.notes.create(transcript='...')

asyncio.run(main())
```

</CodeGroup>

## Audio Transcriptions

### Upload a File

```python
with open('audio.mp3', 'rb') as f:
    transcription = client.audio.transcriptions.create(
        file=f,
        language='en'
    )

print(f"Transcription ID: {transcription.id}")
print(f"Status: {transcription.status}")
```

### Poll for Completion

```python
import time

def wait_for_transcription(transcription_id: str):
    result = client.audio.transcriptions.retrieve(transcription_id)

    while result.status in ['pending', 'processing']:
        time.sleep(1)
        result = client.audio.transcriptions.retrieve(transcription_id)

    if result.status == 'failed':
        raise Exception(f"Transcription failed: {result.error}")

    return result

completed = wait_for_transcription(transcription.id)
print(completed.transcription)
```

### Async Polling

```python
import asyncio
from sullyai import AsyncSullyAI

async def wait_for_transcription(client, transcription_id: str):
    result = await client.audio.transcriptions.retrieve(transcription_id)

    while result.status in ['pending', 'processing']:
        await asyncio.sleep(1)
        result = await client.audio.transcriptions.retrieve(transcription_id)

    return result
```

## Notes

### Create a SOAP Note

```python
note = client.notes.create(
    transcript='Patient presents with...',
    note_type={'format': 'soap'}
)
```

### Create a Note with Context

```python
note = client.notes.create(
    transcript=transcription_text,
    note_type={'format': 'soap'},
    date='2026-01-27',
    patient_info={
        'name': 'John Doe',
        'date_of_birth': '1980-05-15',
        'gender': 'male'
    },
    medication_list=['Lisinopril 10mg', 'Metformin 500mg'],
    instructions='Focus on cardiovascular symptoms'
)
```

### Retrieve a Note

```python
note = client.notes.retrieve(note_id)

print(note.status)
print(note.payload.markdown if note.payload else None)
print(note.payload.json if note.payload else None)
```

## Response Handling

Responses are Pydantic models with helpful methods:

```python
note = client.notes.retrieve(note_id)

# Convert to dictionary
note_dict = note.to_dict()

# Convert to JSON string
note_json = note.to_json()

# Check which fields were set vs null
note.model_fields_set
```

## Error Handling

```python
from sullyai import (
    SullyAI,
    APIError,
    AuthenticationError,
    RateLimitError,
    BadRequestError,
    NotFoundError,
    APIConnectionError
)

try:
    note = client.notes.create(transcript='')
except AuthenticationError:
    print('Invalid API key or account ID')
except RateLimitError as e:
    print(f'Rate limited. Retry after: {e.response.headers.get("retry-after")}')
except BadRequestError as e:
    print(f'Invalid request: {e.message}')
except NotFoundError:
    print('Resource not found')
except APIConnectionError:
    print('Connection failed')
except APIError as e:
    print(f'API error: {e.status_code} - {e.message}')
```

## Automatic Retries

The SDK automatically retries failed requests up to 2 times:

```python
client = SullyAI(
    max_retries=3  # Default is 2
)
```

## Timeouts

Configure request timeouts:

```python
client = SullyAI(
    timeout=60.0  # 60 seconds
)

# Or per-request
note = client.notes.create(
    transcript='...',
    timeout=120.0
)
```

## Next Steps

<CardGroup cols={2}>
  <Card title="Transcription Guide" icon="microphone" href="/documentation/guides/transcription">
    File uploads and real-time streaming
  </Card>
  <Card title="Error Handling" icon="triangle-exclamation" href="/documentation/production/error-handling">
    Production error handling patterns
  </Card>
</CardGroup>
```

**Step 2: Commit**

```bash
git add documentation/sdks/python.mdx
git commit -m "docs: add Python SDK guide

- Sync and async client usage
- Audio transcriptions API
- Notes API with context
- Pydantic response handling
- Error handling with exceptions"
```

---

## Phase 3: Core Workflow Guides

### Task 3.1: Create Transcription Guide

**Files:**
- Create: `documentation/guides/transcription.mdx`

**Content focus:**
- File upload (formats, size limits, polling)
- Real-time WebSocket streaming (token auth, connection lifecycle)
- Production streaming (reconnection, error recovery, buffering)
- Language support (BCP47 tags, multilingual)
- Decision matrix: upload vs stream

**Key code sections:**
- Basic file upload with SDK
- Production WebSocket with exponential backoff reconnection
- Language specification examples

**Commit message:** `docs: add transcription guide with streaming patterns`

---

### Task 3.2: Create Clinical Notes Guide

**Files:**
- Create: `documentation/guides/clinical-notes.mdx`

**Content focus:**
- SOAP notes (built-in format)
- Custom note styles (from sample)
- Note templates (structured sections)
- Decision flowchart for choosing approach
- Polling vs webhooks for retrieval

**Key code sections:**
- SOAP note generation
- Creating and using custom styles
- Template-based notes

**Commit message:** `docs: add clinical notes guide covering SOAP, styles, templates`

---

### Task 3.3: Create Note Customization Guide

**Files:**
- Create: `documentation/guides/note-customization.mdx`

**Content focus:**
- Styles deep dive (how Sully learns from samples)
- Templates deep dive (section types, hierarchy)
- Template schema reference
- Complete template examples for common specialties
- Migrating from styles to templates

**Commit message:** `docs: add note customization deep dive`

---

### Task 3.4: Create Webhooks Guide

**Files:**
- Create: `documentation/guides/webhooks.mdx`

**Content focus:**
- Why webhooks (vs polling)
- All 6 event types with payload examples
- HMAC-SHA256 signature verification (CRITICAL)
- Idempotent event handling
- Production webhook handler

**Key code:** Reuse/reference existing webhook verification code from api-reference/events/webhooks.mdx

**Commit message:** `docs: add webhooks guide for production event handling`

---

## Phase 4: Integration Guides

### Task 4.1: Create EHR Integration Guide

**Files:**
- Create: `documentation/guides/ehr-integration.mdx`

**Content focus:**
- JSON output from SOAP notes
- JSON output from templates
- text-to-json utility
- Field mapping patterns

**Commit message:** `docs: add EHR integration guide for structured JSON output`

---

### Task 4.2: Create Medical Coding Guide

**Files:**
- Create: `documentation/guides/medical-coding.mdx`

**Content focus:**
- ICD-10-CM, CPT, SNOMED CT extraction
- Creating coding requests
- Understanding results (diagnoses, procedures, text spans)
- Combining coding with notes workflow

**Commit message:** `docs: add medical coding guide for ICD-10, CPT, SNOMED`

---

### Task 4.3: Create Multi-Language Guide

**Files:**
- Create: `documentation/guides/multi-language.mdx`

**Content focus:**
- Full language list with BCP47 tags
- Single-language mode
- Multilingual mode (`multi` tag)
- Language with streaming

**Commit message:** `docs: add multi-language guide for international support`

---

### Task 4.4: Create Context Enrichment Guide

**Files:**
- Create: `documentation/guides/context-enrichment.mdx`

**Content focus:**
- All context fields (patientInfo, medicationList, previousNote, instructions, context, date)
- How each field improves note quality
- Complete example with all fields

**Commit message:** `docs: add context enrichment guide for improved note quality`

---

## Phase 5: Production Guides

### Task 5.1: Create Error Handling Guide

**Files:**
- Create: `documentation/production/error-handling.mdx`

**Content focus:**
- Error response format
- HTTP status codes and meanings
- Transient vs permanent errors
- Retry strategies with exponential backoff
- WebSocket error recovery
- SDK error classes

**Commit message:** `docs: add production error handling guide`

---

### Task 5.2: Create Security Guide

**Files:**
- Create: `documentation/production/security.mdx`

**Content focus:**
- API key management (env vars, never in code)
- Webhook signature verification (reference existing code)
- TLS requirements
- Account ID patterns

**Commit message:** `docs: add security guide for production deployments`

---

## Phase 6: Alpha Documentation

### Task 6.1: Create Alpha Overview

**Files:**
- Create: `documentation/alpha/overview.mdx`

**Content focus:**
- What "alpha" means
- Different auth (Bearer token)
- Stability expectations
- Feedback channels

**Commit message:** `docs: add alpha overview with expectations`

---

### Task 6.2: Create Medical Consensus Guide

**Files:**
- Create: `documentation/alpha/medical-consensus.mdx`

**Content focus:**
- What consensus does
- Creating requests
- Polling for results
- Understanding responses

**Commit message:** `docs: add medical consensus alpha guide`

---

### Task 6.3: Create Template Generation Guide

**Files:**
- Create: `documentation/alpha/template-generation.mdx`

**Content focus:**
- Generation vs refinement modes
- Creating templates from description
- Refining existing templates
- Iteration workflow

**Commit message:** `docs: add template generation alpha guide`

---

## Phase 7: Finalization

### Task 7.1: Update Overview Page

**Files:**
- Modify: `documentation/overview.mdx`

**Changes:**
- Add links to new guides
- Update navigation hints
- Add CardGroup for quick access

**Commit message:** `docs: update overview with links to new guides`

---

### Task 7.2: Deprecate Old Notes Page

**Files:**
- Modify: `documentation/notes.mdx`

**Changes:**
- Add deprecation notice at top
- Redirect to new guides

**Commit message:** `docs: deprecate notes.mdx in favor of new guides`

---

### Task 7.3: Final Review and Cleanup

**Steps:**
1. Run Mintlify dev server: `npx mintlify dev`
2. Navigate all pages, verify no broken links
3. Check all code examples render correctly
4. Verify navigation structure matches plan

**Commit message:** `docs: final review and link fixes`

---

## Summary

| Phase | Tasks | Files |
|-------|-------|-------|
| 1. Foundation | 2 | docs.json, directories |
| 2. Get Started | 3 | quickstart, TS SDK, Python SDK |
| 3. Core Workflows | 4 | transcription, clinical-notes, note-customization, webhooks |
| 4. Integrations | 4 | ehr-integration, medical-coding, multi-language, context-enrichment |
| 5. Production | 2 | error-handling, security |
| 6. Alpha | 3 | overview, medical-consensus, template-generation |
| 7. Finalization | 3 | overview update, notes deprecation, review |

**Total: 21 tasks, 17 new files, 3 modified files**
