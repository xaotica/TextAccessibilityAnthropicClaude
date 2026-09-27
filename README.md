# TextAccessibilityAnthropicClaude
Spent a session doing deep research into why Google Docs kept rendering my AI-generated documents with black backgrounds.  

Turns out it's a combination of obscure XML spec behavior, an unfixed bug in the docx-js library, and GDocs silently failing on malformed properties.  

Built a reusable prompt that generates perfectly formatted Google Docs from any content — correct colors, typography, heading hierarchy, accessible contrast ratios — all grounded in HCI and cognitive psychology research.  

Now I just paste content and get a research-backed document out. No manual formatting ever.

Any AI that generates .docx files using docx-js and expects them to import cleanly into Google Docs will hit some version of this problem, because the failure modes are in the library and the format spec, not in the AI's reasoning:

ShadingType.SOLID causing black cells is spec-correct behavior — any model following the docx-js documentation will use it wrong.

The missing color: "auto" on ShadingType.CLEAR is an undocumented requirement — nothing in the docx-js docs flags it.

Issue #2712 (structural XML divergence) is an unfixed library bug that affects all output from docx-js regardless of what the model does.

The TextRun.color character-locking problem in GDocs is a GDocs import behavior that no model would know to avoid without specifically researching it.
