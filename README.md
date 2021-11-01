# NL_Gemeenteraad_Weert_Docs
<#
Genereert een lijst van de documenten van voorgaande vergaderingen van gemeente(raad) Weert van 2015 t/m 2021

Verander $outputDirectory en eventueel $outputFile

documenten voor komende vergaderingen onderaan

Alleen documenten, geen video en audio, maar daarvoor zou je aan kunnen passen

#>
# documenten vroegere vergaderingen
$outputDirectory = "S:\_ue\"
$outputFile = "GemeenteWeertDocumentenLijst.txt"
$baseurl = "https://gemeenteraad.weert.nl"
$output = -join("$outputDirectory","$outputFile")
$listCies = "Gemeenteraad","Raadscommissie-Samenleving-en-Inwoners","Collegeadviescommissies","Agendacommissie","Fractievoorzittersoverleg","Collegeadviescommissies","Raadscommissie-Ruimte-Economie","Raadscommissie-Middelen-en-Bestuur","Sprekersplein","Informatiebijeenkomst","College-van-B-W","Auditcommissie","Werkgeverscommissie","Informatiebijeenkomst-Bedrijfsvoering-Inwoners","Informatiebijeenkomst-Ruimte","Raadscommissie-Bedrijfsvoering-Inwoners","Raadscommissie-Ruimte"
$listCies |%{
  $Cie = $_
	$listYears = 2015,2016,2017,2018,2019,2020,2021
	$listYears |%{
	  $Jaar = $_
	  $url = -join("$baseurl","/Vergaderingen/","$Cie","/","$Jaar")
    $ProgressPreference = 'SilentlyContinue'    # Subsequent calls do not display UI.
      $page = Invoke-Webrequest $url
    $ProgressPreference = 'Continue'            # Subsequent calls do display UI.
    # get links from actual meetings and attach $baseurl
    $page.links.href |?{($_ -like "*$Jaar*") -and ($_.Split("/").GetUpperBound(0) -gt  4) } |%{-join("$baseurl",$_)} |%{
      $url2 = $_
      $ProgressPreference = 'SilentlyContinue'    # Subsequent calls do not display UI.
        $page2 = Invoke-Webrequest $url2
      $ProgressPreference = 'Continue'            # Subsequent calls do display UI.
      $page2.links.href |?{($_ -like "*.pdf*") -or ($_ -like "*.doc*") -or ($_ -like "*.txt*") -or ($_ -like "*.xls*") -or ($_ -like "*.ppt*") -or ($_ -like "*.rtf*")} |%{-join("$baseurl",$_)} | Out-File -FilePath $output -Append
    }
	}
}


# documenten komende vergaderingen
$outputDirectory = "S:\_ue\"
$outputFile = "GemeenteWeertDocumentenLijstKomend.txt"
$baseurl = "https://gemeenteraad.weert.nl"
$output = -join("$outputDirectory","$outputFile")
$listCies = "Gemeenteraad","Raadscommissie-Samenleving-en-Inwoners","Collegeadviescommissies","Agendacommissie","Fractievoorzittersoverleg","Collegeadviescommissies","Raadscommissie-Ruimte-Economie","Raadscommissie-Middelen-en-Bestuur","Sprekersplein","Informatiebijeenkomst","College-van-B-W","Auditcommissie","Werkgeverscommissie","Informatiebijeenkomst-Bedrijfsvoering-Inwoners","Informatiebijeenkomst-Ruimte","Raadscommissie-Bedrijfsvoering-Inwoners","Raadscommissie-Ruimte"
$listCies |%{
  $Cie = $_
  #$Cie
  $url = -join("$baseurl","/Vergaderingen/","$Cie","/","Komende-vergaderingen")
  $ProgressPreference = 'SilentlyContinue'    # Subsequent calls do not display UI.
    $page = Invoke-Webrequest $url
  $ProgressPreference = 'Continue'            # Subsequent calls do display UI.
  # get links from actual meetings and attach $baseurl
  $page.links.href |?{ ($_.Split("/").GetUpperBound(0) -gt  4) } |%{-join("$baseurl",$_)} |%{
    $url2 = $_
    #$url2
    $ProgressPreference = 'SilentlyContinue'    # Subsequent calls do not display UI.
      $page2 = Invoke-Webrequest $url2
    $ProgressPreference = 'Continue'            # Subsequent calls do display UI.
    $page2.links.href |?{($_ -like "*.pdf*") -or ($_ -like "*.doc*") -or ($_ -like "*.txt*") -or ($_ -like "*.xls*") -or ($_ -like "*.ppt*") -or ($_ -like "*.rtf*")} |%{-join("$baseurl",$_)} | Out-File -FilePath $output -Append
	}
}



