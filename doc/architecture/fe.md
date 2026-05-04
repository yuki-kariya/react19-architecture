# Architecture

## Logical Groups

### Generic Components

Components dedicated to implementing specific UI elements without carrying any feature-specific context.

Examples: `Input`, `Select`, `Popup`

### Feature Components

A feature is a unit that groups the UI, state management, and logic required for a business objective a user wants to accomplish.

It should be defined by the user's goal, such as "employee search" or "reservation registration," rather than by a single action such as "button click."

For UI implementation, feature components depend on generic components.

They may also contain some data model definitions related to the application's domain area.

Examples: `AddressSearchInput`, `ProfileForm`

### Page Components

Components dedicated to implementing page-level UI and feature composition.

These are resources that are associated with URLs and can be routed to.

For UI implementation, page components depend on both generic components and feature components.

Example: `EditProfilePage`

### Layout Components

Components dedicated to implementing UI structures shared across multiple pages.

Example: `WithSideNavLayout`

### External Ports

Interfaces between internal application logic and external services.

To prevent external service data structures from leaking into application logic, data should be transformed by adapters described below.

Examples: `ApiPort`, `StorageServicePort`

Define ports only for dependencies that are likely to change in the future.

React and some related packages are treated like browser built-in APIs, meaning they are not considered switchable dependencies.

### Adapters

Components dedicated to implementing the external ports described above.

They call external services directly and convert the data into the structures expected by the application logic.

Examples: `ApiImpl`, `StorageServiceImpl`

### Configuration

Components dedicated to managing application configuration values.

Typical usage includes exporting environment variables with type safety.

### Routing

Components dedicated to routing configuration.

They should follow the APIs and conventions of `react-router`.

### Locales

Components dedicated to internationalization.

They should follow the APIs and conventions of `react-i18next`.

### Lib

Shared initialization and common configuration for external libraries.

### Util

Context-independent utility functions such as date calculations and formatting.

## Dependency Flow

Dependencies must flow from higher-level groups to lower-level groups. Lower-level groups must not depend on higher-level groups.

```plaintext
┌──────────────────────────────────────────────┐
│ Routing (Application)                       │ ← can depend on Pages / Layouts
├──────────────────────────────────────────────┤
│ Layouts (Application)                       │ ← can depend on Pages / Features / Components / Config / Lib / Utils / Locales
├──────────────────────────────────────────────┤
│ Pages (Application)                         │ ← can depend on Features / Components / Config / Lib / Utils / Locales
├──────────────────────────────────────────────┤
│ Features (Application)                      │ ← can depend on Components / Ports / Config / Lib / Utils / Locales
├──────────────────────────────────────────────┤
│ Components (UI)                             │ ← can depend on Config / Lib / Utils / Locales
├──────────────────────────────────────────────┤
│ Adapters (Infrastructure)                   │ ← can depend on Ports / Config / Lib
├──────────────────────────────────────────────┤
│ Ports (Interface)                           │ ← depends on nothing
├──────────────────────────────────────────────┤
│ Config / Lib / Utils / Locales (Shared)     │ ← no application-specific dependencies
└──────────────────────────────────────────────┘
```

## Directory Structure

```plaintext
src/
├── pages/                         # Page components
│   ├── AccountProfilePage/
│   │   ├── AccountProfilePage.tsx
│   │   ├── index.ts
│   │   ├── types.ts
│   │   ├── components/            # Page-specific components
│   │   └── hooks/
│   └── ...
│
├── layouts/                       # Layout components
│   ├── PrivateRouteLayout/
│   │   ├── PrivateRouteLayout.tsx
│   │   ├── index.ts
│   │   ├── types.ts
│   │   └── hooks/
│   └── ...
│
├── features/                      # Feature components
│   ├── account/
│   │   ├── AccountProfileTable/
│   │   │   ├── AccountProfileTable.tsx
│   │   │   ├── index.ts
│   │   │   ├── types.ts
│   │   │   └── hooks/
│   │   ├── model/
│   │   └── index.ts
│   └── ...
│
├── components/                    # Generic components
│   ├── Button/
│   │   ├── Button.tsx
│   │   └── index.ts
│   ├── Input/
│   │   ├── Input.tsx
│   │   └── index.ts
│   └── ...
│
├── ports/                         # External service interfaces
│   ├── ApiPort.ts
│   ├── StorageServicePort.ts
│   └── ...
│
├── adapters/                      # Implementations of ports
│   ├── api/
│   │   ├── ApiImpl.ts
│   │   └── index.ts
│   ├── storage/
│   │   ├── StorageServiceImpl.ts
│   │   └── index.ts
│   └── ...
│
├── config/                        # Configuration
│   └── env.ts
│
├── lib/                           # Library initialization and shared settings
│   ├── axios/
│   │   ├── HrApiClient.ts
│   │   └── index.ts
│   └── ReactQueryClient.ts
│
├── routing/                       # Routing configuration
│   ├── routes.tsx
│   └── index.ts
│
├── locales/                       # Internationalization resources
│   ├── ja/
│   │   └── common.json
│   ├── en/
│   │   └── common.json
│   └── index.ts
│
├── utils/                         # Context-independent utility functions
│   ├── formatDate.ts
│   └── ...
│
└── tests/
    └── ...
```
