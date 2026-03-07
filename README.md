# MoveMark

**Protect your deposit. Prove your case.**

MoveMark is an iOS-first renter protection app that helps renters document property condition, store evidence, log maintenance issues, and generate dispute packets for move-in, move-out, damage, fee, and deposit disputes.

## Features

- **Property Vault** — Store lease, deposit receipt, landlord info, and listing screenshots
- **Guided Move-In Capture** — Room-by-room photo capture with issue tags (scratch, dent, crack, mold, etc.)
- **Maintenance Log** — Track issues with dates, landlord responses, and attachments
- **Guided Move-Out Capture** — Re-capture room condition for before/after proof
- **Dispute Builder** — Create dispute packets with evidence selection and PDF export
- **Settings & Paywall** — Account, subscription (Move Pack $29 / Shield Pro $5.99/mo), and preferences

## Project Structure

```
rpa/
├── MoveMarkApp.swift              # App entry point
├── App/
│   ├── AppRouter.swift           # Auth-based routing
│   └── RootTabView.swift        # Tab navigation (Home, Properties, Disputes, Settings)
├── Core/
│   ├── Theme/LeaseShieldTheme.swift
│   ├── Components/               # PrimaryButton, PropertyCard, RoomCard, etc.
│   └── Managers/
│       ├── SessionManager.swift
│       └── EntitlementManager.swift
├── Models/                      # Property, Room, Inspection, MaintenanceIssue, Dispute, etc.
├── ViewModels/
├── Features/
│   ├── Auth/                    # Welcome, SignIn, CreateAccount
│   ├── Onboarding/              # OnboardingIntro, CreateProperty, AddDocuments
│   ├── Dashboard/               # HomeDashboardScreen
│   ├── Properties/              # PropertiesList, PropertyDetail, DocumentVault
│   ├── MoveIn/                  # Room list, capture, review
│   ├── MoveOut/                 # Room list, capture, review
│   ├── Maintenance/             # Issue list, add, detail
│   ├── Disputes/                # List, type selection, questionnaire, evidence, preview
│   ├── Settings/
│   └── Subscription/            # PaywallScreen
└── Data/Repositories/           # PropertyRepository (mock), Supabase-ready
```

## Build & Run

1. Open `rpa.xcodeproj` in Xcode
2. Select an iOS Simulator (e.g. iPhone 17)
3. Build and run (⌘R)

Display name **MoveMark** is set in Info.plist. Camera and photo library usage descriptions are configured for move-in/move-out capture.

## Backend (Supabase)

Supabase migration and schema are in `supabase/migrations/001_initial_schema.sql`. Tables include:

- `profiles` — User profiles
- `properties` — Rental properties
- `property_documents` — Lease, deposit receipt, screenshots
- `rooms`, `inspections`, `inspection_items` — Move-in/move-out data
- `evidence_files` — Photos/videos
- `maintenance_issues` — Maintenance log
- `disputes`, `dispute_evidence_links`, `exports`

RLS policies are included. Storage buckets to create: `leases`, `deposit-receipts`, `inspection-media`, `maintenance-media`, `exports`, `listing-screenshots`.

### Deploy Edge Function (formal dispute packet)

The formal dispute packet export calls the `generate-dispute-packet` Edge Function. Deploy it to your Supabase project so export works in production:

```bash
# From repo root, with Supabase CLI linked to project cxegmojxcstxinhuexjj
supabase functions deploy generate-dispute-packet --project-ref cxegmojxcstxinhuexjj
```

If the function is not deployed, the app shows explicit guidance when export fails instead of a generic error.

## Next Steps

1. Add Supabase Swift SDK and wire auth + properties
2. Implement PDF export for move-in/move-out reports and dispute packets
3. Integrate StoreKit 2 for Move Pack and Shield Pro
4. Add camera capture (UIImagePickerController or PHPicker) for direct photo capture
# movemark
