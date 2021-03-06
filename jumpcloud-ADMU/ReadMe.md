# jumpcloud-ADMU

Jumpcloud Active Directory Migration Utility.

## Development

This project is being developed in Powershell using XAML front-end.

### Scripts

The directory `Deploy` contains a set of scripts used in the pipeline for CI and CD.

- **admu.ico:** Icon file used in .exe generation
- **ADMU.ps1:** Combined single file output used in .exe generation
- **Build.ps1:** Builds .exe utilizing ps2exe module
- **Build-HelpFiles.ps1:** Uses PlatyPS to generate function help file
- **BuildNuspecFromPsd1.ps1:** Builds Nuspec from Powershell Module to publish
- **Get-Config.ps1:** Used to set project configuration vars from CI pipeline
- **Invoke-GitCommit.ps1:** Git commit script used in pipeline
- **Sign.ps1:** Signs .exe with codesigning certificate
- **TestSetup.ps1:** Ran in pipeline to clear & install latest JC agent using ADMU functions

- **Get-PSGalleryModuleVersion.ps1:** Used to increment and find currently published module version
- **New-ModuleChangeLog.ps1:** Used to generate and add to module changelog md file

The directory `Docs` contains the generated and populated help files for the Powershell functions in the Module.

- **`Start-Migration.md`:** Help file for start-migration function

The directory `jumpcloud-ADMU\Exe\` contains the signed .exe output from the pipeline build used in the release steps.

- **gui_jcadmu.exe:** Signed executable file

The directory `jumpcloud-ADMU\Gpo\` contains the GPO's used in mass deployment/invoke-command scenario scripts.

The directory `jumpcloud-ADMU\Powershell\` contains the Powershell scripts used by the ADMU.

- **Form.ps1:** XAML & form logic
- **Functions.ps1:** Utilized functions in gui & Start-Migration Function
- **InvokePester.ps1:** Calls invoke-pester with formated output for pipeline
- **Start-JCADMU.ps1:** Calls Form.ps1 & passes output to Start-Migration Function as object

The directory `jumpcloud-ADMU\Powershell\Tests\` contains the signed .exe output from the pipeline build used in the release steps.

- **gui_jcadmu.exe:** Signed executable file

The directory `jumpcloud-ADMU-Advanced-Deployment` contains powershell scripts to run discovery and migration in a mass deployment scenario.

- **invoke-admu-discovery.ps1:** Used to collect and output ADMU_DISCOVERY.csv which contains domain accounts and information from each system
- **invoke-admu-migration.ps1:** Used to install and invoke the ADMU start-migration cmd and pass params from the ADMU_DISCOVERY.csv

### CI\CD Pipeline

https://dev.azure.com/JumpCloudPowershell/JumpCloud%20ADMU/_build?definitionId=24&_a=summary

The pipeline runs the following steps on CI builds:

- **Powershell Build Script:** Builds exe from powershell scripts
- **Powershell Sign exe:** Signs exe build with code signing certificate
- **Test Setup Script:** Setup build server with domain joined agent system
- **InvokePester Script:** Runs pester tests & verifys execuatable signature
- **Copy Files to:powershell:** Copy powershell files to artifact directory for use on release
- **Copy Files to:exe:** Copy exe to artifact directory for use on release
- **Publish Artifact: ADMU:** Publish artifact directory to pipeline artifact
- **Invoke-GitCommit - BranchName:** Commit execuatble build back to branch if previous steps pass without issue

The pipeline runs the following steps on CD releases:

- **_JumpCloudADMU-CI artifact:** powershell & exe files from successful build branch
- **GH Release (create):** GitHub release draft is created containing artifact assets
    - Update Tag & Release notes

### Testing

TestSetup.ps1

- Clears Temp & JCADMU Folders
- Uninstalls JC, VS C++ prereqs
- Reinstalls JCagent

Build.Tests.ps1

- Checks XAML form build number
- Checks built exe build number

Functions.Tests.ps1
gpo.Tests.ps1

PSScriptAnalyzer.Tests.ps1

- Runs psscriptanalyzer against powershell directory with custom exclude rules.