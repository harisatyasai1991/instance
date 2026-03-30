# DMS Insight - Changelog

All notable changes to DMS Insight will be documented in this file.

The format is based on [Keep a Changelog](https://keepachangelog.com/en/1.0.0/),
and this project adheres to [Semantic Versioning](https://semver.org/spec/v2.0.0.html).

---

## [2.6.1-DMS] - 2026-03-30

### Fixed
- **Critical**: Fixed Master Admin auto-creation using wrong password field name (`password_hash` → `hashed_password`)
- **Critical**: Fixed password hashing algorithm mismatch (SHA-256 → bcrypt) to match existing auth system
- Added better error logging with stack traces for initialization failures

---

## [2.6.0-DMS] - 2026-03-30

### Added
- **Database Auto-Initialization**: Fresh deployments now work out-of-the-box
  - Master Admin user auto-created on first startup (`DMSInsight` / `Insight@CBM`)
  - 7 Default Daily Log templates for all asset types (Transformer, Switchgear, Motors, Generators, UPS, Cables, Generic)
  - Default Feature Flags initialized
  - Environment variable overrides: `MASTER_ADMIN_USERNAME`, `MASTER_ADMIN_PASSWORD`
  
- **Daily Operational Logs**: Complete daily inspection logging system
  - "Daily Readings" tab on Asset Detail page
  - OCR-enabled camera capture for gauge readings
  - Configurable parameters per asset type
  - Auto-creates downtime when status changes to De-energized
  - Shift-based logging (Morning/Afternoon/Night)

- **Branding Logo Persistence**: Base64 fallback for logo images

### Deployment Notes
⚠️ **IMPORTANT**: Before every deployment, bump the version in ALL THREE files:
1. `/app/frontend/package.json` → `"version": "X.X.X"`
2. `/app/VERSION.json` → `"version": "X.X.X"` + update `release_date` and `features_in_release`
3. `/app/frontend/src/index.js` → `APP_VERSION = 'X.X.X'`

Skipping version bump may cause the deployment pipeline to skip pushing new frontend code.

### Fresh Installation
On first startup with empty database:
1. Backend automatically creates Master Admin
2. Default Daily Log templates are seeded
3. Feature flags are initialized
4. Login with `DMSInsight` / `Insight@CBM` to begin setup

To customize Master Admin credentials, set environment variables before first startup:
```
MASTER_ADMIN_USERNAME=CustomAdmin
MASTER_ADMIN_PASSWORD=SecurePassword123
```

---

## [2.5.0-DMS] - 2026-03-09

### Added
- **Feature Flags System**: Master Admin can enable/disable specific features per company
  - 18 feature flags across 4 modules
  - 3 workflow variants with configurable options
  - Frontend `hasFeature()` hook for conditional rendering
  
- **AI Demo Company Generator**: Create realistic demo companies using natural language
  - GPT-4o powered prompt parsing
  - Generates complete data: company, users, sites, assets, test records
  - 5 preset example prompts
  
- **Company Factory Reset**: Clear all operational data while preserving users
  - Safety confirmation (type company name)
  - Audit logging of reset actions
  
- **Dynamic Region Risk Heatmap**: Compass-style grid layout for regions
  - Supports 1-9 regions in intelligent layout
  - Auto-detects region names for positioning
  
- **Version Tracking System**: Track deployed versions across instances
  - VERSION.json configuration
  - API endpoint for version info
  - Footer version display

### Changed
- Region Risk Heatmap now dynamically adapts to any number of regions
- Company Management page now has 4 action buttons per company

### Fixed
- GridTech seed script duplicate site names issue
- Export/Import cross-tenant ID mapping
- Region names not displaying correctly in exports

---

## [2.4.0-DMS] - 2026-03-05

### Added
- Complete Company Export/Import with all modules
- Cross-module linking between Online Monitoring and Asset Performance
- Test Records bulk export with asset mapping

### Fixed
- Import failing when sites/assets present without company
- Duplicate equipment imports on re-import

---

## [2.3.0-DMS] - 2026-03-01

### Added
- Online Monitoring Dashboard with real-time widgets
- Substation Heatmap Widget
- Mini bar charts in Region Risk Overview
- Data Import/Export for Monitoring Module

### Changed
- Improved dashboard layout and responsiveness

---

## [2.2.0-DMS] - 2026-02-26

### Added
- Audit Testing Feature for Production Module
- Third-party audit workflow support
- Audit test result recording

---

## [2.1.0-DMS] - 2026-02-20

### Added
- Production Testing Module MVP
- Product and Test Specification management
- Test execution workflow
- Operator and Supervisor roles

---

## [2.0.0-DMS] - 2026-02-15

### Added
- Multi-tenant architecture
- Company Management for Master Admin
- Module-based access control
- Online Monitoring Module MVP

### Changed
- Complete UI redesign with Shadcn components
- New navigation structure

---

## [1.0.0-DMS] - 2026-01-15

### Added
- Initial release of DMS Insight
- Asset Performance Module
- Site and Asset Management
- Test Records tracking
- User authentication and authorization
- Basic reporting

---

## Version Naming Convention

```
v{MAJOR}.{MINOR}.{PATCH}-{CUSTOMER_CODE}

Examples:
- v2.5.0-DMS   → DMS Insight SaaS (main instance)
- v2.5.0-TC    → Torrent Cables (on-premise)
- v2.5.0-GE    → GridTech Energy (on-premise)
```

### Customer Codes
| Code | Customer | Deployment |
|------|----------|------------|
| DMS | DMS Insight | SaaS (Cloud) |
| TC | Torrent Cables | On-Premise |
| GE | GridTech Energy | On-Premise |

---

## Upgrade Notes

### Upgrading to 2.5.0
- No database migration required
- Feature flags will use defaults until configured
- Existing workflows continue to work as "standard"

### Upgrading to 2.4.0
- Run database migration for cross_module_links collection
- Re-export any existing data to include new fields
