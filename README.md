Licenses Management scripts

#Lets connect to the Azure Active Directory
Connect-AzureAD

 

#What licenses are available?
Get-AzureADSubscribedSku

 

#More info about the license package
Get-AzureADSubscribedSku | Select-Object -Property ObjectId, SkuPartNumber, ConsumedUnits -ExpandProperty PrepaidUnits

 

#What is included in the license package
Get-AzureADSubscribedSku `
-ObjectId 95b14fab-6bbf-4756-94d4-99993dd27f55_05e9a617-0261-4cee-bb44-138d3ef5d965 | Select-Object -ExpandProperty ServicePlans

 

#To list all licensed users
Get-AzureAdUser | ForEach { $licensed=$False ; For ($i=0; $i -le ($_.AssignedLicenses | Measure).Count ; $i++)`
{ If( [string]::IsNullOrEmpty( $_.AssignedLicenses[$i].SkuId ) -ne $True) { $licensed=$true } } ; If( $licensed -eq $true)`
{ Write-Host $_.UserPrincipalName} }

 

#To list all of the unlicensed users
Get-AzureAdUser | ForEach{ $licensed=$False ; For ($i=0; $i -le ($_.AssignedLicenses | Measure).Count ; $i++)`
{ If( [string]::IsNullOrEmpty( $_.AssignedLicenses[$i].SkuId ) -ne $True) { $licensed=$true } } ; If( $licensed -eq $false)`
{ Write-Host $_.UserPrincipalName} }
