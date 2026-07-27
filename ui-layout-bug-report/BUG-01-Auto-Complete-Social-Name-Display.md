# 🐞 BUG-01 - Social Name Missing Parentheses Wrapper in Auto-Complete Dropdown

## 📌 Defect Overview
* **Severity:** `Medium` (UI/UX Spec Deviation / Layout Formatting)
* **Priority:** `P2` (High Usability Impact)
* **Component:** Unified Search Auto-Complete Component
* **Specification Ref:** Business Requirement Section 1.2.1 / Acceptance Criteria CA-2
* **Target Release:** VersaSaúde v2.14.0

---

## 📝 Defect Description
When typing a citizen's name into the primary search input who has a registered `Social Name` (`Nome Social`), the auto-complete dropdown fails to render the required parenthetical formatting around the official `Name` (`Nome`). 

According to Business Rule **1.2.1** and **CA-2**, the auto-complete primary line must display in the format:  
`Social Name (Official Name)` in **bold font**. 

Currently, the dropdown renders the two strings inline without parentheses, making it difficult for healthcare workers to distinguish the social name from the legal name.

---

## 🔄 Steps to Reproduce
1. Navigate to `Registration -> Citizen` (`Cadastro -> Cidadão`).
2. Focus on the primary unified search input (`Name/Social Name/CNS/CPF`).
3. Type the registered Social Name of a citizen (e.g., `"JUAN PEREIRA"`).
4. Inspect the rendered auto-complete dropdown item list.

---

## ❌ Expected vs. Actual Results

### Expected Result (BR 1.2.1 / CA-2)
The primary line of the auto-complete suggestion renders as:  
**`JUAN PEREIRA (JOAO PEREIRA)`**  
`CPF: 987.654.321-11`

### Actual Result
The primary line renders without parentheses:  
**`JUAN PEREIRA JOAO PEREIRA`**  
`CPF: 987.654.321-11`

---

## 🛠️ Inspector / Technical Findings (DOM / Code Inspection)
Inspecting the element via Chrome DevTools confirms that the text node rendered inside .autocomplete-item-title contains a single string without parenthetical delimiters separating social_name and official_name.
