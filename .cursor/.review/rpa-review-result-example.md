# RPA Review Result

## Metadata
Review date: 2026-05-17
Project name: RPA000_Sample_ReviewSample
Review type: Full
Reviewer: Cursor RPA Review Agent
Ruleset version: Workspace RPA review rules 2026-05-17
Review confidence: High

## Executive Summary
Overall risk: High
Production readiness: Needs fixes
Main concern: Proje genel olarak Rocky Framework standartlarına uygun görünse de, veritabanı bağlantı dizesinin (connection string) kod içine gömülü olması ve SAP giriş adımında hata yakalama (exception handling) mekanizmasının eksikliği canlı ortamda ciddi güvenlik ve duruş riskleri taşımaktadır.

## Findings

### RPA-REV-001
Severity: Critical
Status: Open
Rule: Config and Security Rules / No Static Values
File / Workflow: `HR_IseAlim/Init/GetEmployeeData.xaml`
Evidence: Assign aktivitesinde `str_DbConnectionString` değişkenine doğrudan `"Server=prod-db01;Database=HR_Data;User Id=hr_admin;Password=SuperSecretPassword!;"` değeri atanmış.
Why it matters: Veritabanı şifresi ve sunucu bilgileri kod içinde açıkça (plain text) tutulmaktadır. Bu durum ciddi bir güvenlik ihlalidir ve ortam (Dev/Test/Prod) değişikliklerinde kodun yeniden derlenmesini gerektirir.
Suggested fix: Bağlantı dizesini ve şifreyi Orchestrator Asset'lerine (Credential) taşıyın ve çalışma zamanında `Get Credential` aktivitesi ile güvenli bir şekilde çekin.
Fix eligibility: Manual-only
Fix risk: Medium
Confidence: High

### RPA-REV-002
Severity: Warning
Status: Open
Rule: Exception Handling Rules / TryCatch Requirement
File / Workflow: `HR_IseAlim/Init/LoginSAP.xaml`
Evidence: SAP uygulamasına giriş yapan `Type Into` ve `Click` aktiviteleri herhangi bir `TryCatch` bloğu ile sarmalanmamış.
Why it matters: SAP uygulamasının geç açılması, şifre süresinin dolması veya ekranın donması gibi durumlarda süreç doğrudan çökecek ve framework'ün standart hata yönetimi (retry vb.) devreye giremeden kontrolsüz bir sistem hatası fırlatacaktır.
Suggested fix: Giriş aktivitelerini bir `TryCatch` bloğu içine alın. Olası bir `SelectorNotFoundException` durumunda hatayı loglayıp anlamlı bir `SystemException` ("SAP giriş ekranı yüklenemedi" vb.) fırlatarak framework'e iletin.
Fix eligibility: Semi-auto-fixable
Fix risk: Low
Confidence: High

### RPA-REV-003
Severity: Suggestion
Status: Open
Rule: Naming Rules / Variable Naming
File / Workflow: `HR_IseAlim/Process/CreateUser.xaml`
Evidence: İsim ve soyisim birleştirilirken `tempStr` adında bir değişken kullanılmış.
Why it matters: `tempStr` değişken adı, tuttuğu verinin içeriği hakkında (Çalışan Adı Soyadı) bilgi vermemektedir. Bu durum kodun okunabilirliğini ve bakımını zorlaştırır.
Suggested fix: `tempStr` değişkeninin adını `str_EmployeeFullName` olarak değiştirin.
Fix eligibility: Auto-fixable
Fix risk: Low
Confidence: High

## Manual Checks
- SAP ekranlarındaki UI Selector'lerin canlı (Production) ortamdaki SAP GUI versiyonu ile uyumlu olup olmadığı manuel olarak test edilmelidir.
- Orchestrator üzerinde `HR_DB_Credential` adlı Asset'in Prod ortamı için doğru yetkilerle tanımlandığı doğrulanmalıdır.
- İşe alım verilerinin çekildiği veritabanı tablosunda, robotun okuduğu satırların statüsünün (Örn: "İşleniyor") başka bir robotla çakışmayı önleyecek şekilde (idempotency) kilitlenip kilitlenmediği kontrol edilmelidir.

## Recommended Next Actions
1. `GetEmployeeData.xaml` içindeki hardcoded veritabanı bağlantı dizesini ve şifresini Orchestrator Asset kullanımına geçirin.
2. `LoginSAP.xaml` içindeki giriş adımlarını `TryCatch` bloğu ile koruma altına alın.
3. `CreateUser.xaml` içindeki isimlendirme standartlarına uymayan değişken adlarını düzeltin.