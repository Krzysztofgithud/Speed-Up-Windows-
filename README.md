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
