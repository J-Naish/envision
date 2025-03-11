# Infrastructure Management

Terraformを使って以下のような構成にする(予定)

## Directory Structure

```
infrastructure/
├── environments/
│   ├── development/
│   │   ├── main.tf
│   │   ├── variables.tf
│   │   └── outputs.tf
│   ├── staging/
│   └── production/
├── modules/
│   ├── kubernetes/
│   ├── networking/
│   └── databases/
└── backend/
    └── main.tf
```
