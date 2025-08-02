# Code Guardian Aegis (VibeCoding新手盾)

## Project Description

This project aims to prevent serious security issues that may arise when non-engineers use VibeCoding. With the widespread adoption of AI code generation tools, non-professional developers may inadvertently introduce security vulnerabilities during development due to insufficient security awareness, exposing systems to potential threats. Specifically optimized security auditing for common novice issues, providing targeted protection mechanisms.

## Project Goals

Provide comprehensive security protection mechanisms to ensure VibeCoding development projects comply with security best practices:

1. **Pre-development Prevention**: Provide security guidelines and documentation to establish security baselines during development
2. **Pre-deployment Verification**: Provide automated security audit tools for comprehensive detection of potential project risks

## Project Structure

```
├── security-guidelines-documents/  # Security Guidelines Documents
│   ├── Security-Guidelines_English.md         # Security Development Guidelines (for AI and developers)
│   ├── Security-Guidelines-Explanation_English.md  # Detailed explanation of security rules
│   ├── 安全守則_繁體中文.md         # 安全開發守則 (供 AI 和開發者使用)
│   └── 安全守則說明_繁體中文.md     # 安全守則詳細說明與原理解釋
├── security-audit-meta-prompt/     # Security Audit Meta Prompt
│   ├── Security-Audit-Prompt_English.md      # Project security audit instruction template
│   └── 安全審計提示_繁體中文.md     # 專案安全審計指令模板
├── README.md                       # Project Documentation
└── CHANGELOG.md                    # Change Log
```

## File Descriptions

### Security Guidelines Documents (security-guidelines-documents/)
- **[Security-Guidelines_English.md](./security-guidelines-documents/Security-Guidelines_English.md)** - Core security development checklist for developers and AI assistants to follow during development
- **[Security-Guidelines-Explanation_English.md](./security-guidelines-documents/Security-Guidelines-Explanation_English.md)** - Detailed explanation of each security rule's importance, including hacker attack scenarios and disaster consequence analysis
- **[安全守則_繁體中文.md](./security-guidelines-documents/安全守則_繁體中文.md)** - 核心安全開發檢查清單，供開發者和 AI 助手在開發過程中遵循
- **[安全守則說明_繁體中文.md](./security-guidelines-documents/安全守則說明_繁體中文.md)** - 詳細解釋每條安全規則的重要性，包含駭客攻擊劇本和災難後果分析

### Security Audit Tools (security-audit-meta-prompt/)
- **[Security-Audit-Prompt_English.md](./security-audit-meta-prompt/Security-Audit-Prompt_English.md)** - Complete security audit instruction template for advanced AI models to conduct comprehensive project security checks
- **[安全審計提示_繁體中文.md](./security-audit-meta-prompt/安全審計提示_繁體中文.md)** - 完整的安全審計指令模板，用於讓高級 AI 模型進行全面的專案安全檢查

## Usage

### Pre-development Phase
1. Place security guideline files from `security-guidelines-documents/` into your VibeCoding project root directory
2. Read the security guidelines thoroughly to ensure development follows security standards
3. **Let VibeCoding Agent also read the security guidelines**: Ensure AI assistants follow security standards when generating code

### Pre-deployment Phase
1. Use audit tools from `security-audit-meta-prompt/`
2. Have advanced programming models read and audit the entire project's security
3. Fix security issues found in the audit report

## Importance

- **Risk Reduction**: Minimize security vulnerabilities caused by lack of security knowledge
- **Quality Improvement**: Ensure code meets industry security standards
- **Asset Protection**: Prevent data breaches or system intrusions due to security issues
- **Compliance Requirements**: Meet enterprise or organizational security compliance needs

## Contributing

Welcome security-related improvement suggestions, new detection rules, or best practice cases. Please ensure all contributions are thoroughly security-verified.

## License and Copyright

This project content is co-created by Cyesuta and Google Gemini Pro AI.

This project is open source licensed, aiming to promote secure development practices and protect the interests of the entire developer community. Free to use, modify, and distribute, but please retain copyright notices.

## Multi-language Support

This project provides versions in the following languages:
- English
- Traditional Chinese (繁體中文)
- Simplified Chinese (簡體中文)
- Japanese (日本語)
- German (Deutsch)
- French (Français)
- Russian (Русский)
- Italian (Italiano)
- Korean (한국어)
- Spanish (Español)
- Portuguese (Português)
- Arabic (العربية)
- Dutch (Nederlands)
- Thai (ไทย)
- Vietnamese (Tiếng Việt)

---

# Code Guardian Aegis (VibeCoding新手盾)

## 專案描述

本專案旨在防止非工程師使用 VibeCoding 時可能引發的嚴重資安問題。隨著 AI 程式碼生成工具的普及，非專業開發者在缺乏足夠資安意識的情況下進行開發，可能無意中引入安全漏洞，導致系統暴露於潛在威脅之下。特別針對新手常見問題優化安全審計，提供針對性的防護機制。

## 專案目標

提供完整的安全防護機制，確保使用 VibeCoding 開發的專案符合資安最佳實務：

1. **開發前預防**：提供安全守則與指導文件，建立開發時的安全基準
2. **上線前檢核**：提供自動化安全審計工具，全面檢測專案潛在風險

## 專案結構

```
├── security-guidelines-documents/  # 安全守則文件
│   ├── Security-Guidelines_English.md         # Security Development Guidelines (for AI and developers)
│   ├── Security-Guidelines-Explanation_English.md  # Detailed explanation of security rules
│   ├── 安全守則_繁體中文.md         # 安全開發守則 (供 AI 和開發者使用)
│   └── 安全守則說明_繁體中文.md     # 安全守則詳細說明與原理解釋
├── security-audit-meta-prompt/     # 安全審計元提示
│   ├── Security-Audit-Prompt_English.md      # Project security audit instruction template
│   └── 安全審計提示_繁體中文.md     # 專案安全審計指令模板
├── README.md                       # 專案說明文件
└── CHANGELOG.md                    # 更新日誌
```

## 文件說明

### 安全守則文件 (security-guidelines-documents/)
- **[Security-Guidelines_English.md](./security-guidelines-documents/Security-Guidelines_English.md)** - Core security development checklist for developers and AI assistants to follow during development
- **[Security-Guidelines-Explanation_English.md](./security-guidelines-documents/Security-Guidelines-Explanation_English.md)** - Detailed explanation of each security rule's importance, including hacker attack scenarios and disaster consequence analysis
- **[安全守則_繁體中文.md](./security-guidelines-documents/安全守則_繁體中文.md)** - 核心安全開發檢查清單，供開發者和 AI 助手在開發過程中遵循
- **[安全守則說明_繁體中文.md](./security-guidelines-documents/安全守則說明_繁體中文.md)** - 詳細解釋每條安全規則的重要性，包含駭客攻擊劇本和災難後果分析

### 安全審計工具 (security-audit-meta-prompt/)
- **[Security-Audit-Prompt_English.md](./security-audit-meta-prompt/Security-Audit-Prompt_English.md)** - Complete security audit instruction template for advanced AI models to conduct comprehensive project security checks
- **[安全審計提示_繁體中文.md](./security-audit-meta-prompt/安全審計提示_繁體中文.md)** - 完整的安全審計指令模板，用於讓高級 AI 模型進行全面的專案安全檢查

## 使用方法

### 開發前階段
1. 將 `security-guidelines-documents/` 中的安全守則文件放入您的 VibeCoding 專案根目錄
2. 詳細閱讀安全守則說明，確保開發過程遵循安全規範
3. **讓 VibeCoding Agent 也讀取安全守則**：確保 AI 助手在生成程式碼時也能遵循安全規範

### 上線前階段
1. 使用 `security-audit-meta-prompt/` 中的審計工具
2. 讓高級程式設計模型讀取並審計整個專案的安全性
3. 根據審計報告修復發現的安全問題

## 重要性

- **降低風險**：減少因缺乏資安知識而產生的安全漏洞
- **提升品質**：確保程式碼符合業界安全標準
- **保護資產**：防止因安全問題造成的資料外洩或系統入侵
- **合規要求**：滿足企業或組織的資安合規需求

## 貢獻指南

歡迎提交安全相關的改進建議、新的檢測規則或最佳實務案例。請確保所有貢獻內容都經過充分的安全性驗證。

## 授權與著作權

本專案內容由 Cyesuta 與 Google Gemini Pro AI 共同創作完成。

本專案採用開源授權，旨在推廣安全開發實務，保護整個開發者社群的利益。歡迎自由使用、修改和分發，但請保留著作權聲明。

## 多語言支援

本專案提供以下語言版本：
- 英文 (English)
- 繁體中文 (Traditional Chinese)
- 簡體中文 (Simplified Chinese) 
- 日文 (Japanese)
- 德文 (German)
- 法文 (French)
- 俄文 (Russian)
- 意大利文 (Italian)
- 韓文 (Korean)
- 西班牙文 (Spanish)
- 葡萄牙文 (Portuguese)
- 阿拉伯文 (Arabic)
- 荷蘭文 (Dutch)
- 泰文 (Thai)
- 越南文 (Vietnamese)