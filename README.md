# flowise-chat-embed
Embed the flowise changed in the link so that it works on Glie
For the universal wrapper: How to use it from Airtable (low-maintenance way)

You already store:

Chat URL Base

Flowise Agent ID

Airtable Record ID (used as sessionId)

Now instead of per-language wrappers, you can have one base:

https://sofia-happy-wibes.github.io/flowise-chat-embed/index-universal.html?lang=


Example Airtable formula for the full URL (without sessionId, since Softr adds it via the script):

"https://sofia-happy-wibes.github.io/flowise-chat-embed/index-universal.html?lang="
& LOWER({LanguageCode})               /* e.g. "en", "it", "fr", "bg", "ar" */
& "&flowId="
& {Flowise Agent ID}


Then Softr script just appends:

fullUrlFromAirtable + "&sessionId=" + recordId


…which is exactly what your current logic already does – you’re just swapping the base to index-universal.html?lang=.

**Universal Wrapper Instructions (index-universal.html)**

This repository contains a universal HTML wrapper used to embed Flowise chat agents inside Softr, Glide, or any website.
It allows full multi-language support and dynamic session tracking.

✔️ 1. Purpose of the Universal Wrapper

index-universal.html is a language-flexible wrapper for Flowise agents.
It provides:

A customizable welcome message (per language)

RTL/LTR support for Arabic

Automatic clearing of local/session storage

Stable visual structure for all agents

Dynamic loading based on Flowise Chatflow ID and Airtable Session ID

This wrapper does NOT force the conversation language.
The agent itself (Flowise chatflow) decides the language of the conversation.
The wrapper only sets the initial welcome message.

✔️ 2. URL Structure

The wrapper is accessed via:

https://YOUR_GITHUB_USERNAME.github.io/flowise-chat-embed/index-universal.html


It accepts the following query parameters:

Parameter	Required	Description
flowId	Yes	The Flowise Chatflow ID for the agent you want to load
sessionId	Yes	Unique session identifier (Airtable record ID) for tracking
lang	Yes	Controls the welcome message language (en, it, fr, bg, ar)
apiHost	Optional	Custom Flowise host (default is the production host defined in the script)
✔️ 3. Example URLs
English agent
index-universal.html?lang=en&flowId=XXXXXXXX&sessionId=recABC123

Italian agent
index-universal.html?lang=it&flowId=XXXXXXXX&sessionId=recDEF456

Arabic (RTL)
index-universal.html?lang=ar&flowId=XXXXXXXX&sessionId=recXYZ987


You can dynamically generate these via Airtable formulas.

✔️ 4. How the Wrapper Works

The wrapper reads the parameters from the URL:

flowId

sessionId

lang

It sets a welcome message based on the language:

English → “Welcome to your training. Ready to go?”

Italian → “Benvenuto/a alla tua formazione. Pronto/a per iniziare?”

French → “Bienvenue dans ta formation. Prêt(e) à commencer ?”

Bulgarian → “Добре дошли в обучението. Готови ли сте да започнем?”

Arabic → RTL layout + Arabic welcome

It initializes the Flowise embed with:

the correct chatflow (via flowId)

the correct session (via sessionId)

the correct welcome message

storage cleared (to enforce clean state)

It does NOT set any system language →
Flowise agent instructions determine conversation language.

✔️ 5. How to Use With Softr

Softr loads the wrapper through an <iframe> created in a small JS embed script.

The Softr block script:

Reads values from Airtable displayed on the page:

Base URL (containing index-universal.html?lang=…&flowId=)

Airtable record ID for sessionId

Builds the full URL

Injects the wrapper as an iframe

This makes the system fully dynamic and multi-agent-compatible.

✔️ 6. How to Use With Airtable

Airtable stores:

Each agent’s Flowise Chatflow ID

Each agent’s Language

A Chat URL Base, for example:

"https://sofia-happy-wibes.github.io/flowise-chat-embed/index-universal.html?lang=" 
& LOWER({Language}) 
& "&flowId=" 
& {Flowise Agent ID}


Softr then appends the correct sessionId.

This ensures each training session is tied to the correct Flowise agent and evaluated properly.

✔️ 7. Adding More Languages

The universal wrapper can be extended at any time.

Simply add a new case to:

function getWelcomeMessage(lang) { ... }


and (if needed) RTL direction logic.

✔️ 8. Notes

The wrapper is frontend-only → safe & lightweight.

It does not expose Flowise API keys or secrets.

Each Flowise agent maintains full autonomy.

The wrapper's only job is welcome message & UI presentation.
