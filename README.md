# Ustawienie preferencji błędów
$ErrorActionPreference = "Stop"

# Funkcja do czyszczenia pamięci podręcznej
function Clear-Cache {
    Write-Host "Czyszczenie pamięci podręcznej..."
    cmd.exe /c "cleanmgr /sagerun:1"
    Write-Host "Pamięć podręczna wyczyszczona."
}

# Funkcja do usuwania zbędnych plików
function Remove-TemporaryFiles {
    Write-Host "Usuwanie zbędnych plików..."
    $tempPath = [System.IO.Path]::GetTempPath()
    Remove-Item "$tempPath\*" -Recurse -Force -ErrorAction SilentlyContinue
    Write-Host "Zbędne pliki usunięte."
}

# Funkcja do wyłączania zbędnych usług startowych
function Disable-UnnecessaryServices {
    Write-Host "Wyłączanie zbędnych usług startowych..."
    $services = @(
        "DiagTrack",
        "dmwappushservice",
        "MapsBroker"
    )
    foreach ($service in $services) {
        Set-Service -Name $service -StartupType Disabled
        Write-Host "Usługa $service wyłączona."
    }
}

# Funkcja do defragmentacji dysku
function Defragment-Disk {
    Write-Host "Defragmentacja dysku..."
    Optimize-Volume -DriveLetter C -Defrag -Verbose
    Write-Host "Dysk zdefragmentowany."
}

# Wykonanie wszystkich funkcji optymalizacyjnych
Clear-Cache
Remove-TemporaryFiles
Disable-UnnecessaryServices
Defragment-Disk

Write-Host "Optymalizacja systemu Windows 11 zakończona pomyślnie!"

# Czyszczenie pamięci RAM
function Clear-RAM {
    Write-Host "Oczyszczanie pamięci RAM..."
    [System.GC]::Collect()
    [System.GC]::WaitForPendingFinalizers()
    Write-Host "Pamięć RAM oczyszczona."
}

Clear-RAM

# Wyłączenie zbędnych usług:
function Disable-UnnecessaryServices {
    Write-Host "Wyłączanie zbędnych usług..."
    $services = @(
        "DiagTrack",
        "dmwappushservice",
        "MapsBroker"
    )
    foreach ($service in $services) {
        Set-Service -Name $service -StartupType Disabled
        Write-Host "Usługa $service wyłączona."
    }
}

Disable-UnnecessaryServices

# Zmniejszenie liczby aplikacji uruchamianych przy starcie:

function Disable-StartupApps {
    Write-Host "Wyłączanie aplikacji uruchamianych przy starcie..."
    $startupApps = Get-ItemProperty -Path "HKCU:\SOFTWARE\Microsoft\Windows\CurrentVersion\Run"
    foreach ($app in $startupApps.PSObject.Properties) {
        Remove-ItemProperty -Path "HKCU:\SOFTWARE\Microsoft\Windows\CurrentVersion\Run" -Name $app.Name
        Write-Host "Aplikacja $($app.Name) wyłączona przy starcie."
    }
}

Disable-StartupApps

# Ustawienie pliku stronicowania na określoną wartość:

function Set-PagingFileSize {
    param (
        [int]$InitialSize = 1024,
        [int]$MaximumSize = 4096
    )
    
    Write-Host "Ustawianie pliku stronicowania na określoną wartość..."
    wmic pagefileset where name="C:\\pagefile.sys" set InitialSize=$InitialSize,MaximumSize=$MaximumSize
    Write-Host "Plik stronicowania ustawiony na $InitialSize MB (początkowy) i $MaximumSize MB (maksymalny)."
}

Set-PagingFileSize -InitialSize 2048 -MaximumSize 8192

