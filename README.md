# Code Guardian Aegis (VibeCoding新手盾)

## Multi-language Support

This project provides versions in the following languages:
- **[English](#english-version)** (Current)
- **[繁體中文](#繁體中文版本)** (Traditional Chinese)
- **[簡體中文](#简体中文版本)** (Simplified Chinese)
- **[日本語](#日本語版)** (Japanese)
- **[Deutsch](#deutsch-version)** (German)
- **[Français](#version-française)** (French)
- Russian (Русский) - *Coming Soon*
- Italian (Italiano) - *Coming Soon*
- Korean (한국어) - *Coming Soon*
- Spanish (Español) - *Coming Soon*
- Portuguese (Português) - *Coming Soon*
- Arabic (العربية) - *Coming Soon*
- Dutch (Nederlands) - *Coming Soon*
- Thai (ไทย) - *Coming Soon*
- Vietnamese (Tiếng Việt) - *Coming Soon*

---

## English Version

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

### Security Audit Tools (security-audit-meta-prompt/)
- **[Security-Audit-Prompt_English.md](./security-audit-meta-prompt/Security-Audit-Prompt_English.md)** - Complete security audit instruction template for advanced AI models to conduct comprehensive project security checks

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

---

## 繁體中文版本

[🔗 切換到英文版本](#english-version)

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
- **[安全守則_繁體中文.md](./security-guidelines-documents/安全守則_繁體中文.md)** - 核心安全開發檢查清單，供開發者和 AI 助手在開發過程中遵循
- **[安全守則說明_繁體中文.md](./security-guidelines-documents/安全守則說明_繁體中文.md)** - 詳細解釋每條安全規則的重要性，包含駭客攻擊劇本和災難後果分析

### 安全審計工具 (security-audit-meta-prompt/)
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

---

## 简体中文版本

[🔗 切换到英文版本](#english-version) | [🔗 切换到繁體中文版本](#繁體中文版本)

## 项目描述

本项目旨在防止非工程师使用 VibeCoding 时可能引发的严重资安问题。随着 AI 程序代码生成工具的普及，非专业开发者在缺乏足够资安意识的情况下进行开发，可能无意中引入安全漏洞，导致系统暴露于潜在威胁之下。特别针对新手常见问题优化安全审计，提供针对性的防护机制。

## 项目目标

提供完整的安全防护机制，确保使用 VibeCoding 开发的项目符合资安最佳实务：

1. **开发前预防**：提供安全守则与指导文件，建立开发时的安全基准
2. **上线前检核**：提供自动化安全审计工具，全面检测项目潜在风险

## 项目结构

```
├── security-guidelines-documents/  # 安全守则文件
│   ├── Security-Guidelines_English.md         # Security Development Guidelines (for AI and developers)
│   ├── Security-Guidelines-Explanation_English.md  # Detailed explanation of security rules
│   ├── Security-Guidelines_Simplified-Chinese.md         # 安全开发守则 (供 AI 和开发者使用)
│   ├── Security-Guidelines-Explanation_Simplified-Chinese.md  # 安全守则详细说明与原理解释
│   ├── 安全守则_繁體中文.md         # 安全開發守則 (供 AI 和開發者使用)
│   └── 安全守则说明_繁體中文.md     # 安全守則詳細說明與原理解釋
├── security-audit-meta-prompt/     # 安全审计元提示
│   ├── Security-Audit-Prompt_English.md      # Project security audit instruction template
│   ├── Security-Audit-Prompt_Simplified-Chinese.md      # 项目安全审计指令模板
│   └── 安全审计提示_繁體中文.md     # 專案安全審計指令模板
├── README.md                       # 项目说明文件
└── CHANGELOG.md                    # 更新日志
```

## 文件说明

### 安全守则文件 (security-guidelines-documents/)
- **[Security-Guidelines_Simplified-Chinese.md](./security-guidelines-documents/Security-Guidelines_Simplified-Chinese.md)** - 核心安全开发检查清单，供开发者和 AI 助手在开发过程中遵循
- **[Security-Guidelines-Explanation_Simplified-Chinese.md](./security-guidelines-documents/Security-Guidelines-Explanation_Simplified-Chinese.md)** - 详细解释每条安全规则的重要性，包含黑客攻击剧本和灾难后果分析

### 安全审计工具 (security-audit-meta-prompt/)
- **[Security-Audit-Prompt_Simplified-Chinese.md](./security-audit-meta-prompt/Security-Audit-Prompt_Simplified-Chinese.md)** - 完整的安全审计指令模板，用于让高级 AI 模型进行全面的项目安全检查

## 使用方法

### 开发前阶段
1. 将 `security-guidelines-documents/` 中的安全守则文件放入您的 VibeCoding 项目根目录
2. 详细阅读安全守则说明，确保开发过程遵循安全规范
3. **让 VibeCoding Agent 也读取安全守则**：确保 AI 助手在生成程序代码时也能遵循安全规范

### 上线前阶段
1. 使用 `security-audit-meta-prompt/` 中的审计工具
2. 让高级程序设计模型读取并审计整个项目的安全性
3. 根据审计报告修复发现的安全问题

## 重要性

- **降低风险**：减少因缺乏资安知识而产生的安全漏洞
- **提升品质**：确保程序代码符合业界安全标准
- **保护资产**：防止因安全问题造成的资料外泄或系统入侵
- **合规要求**：满足企业或组织的资安合规需求

## 贡献指南

欢迎提交安全相关的改进建议、新的检测规则或最佳实务案例。请确保所有贡献内容都经过充分的安全性验证。

## 授权与著作权

本项目内容由 Cyesuta 与 Google Gemini Pro AI 共同创作完成。

本项目采用开源授权，旨在推广安全开发实务，保护整个开发者社群的利益。欢迎自由使用、修改和分发，但请保留著作权声明。

---

## 日本語版

[🔗 Switch to English](#english-version) | [🔗 切換到繁體中文](#繁體中文版本) | [🔗 切换到简体中文](#简体中文版本)

## プロジェクト概要

このプロジェクトは、非エンジニアがVibeCodingを使用する際に発生する可能性のある深刻なセキュリティ問題を防ぐことを目的としています。AIコード生成ツールの普及に伴い、専門的でない開発者が十分なセキュリティ意識なしに開発を行うと、無意識のうちにセキュリティ脆弱性を導入し、システムを潜在的な脅威にさらす可能性があります。特に初心者によくある問題に最適化されたセキュリティ監査を提供し、的を絞った保護メカニズムを提供します。

## プロジェクト目標

VibeCoding開発プロジェクトがセキュリティベストプラクティスに準拠することを保証する包括的なセキュリティ保護メカニズムを提供します：

1. **開発前予防**：セキュリティガイドラインと文書を提供し、開発時のセキュリティベースラインを確立
2. **デプロイ前検証**：自動化されたセキュリティ監査ツールを提供し、プロジェクトの潜在的リスクを包括的に検出

## プロジェクト構造

```
├── security-guidelines-documents/  # セキュリティガイドライン文書
│   ├── Security-Guidelines_English.md         # Security Development Guidelines (for AI and developers)
│   ├── Security-Guidelines-Explanation_English.md  # Detailed explanation of security rules
│   ├── Security-Guidelines_Simplified-Chinese.md         # 安全开发守则 (供 AI 和开发者使用)
│   ├── Security-Guidelines-Explanation_Simplified-Chinese.md  # 安全守则详细说明与原理解释
│   ├── Security-Guidelines_Japanese.md         # セキュリティ開発ガイドライン（AIと開発者向け）
│   ├── Security-Guidelines-Explanation_Japanese.md  # セキュリティガイドライン詳細説明と原理解説
│   ├── 安全守则_繁體中文.md         # 安全開發守則 (供 AI 和開發者使用)
│   └── 安全守则说明_繁體中文.md     # 安全守則詳細說明與原理解釋
├── security-audit-meta-prompt/     # セキュリティ監査メタプロンプト
│   ├── Security-Audit-Prompt_English.md      # Project security audit instruction template
│   ├── Security-Audit-Prompt_Simplified-Chinese.md      # 项目安全审计指令模板
│   ├── Security-Audit-Prompt_Japanese.md      # プロジェクトセキュリティ監査指令テンプレート
│   └── 安全审计提示_繁體中文.md     # 專案安全審計指令模板
├── README.md                       # プロジェクトドキュメント
└── CHANGELOG.md                    # 変更ログ
```

## ファイル説明

### セキュリティガイドライン文書 (security-guidelines-documents/)
- **[Security-Guidelines_Japanese.md](./security-guidelines-documents/Security-Guidelines_Japanese.md)** - 開発者とAIアシスタントが開発プロセスで従うべき核心セキュリティ開発チェックリスト
- **[Security-Guidelines-Explanation_Japanese.md](./security-guidelines-documents/Security-Guidelines-Explanation_Japanese.md)** - 各セキュリティルールの重要性の詳細説明、ハッカー攻撃シナリオと災害結果分析を含む

### セキュリティ監査ツール (security-audit-meta-prompt/)
- **[Security-Audit-Prompt_Japanese.md](./security-audit-meta-prompt/Security-Audit-Prompt_Japanese.md)** - 高度なAIモデルが包括的なプロジェクトセキュリティチェックを実行するための完全なセキュリティ監査指令テンプレート

## 使用方法

### 開発前段階
1. `security-guidelines-documents/`のセキュリティガイドラインファイルをVibeCodingプロジェクトのルートディレクトリに配置
2. セキュリティガイドラインを詳しく読み、開発プロセスがセキュリティ基準に従うことを確認
3. **VibeCoding Agentにもセキュリティガイドラインを読ませる**：AIアシスタントがコード生成時にセキュリティ基準に従うことを確保

### デプロイ前段階
1. `security-audit-meta-prompt/`の監査ツールを使用
2. 高度なプログラミングモデルにプロジェクト全体のセキュリティを読み取り、監査させる
3. 監査レポートで発見されたセキュリティ問題を修正

## 重要性

- **リスク軽減**：セキュリティ知識不足による脆弱性を最小化
- **品質向上**：コードが業界セキュリティ基準を満たすことを保証
- **資産保護**：セキュリティ問題によるデータ漏洩やシステム侵入を防止
- **コンプライアンス要件**：企業や組織のセキュリティコンプライアンス要件を満たす

## 貢献ガイド

セキュリティ関連の改善提案、新しい検出ルール、ベストプラクティス事例の提出を歓迎します。すべての貢献内容が十分にセキュリティ検証されていることを確認してください。

## ライセンスと著作権

このプロジェクトの内容は、CyesutaとGoogle Gemini Pro AIによって共同制作されました。

このプロジェクトはオープンソースライセンスを採用し、安全な開発実務の推進と開発者コミュニティ全体の利益保護を目的としています。自由に使用、修正、配布できますが、著作権表示を保持してください。

---

## Deutsch Version

[🔗 Switch to English](#english-version) | [🔗 切換到繁體中文](#繁體中文版本) | [🔗 切换到简体中文](#简体中文版本) | [🔗 日本語版に切り替え](#日本語版)

## Projektbeschreibung

Dieses Projekt zielt darauf ab, schwerwiegende Sicherheitsprobleme zu verhindern, die entstehen können, wenn Nicht-Ingenieure VibeCoding verwenden. Mit der weit verbreiteten Einführung von KI-Code-Generierungstools können nicht-professionelle Entwickler während der Entwicklung versehentlich Sicherheitslücken einführen, da sie nicht über ausreichendes Sicherheitsbewusstsein verfügen, wodurch Systeme potenziellen Bedrohungen ausgesetzt werden. Speziell optimierte Sicherheitsprüfung für häufige Anfängerprobleme, die gezielte Schutzmechanismen bietet.

## Projektziele

Bereitstellung umfassender Sicherheitsschutzmechanismen, um sicherzustellen, dass VibeCoding-Entwicklungsprojekte den Sicherheits-Best-Practices entsprechen:

1. **Vorbeugung vor der Entwicklung**: Bereitstellung von Sicherheitsrichtlinien und Dokumentation zur Etablierung von Sicherheitsbaselines während der Entwicklung
2. **Verifikation vor der Bereitstellung**: Bereitstellung automatisierter Sicherheitsprüfungstools für umfassende Erkennung potenzieller Projektrisiken

## Projektstruktur

```
├── security-guidelines-documents/  # Sicherheitsrichtlinien-Dokumente
│   ├── Security-Guidelines_English.md         # Security Development Guidelines (for AI and developers)
│   ├── Security-Guidelines-Explanation_English.md  # Detailed explanation of security rules
│   ├── Security-Guidelines_Simplified-Chinese.md         # 安全开发守则 (供 AI 和开发者使用)
│   ├── Security-Guidelines-Explanation_Simplified-Chinese.md  # 安全守则详细说明与原理解释
│   ├── Security-Guidelines_Japanese.md         # セキュリティ開発ガイドライン（AIと開発者向け）
│   ├── Security-Guidelines-Explanation_Japanese.md  # セキュリティガイドライン詳細説明と原理解説
│   ├── Security-Guidelines_German.md         # Sicherheitsentwicklungsrichtlinien (für KI und Entwickler)
│   ├── Security-Guidelines-Explanation_German.md  # Detaillierte Erklärung der Sicherheitsrichtlinien
│   ├── 安全守则_繁體中文.md         # 安全開發守則 (供 AI 和開發者使用)
│   └── 安全守则说明_繁體中文.md     # 安全守則詳細說明與原理解釋
├── security-audit-meta-prompt/     # Sicherheitsprüfungs-Meta-Prompt
│   ├── Security-Audit-Prompt_English.md      # Project security audit instruction template
│   ├── Security-Audit-Prompt_Simplified-Chinese.md      # 项目安全审计指令模板
│   ├── Security-Audit-Prompt_Japanese.md      # プロジェクトセキュリティ監査指令テンプレート
│   ├── Security-Audit-Prompt_German.md      # Projekt-Sicherheitsprüfungs-Anweisungsvorlage
│   └── 安全审计提示_繁體中文.md     # 專案安全審計指令模板
├── README.md                       # Projektdokumentation
└── CHANGELOG.md                    # Änderungsprotokoll
```

## Dateibeschreibungen

### Sicherheitsrichtlinien-Dokumente (security-guidelines-documents/)
- **[Security-Guidelines_German.md](./security-guidelines-documents/Security-Guidelines_German.md)** - Kern-Sicherheitsentwicklungs-Checkliste für Entwickler und KI-Assistenten zur Befolgung während des Entwicklungsprozesses
- **[Security-Guidelines-Explanation_German.md](./security-guidelines-documents/Security-Guidelines-Explanation_German.md)** - Detaillierte Erklärung der Wichtigkeit jeder Sicherheitsregel, einschließlich Hacker-Angriffsszenarien und Katastrophenfolgen-Analyse

### Sicherheitsprüfungstools (security-audit-meta-prompt/)
- **[Security-Audit-Prompt_German.md](./security-audit-meta-prompt/Security-Audit-Prompt_German.md)** - Vollständige Sicherheitsprüfungs-Anweisungsvorlage für fortgeschrittene KI-Modelle zur Durchführung umfassender Projekt-Sicherheitsprüfungen

## Verwendung

### Vor-Entwicklungsphase
1. Platzieren Sie Sicherheitsrichtlinien-Dateien aus `security-guidelines-documents/` in Ihr VibeCoding-Projekt-Stammverzeichnis
2. Lesen Sie die Sicherheitsrichtlinien gründlich, um sicherzustellen, dass die Entwicklung den Sicherheitsstandards folgt
3. **Lassen Sie auch VibeCoding Agent die Sicherheitsrichtlinien lesen**: Stellen Sie sicher, dass KI-Assistenten bei der Code-Generierung Sicherheitsstandards befolgen

### Vor-Bereitstellungsphase
1. Verwenden Sie Prüfungstools aus `security-audit-meta-prompt/`
2. Lassen Sie fortgeschrittene Programmiermodelle die gesamte Projektsicherheit lesen und prüfen
3. Beheben Sie Sicherheitsprobleme, die im Prüfungsbericht gefunden wurden

## Wichtigkeit

- **Risikominderung**: Minimierung von Sicherheitslücken aufgrund mangelnden Sicherheitswissens
- **Qualitätsverbesserung**: Sicherstellung, dass Code Branchensicherheitsstandards erfüllt
- **Vermögensschutz**: Verhinderung von Datenlecks oder Systemeinbrüchen durch Sicherheitsprobleme
- **Compliance-Anforderungen**: Erfüllung von Unternehmen- oder Organisationssicherheits-Compliance-Anforderungen

## Beitragsleitfaden

Willkommen bei sicherheitsbezogenen Verbesserungsvorschlägen, neuen Erkennungsregeln oder Best-Practice-Fällen. Bitte stellen Sie sicher, dass alle Beiträge gründlich sicherheitsverifiziert sind.

## Lizenz und Urheberrecht

Dieser Projektinhalt wurde gemeinsam von Cyesuta und Google Gemini Pro KI erstellt.

Dieses Projekt ist unter Open-Source-Lizenz lizenziert und zielt darauf ab, sichere Entwicklungspraktiken zu fördern und die Interessen der gesamten Entwicklergemeinschaft zu schützen. Frei zu verwenden, zu modifizieren und zu verteilen, aber bitte behalten Sie Urheberrechtshinweise bei.

---

## Version Française

[🔗 Switch to English](#english-version) | [🔗 切換到繁體中文](#繁體中文版本) | [🔗 切换到简体中文](#简体中文版本) | [🔗 日本語版に切り替え](#日本語版) | [🔗 Zur deutschen Version](#deutsch-version)

## Description du Projet

Ce projet vise à prévenir les problèmes de sécurité graves qui peuvent survenir lorsque des non-ingénieurs utilisent VibeCoding. Avec l'adoption généralisée des outils de génération de code IA, les développeurs non professionnels peuvent involontairement introduire des vulnérabilités de sécurité pendant le développement en raison d'une sensibilisation insuffisante à la sécurité, exposant les systèmes à des menaces potentielles. Audit de sécurité spécialement optimisé pour les problèmes courants des débutants, fournissant des mécanismes de protection ciblés.

## Objectifs du Projet

Fournir des mécanismes de protection de sécurité complets pour s'assurer que les projets de développement VibeCoding respectent les meilleures pratiques de sécurité :

1. **Prévention pré-développement** : Fournir des directives de sécurité et de la documentation pour établir des références de sécurité pendant le développement
2. **Vérification pré-déploiement** : Fournir des outils d'audit de sécurité automatisés pour une détection complète des risques potentiels du projet

## Utilisation

### Phase Pré-développement
1. Placez les fichiers des directives de sécurité de `security-guidelines-documents/` dans le répertoire racine de votre projet VibeCoding
2. Lisez attentivement les directives de sécurité pour vous assurer que le développement suit les normes de sécurité  
3. **Laissez aussi VibeCoding Agent lire les directives de sécurité** : Assurez-vous que les assistants IA suivent les normes de sécurité lors de la génération de code

### Phase Pré-déploiement
1. Utilisez les outils d'audit de `security-audit-meta-prompt/`
2. Faites lire et auditer la sécurité de l'ensemble du projet par des modèles de programmation avancés
3. Corrigez les problèmes de sécurité trouvés dans le rapport d'audit

## Licence et Droits d'Auteur

Le contenu de ce projet a été co-créé par Cyesuta et Google Gemini Pro IA.

Ce projet est sous licence open source, visant à promouvoir les pratiques de développement sécurisé et à protéger les intérêts de toute la communauté des développeurs. Libre d'utiliser, modifier et distribuer, mais veuillez conserver les mentions de droits d'auteur.