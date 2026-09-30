# Active Directory IAM Implementation

## Stage 1 — Enterprise Workforce Identity Provisioning

### Stage 1A — Target OU Validation

Before provisioning workforce identities, the target Active Directory Organizational Units were validated.

Validated target OUs:

- Reception
- Clinical
- IT
- Security

PowerShell variables were used to reference the Distinguished Names of the target OUs.

```powershell
$BaseDN = "DC=corp,DC=medicarehealth,DC=test"
$MediCareDN = "OU=MediCare,$BaseDN"

$ReceptionOU = "OU=Reception,OU=Users,$MediCareDN"
$ClinicalOU = "OU=Clinical,OU=Users,$MediCareDN"
$ITOU = "OU=IT,OU=Users,$MediCareDN"
$SecurityOU = "OU=Security,OU=Users,$MediCareDN"