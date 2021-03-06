# OMSSearch
OMSSearch is a PowerShell module for Azure Automation that will help you execute queries against Microsoft Operations Management Suite.
The module uses ADAL to get Token from Azure AD.

# Prerequisites
1. Your OMS workspace is linked to your Azure Subscription
2. Find your Subscription ID
3. Find what is the name of the resource group where your OMS workspaces is located. ARM explorer (https://resources.azure.com/) can help you.
3. Know the name of your OMS workspace in Azure
# Instructions
1. Archive all files in a OMSSearch.zip file
2. Add module to Azure Automation
4. Create Connection in Azure Automation of type OMSConnection where TenantADName is the UPN with which your Azure AD accounts are created 
(example stasoutlook.onmicrosoft.com), Username is a UPN account in your Azure AD that has access to OMS and Password is the password for that account.
3. Enjoy

# Examples
```PowerShell
workflow Get-SavedSearches
{	
	$OMSCon = Get-AutomationConnection -Name 'stasoutlook'
	$Token = Get-AADToken -OMSConnection $OMSCon
	$subscriptionId = "3c1d68a5-4064-4522-94e4-e03781655555e"
	$ResourceGroupName = "oi-default-east-us"
	$OMSWorkspace = "test"	
	
	Get-OMSSavedSearches `
		-OMSWorkspaceName $OMSWorkspace  `
		-ResourceGroupName $ResourceGroupName `
		-SubscriptionID $subscriptionId `
		-Token $Token
}
```
```PowerShell
workflow Get-RestartedComputers
{	
	$OMSCon = Get-AutomationConnection -Name 'stasoutlook'
	$Token = Get-AADToken -OMSConnection $OMSCon
	$subscriptionId = "3c1d68a5-4064-4522-94e4-e03781655555e"
	$ResourceGroupName = "oi-default-east-us"
	$OMSWorkspace = "test"	
	$Query = "shutdown Type=Event EventLog=System Source=User32 EventID=1074 | Select TimeGenerated,Computer"
	
	Execute-OMSSearchQuery -SubscriptionID $subscriptionId `
	                       -ResourceGroupName $ResourceGroupName  	`
						   -OMSWorkspaceName $OMSWorkspace `
						   -Query $Query `
						   -Token $Token
}
```
# Blogpost
https://cloudadministrator.wordpress.com/2015/06/05/programmatically-search-operations-management-suite/