pipeline {
agent any
 environment {
        dotnet ='C:\\Program Files (x86)\\dotnet\\'
        }
stage('Restore packages'){
   steps{
      bat "dotnet restore .\CoreWebAPIV1\\CoreWebAPIV1.csproj"
     }
  }
stage('Clean'){
    steps{
        bat "dotnet clean .\CoreWebAPIV1\\CoreWebAPIV1.csproj"
     }
   }
stage('Increase version') {
    steps {
        echo "${env.BUILD_NUMBER}"
        powershell '''
           $xmlFileName = ".\\CoreWebAPIV1\\Package.appxmanifest"     
           [xml]$xmlDoc = Get-Content $xmlFileName
           $version = $xmlDoc.Package.Identity.Version
           $trimmedVersion = $version -replace '.[0-9]+$', '.'
           $xmlDoc.Package.Identity.Version = $trimmedVersion + ${env:BUILD_NUMBER}
           echo 'New version:' $xmlDoc.Package.Identity.Version
           $xmlDoc.Save($xmlFileName)
        '''
     }
 }
stage('Build'){
   steps{
      bat "dotnet build .\CoreWebAPIV1\\CoreWebAPIV1.csproj --configuration Release"
    }
 }
 stage('Publish'){
     steps{
       bat "dotnet publish .\CoreWebAPIV1\\CoreWebAPIV1.csproj "
     }
}
}
}