# PowershellNote

```
# 請將下方變數改為你的測試組件路徑與 NUnit 執行檔路徑
$testAssembly = "C:\path\to\YourTests.dll"
$nunitConsole = "C:\Program Files\NUnit.org\nunit-console\nunit3-console.exe"

# 總失敗次數累計
$totalFailed = 0

for ($i = 1; $i -le 100; $i++) {
    Write-Host "`n=== 執行第 $i 次 NUnit 測試 ==="
    # 執行 NUnit 並捕捉所有輸出
    $output = & "$nunitConsole" $testAssembly --noresult --labels=Off 2>&1 | Out-String

    # 解析 Failed 數量
    $match = [regex]::Match($output, 'Failed:\s*(\d+)', 'IgnoreCase')
    if ($match.Success) {
        $fails = [int]$match.Groups[1].Value
    } else {
        $fails = 0
    }

    Write-Host "第 $i 次失敗數：$fails"
    $totalFailed += $fails
}

Write-Host "`n=== 全部 100 次執行累計失敗數：$totalFailed ==="

```
