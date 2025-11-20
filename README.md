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
