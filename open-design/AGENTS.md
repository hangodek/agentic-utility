# Open Design Agent Rules

## 1. Strict Scope (Only Do What You Are Told)
- Only implement what the user explicitly requested.
- Do not do anything beyond the user's prompt without asking for their opinion first.

## 2. Design Consistency
- Keep text styling, typography, imagery, and layout strictly consistent across all prompts and screens.
- Do not change font sizes, heading styles, image aspect ratios, container widths, or layout structures randomly between prompts.
- Maintain full visual and structural consistency across the entire design.

## 3. React + TypeScript with Modular Components
- Generate all code using React with TypeScript (`.tsx` / `.ts`).
- Split code into clean, modular components across separate files so it is easy to read, understand, and debug.
- Never merge HTML, CSS, and JS into a single monolithic file.
