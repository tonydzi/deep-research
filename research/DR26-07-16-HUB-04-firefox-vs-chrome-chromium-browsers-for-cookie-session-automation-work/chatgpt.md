# DR26-07-16-HUB-04 — raw report from chatgpt

> Unedited output of one model. Our reconciled synthesis is the page one level up.

<!-- PROVENANCE
source: ChatGPT Deep Research (Anton Dz Pro), chat https://приватный чат
dr_id: DR26-07-16-HUB-04
topic: headless browser automation on managed Win11 (SxS mozglue / geckodriver stock Firefox)
collected: 2026-07-16  machine: хаб  operator: Anton

-->

# Reliable Headless Browser Automation with Logged-In Sessions on Managed Windows 11

## Executive Summary

**Primary recommendation:** stop treating the broken Playwright Firefox bundle as the main path on this machine. Use the **installed stock Firefox in `C:\Program Files\Mozilla Firefox\firefox.exe` plus Selenium + geckodriver**, and keep logins inside a **separate dedicated Firefox profile** that you create once, log into manually once, and then reuse headlessly with `-profile`. This is the lowest-maintenance path because it uses Mozilla’s supported automation stack for Firefox, targets a browser binary that already works on this host, and avoids the Juggler-patched Playwright Firefox build that is failing during Windows side-by-side activation. Confidence: **established** for the recommendation; **established** that Playwright Firefox is Juggler-based and that Selenium/geckodriver is Mozilla’s supported WebDriver path. citeturn41view1turn41view3turn28search1turn28search4

**Fallback:** if a target site behaves materially better in Chromium, use **Playwright Chromium or branded Chrome/Edge only with a separate automation profile**, not the owner’s real default profile. Playwright’s own docs now warn that automating Chrome’s default profile is unsupported, and Chrome 136+ requires `--user-data-dir` pointing to a non-standard directory for remote debugging behavior on the default profile anyway. Confidence: **established**. citeturn38view0turn42view0

**Do not build the standard operating procedure around Playwright + stock Firefox via `executablePath`.** Public Playwright Firefox is still centered on its patched Firefox/Juggler world; the public protocol selection for BiDi vs Juggler is still not a stable public interface, and custom Firefox builds have open compatibility issues in current Playwright. Confidence: **emerging** for “won’t work in every environment,” **established** for “not a stable supported path to depend on.” One of the compatibility claims below is GitHub-issue based and therefore marked **single-source** where appropriate. citeturn28search4turn28search5turn21view2

## Why Playwright Firefox is failing here

The failure signature you reported is a **Windows side-by-side activation** problem, not a generic “Firefox cannot start” problem. Windows side-by-side activation uses manifests to resolve private and shared assemblies. Microsoft’s documentation is explicit that the OS first checks shared assemblies in WinSxS and then searches for a **private assembly** under the application’s directory structure; it is also explicit that correct installation/execution requires the manifest to accompany the assembly. Mozilla’s Windows manifests in Gecko products declare a dependency on a private assembly named **`mozglue` version `адрес узла`**, which matches the SideBySide event text you are seeing. Confidence: **established**. citeturn32view0turn32view3turn13search3turn14search2

That immediately narrows the technical hypothesis space. If `mozglue.dll` is physically present but Event Log still says **“Dependent Assembly mozglue, version адрес узла could not be found,”** the loader is telling you that **file presence alone was not sufficient to satisfy the assembly identity**. Microsoft documents two important reasons this can happen: for private side-by-side assemblies, the manifest must accompany the assembly; and for DLLs loaded by the OS loader, the relevant manifest may need to be embedded in the DLL as an RT_MANIFEST resource, because an external DLL manifest is **not** checked during `LoadLibrary`. Confidence: **established**. citeturn32view1turn32view2turn32view3

On your exact machine, the likely causes rank like this.

### Most likely cause

The highest-probability explanation is **policy interference with a user-space, patched browser bundle under `%LOCALAPPDATA%\ms-playwright\...`**, not a global Firefox problem. AppLocker and App Control for Business can control **executables and DLLs**, and the relevant diagnostic logs for blocks are the **CodeIntegrity/Operational** and **AppLocker** channels. Microsoft’s AppLocker guidance also describes how applications in **nonstandard locations** are common sources of allowlist misses. Separately, a Playwright Windows issue documents AppLocker blocking Playwright’s bundled Chromium specifically because the executable was unsigned and lived under `AppData\Local\ms-playwright` rather than `Program Files`. That issue is **not about Firefox**, so the analogy to your case is an **inference**, but it is a strong one given your environment and the fact that stock Firefox in `Program Files` works while the Playwright bundle in user space does not. Confidence: **emerging** for your exact root cause; the specific Playwright-AppLocker example is **single-source**. citeturn30search2turn35view0turn35view1turn21view0

### Next most likely cause

The next most likely explanation is **broken or stripped manifest metadata inside the Playwright Firefox bundle**, especially in `mozglue.dll` or `firefox.exe`. Microsoft’s side-by-side documentation explains that private assemblies are identity-driven, not filename-driven. If you extract the embedded manifests and either the expected manifest is missing, or the `assemblyIdentity` does not match `mozglue` / `адрес узла`, you have deterministic confirmation that the bundle is not offering Windows a satisfiable private assembly definition, even though the DLL bytes are on disk. Confidence: **established** as a failure mode; **emerging** as the cause on this machine until you run `mt.exe`. citeturn32view1turn32view2turn32view3turn32view5

### Less likely causes

A broader OS-side WinSxS/component-store problem is **lower probability** here because the installed stock Firefox on the same machine starts normally. Microsoft documents that the loader first checks shared assemblies and then private ones, so a host-wide component-store break would be expected to hit more than one build pattern. It is still worth checking only if the fusion trace points there, but it is not where I would place engineering time first. Confidence: **established** that it is lower priority, based on Microsoft’s search order plus your working stock Firefox fact pattern. citeturn32view0

### Deterministic diagnostics

The right diagnostic path is: **fusion trace first, then manifest inspection, then policy logs**. Windows itself points you to `sxstrace.exe` for side-by-side failures, Microsoft documents the App Control/AppLocker event channels for executable/DLL blocks, and Microsoft’s manifest tool (`mt.exe`) can extract embedded RT_MANIFEST resources for inspection. Confidence: **established**. citeturn35view1turn35view0turn32view4turn32view5

Run these from **PowerShell** or **cmd** as written. The first set does **not** need code changes.

```powershell
# 1) Fusion trace around the failing Playwright Firefox launch
sxstrace Trace -logfile:$env:TEMP\pwff.etl
# In another window, reproduce:
# "%LOCALAPPDATA%\ms-playwright\firefox-1532\firefox\firefox.exe" --version
# Then come back and stop sxstrace by pressing Enter
sxstrace Parse -logfile:$env:TEMP\pwff.etl -outfile:$env:TEMP\pwff.txt
notepad $env:TEMP\pwff.txt
```

```powershell
# 2) Recent SideBySide events
Get-WinEvent -FilterHashtable @{
  LogName='Application'
  ProviderName='SideBySide'
  StartTime=(Get-Date).AddMinutes(-30)
} | Sort-Object TimeCreated -Descending |
  Select-Object -First 20 TimeCreated, Id, LevelDisplayName, Message | Format-List
```

```powershell
# 3) Look for WDAC / App Control blocks
Get-WinEvent -LogName 'Microsoft-Windows-CodeIntegrity/Operational' -MaxEvents 200 |
  Where-Object { $_.Message -match 'ms-playwright|firefox|mozglue' } |
  Select-Object TimeCreated, Id, Message | Format-List

# 4) Look for AppLocker EXE/DLL blocks or allows
Get-WinEvent -LogName 'Microsoft-Windows-AppLocker/EXE and DLL' -MaxEvents 200 |
  Where-Object { $_.Message -match 'ms-playwright|firefox|mozglue' } |
  Select-Object TimeCreated, Id, Message | Format-List
```

```powershell
# 5) If mt.exe exists, extract manifests
where.exe mt

# EXE application manifest
mt.exe -inputresource:"%LOCALAPPDATA%\ms-playwright\firefox-1532\firefox\firefox.exe";#1 `
  -out:"%TEMP%\pw-firefox.exe.manifest"

# DLL manifest candidates: #1 for assembly manifest, #2 for DLL app manifest
mt.exe -inputresource:"%LOCALAPPDATA%\ms-playwright\firefox-1532\firefox\mozglue.dll";#1 `
  -out:"%TEMP%\pw-mozglue-asm.manifest"

mt.exe -inputresource:"%LOCALAPPDATA%\ms-playwright\firefox-1532\firefox\mozglue.dll";#2 `
  -out:"%TEMP%\pw-mozglue-app.manifest"
```

What you are looking for is simple. If `pwff.txt` shows fusion searching for `mozglue` and refusing the local folder because an assembly identity is missing or mismatched, the problem is manifest/assembly identity. If the Code Integrity log shows **3077** or related events for the browser or DLLs, the problem is policy. Microsoft documents **3077** as the main enforced block event and **3089** as the correlated signature detail event. Confidence: **established**. citeturn35view1

### Practical fixes without admin rights

Without admin privileges, the only “real” fixes are **diagnose and pivot**, not heroics. In order: capture `sxstrace`; inspect manifests if `mt.exe` is present; inspect `CodeIntegrity` and `AppLocker` logs; and if either policy or manifest failure is confirmed, **stop using Playwright Firefox on this host** and switch to stock Firefox automation. That is the repairable path for a non-technical owner because it removes the fragile component instead of fighting machine policy from user space. Confidence: **established**. citeturn35view1turn41view1turn41view3

### Admin-only fixes

If the owner is willing to involve admin rights, the admin path is: export the effective AppLocker policy; review CodeIntegrity and AppLocker events; explicitly allow the required executable/DLL set if policy is blocking it; and only then retest. Microsoft documents `Get-AppLockerPolicy -Effective` for exporting the effective policy and documents policy-testing workflows with `Test-AppLockerPolicy`. If component-store corruption is actually implicated by fusion traces, the supported repair track is **DISM /RestoreHealth** and then **SFC /scannow**. Confidence: **established**. citeturn17view3turn17view4turn16search0turn16search1

```powershell
# Effective AppLocker policy export
Get-AppLockerPolicy -Effective -Xml > "$env:TEMP\applocker-effective.xml"

# If component-store corruption is implicated
DISM /Online /Cleanup-Image /RestoreHealth
sfc /scannow
```

## Driving the installed Firefox instead of Playwright Firefox

### Selenium plus geckodriver and Marionette

This is the cleanest path for installed Firefox on Windows. Mozilla’s `geckodriver` is the WebDriver proxy for Gecko browsers; it translates WebDriver commands into the Marionette remote protocol. Selenium’s Firefox docs explicitly support using a custom Firefox binary and Mozilla’s `firefoxOptions` explicitly supports **reusing an existing profile on local disk** by passing `["-profile", "/path/to/profile"]`. The same `args` mechanism supports `["-headless"]`. Confidence: **established**. citeturn41view1turn41view2turn41view3

Operationally, this gives you exactly what you asked for: vendor-supported automation of the **working installed Firefox**, with a **persistent logged-in profile**, in **headless mode**, with a repair surface that is small enough for a non-technical owner. The main downside is detection surface: Firefox sets `navigator.webdriver = true` when Marionette is enabled. Confidence: **established**. citeturn19search14turn18search2

### WebDriver BiDi

BiDi is worth using **through Selenium**, not as the primary stack by itself. Selenium documents that enabling BiDi is a capability toggle (`webSocketUrl` / `enable_bidi` depending on language binding), and Firefox supports BiDi through its Remote Agent/WebDriver work. This is good if you need richer network/event tooling, but for a “mortar and screwdriver” Windows owner the simplest pattern is still **Selenium WebDriver session first, BiDi optionally enabled on top**. Confidence: **established**. citeturn18search0turn19search3

### Direct `-marionette` or raw Remote Agent

Yes, this is technically possible. Mozilla documents that Marionette is only enabled when `--marionette` or `MOZ_MARIONETTE` is specified, and geckodriver documents a `--connect-existing` mode for attaching to an existing Firefox instance with Marionette already enabled. Firefox’s Remote Agent for BiDi/CDP-style debugging can also be started with `--remote-debugging-port`, and Mozilla documents that it accepts only loopback connections by default. Confidence: **established**. citeturn19search0turn41view0turn19search3

I do **not** recommend direct Marionette or raw Remote Agent here as the primary path. It is more fragile, requires manual port/process/profile discipline, and buys you no simplicity over Selenium+geckodriver on this host. Confidence: **established**. citeturn41view0turn41view1

### Playwright with `executablePath` pointing to stock Firefox

This is the wrong thing to standardize on. Playwright’s public Firefox path is still bound up with its patched Firefox/Juggler implementation; the Playwright repository itself contains `browser_patches/firefox/juggler`, and there is an open feature request to make protocol selection public because `_bidiFirefox` is still an internal/non-public API. There is also a fresh open issue reporting **custom Firefox builds broken with Playwright since v1.61.0**. That issue is GitHub-only evidence, so I am marking that specific compatibility claim **single-source**. Confidence overall: **emerging** for current custom-build instability, **established** that it is not the low-risk supported route. citeturn28search1turn28search4turn28search5turn21view2

### Puppeteer and similar alternatives

Puppeteer is now a real cross-browser option: it supports Firefox and Chrome, and for Firefox it uses WebDriver BiDi by default. It also supports `userDataDir` for persistent state. So yes, Puppeteer can automate Firefox or Chrome with a persistent profile. Confidence: **established**. citeturn26search1turn26search6turn27search0

I still would not choose Puppeteer as the main answer for this Windows box because it adds a **Node** runtime and another package-management surface while buying you little over Selenium for the installed Firefox case. It is a fine **secondary** option; it is not the lowest-maintenance option. Confidence: **established**. citeturn26search5turn26search6

## Chromium as the fallback on this machine

If you test Playwright Chromium next, you should **not** expect the same exact failure class as Playwright Firefox’s `mozglue` Fusion error, because the Firefox failure is specifically about Gecko’s `mozglue` private assembly activation. If Chromium fails on this box, the more likely culprits are **enterprise policy, signature trust, or AppLocker/WDAC**, not `mozglue`-style SxS. Playwright’s own docs warn that enterprise browser policies can affect launch/control of Chrome and Edge, and the Playwright Windows AppLocker issue shows bundled Chromium under `AppData\Local\ms-playwright` getting blocked in the field. The extension of that issue to your machine is still an inference, and therefore not a vendor guarantee. Confidence: **emerging** for your host, with one **single-source** field report. citeturn21view3turn21view0

If **you own the browser profile from the beginning** and automate **inside that same browser/profile**, App-Bound Encryption and DBSC are **not blockers** in the way cookie-extraction malware is blocked. Google’s App-Bound encryption design binds encrypted data to the browser’s application identity; a different app cannot decrypt it, but the browser itself can. Chrome’s 2025 remote-debugging hardening also explicitly says that a **non-standard user-data-dir uses a different encryption key**, which is exactly why Chrome now requires `--user-data-dir` for debugging scenarios involving non-default profiles. Confidence: **established** that external extraction is what is being constrained; **established** that non-standard automation profiles are the intended safe pattern. citeturn43view0turn42view0turn42view1

The **ban-risk / automation-detection risk** is meaningfully worse for headless WebDriver-class automation than for your already-working “live Chrome tab via assistant extension” path. MDN documents that `navigator.webdriver` is true in Chrome when `--enable-automation`, `--headless`, or remote debugging mode is used in the documented cases, and in Firefox when Marionette is enabled. That does **not** mean “instant ban”; it means the automation surface is larger and easier for anti-bot systems to classify. By contrast, an extension operating a real already-open tab can be much closer to ordinary browsing depending on how it is implemented. That comparison is an **inference**, but it is a solid one. Confidence: **established** for the signal; **emerging** for site-specific ban outcomes. citeturn18search2turn19search14turn42view0

## Best practice for a managed Windows machine

On this class of endpoint, the most durable pattern is:

| Option | Why it is durable here | What can still go wrong | Confidence |
|---|---|---|---|
| **Installed Firefox + Selenium/geckodriver + separate automation profile** | Signed installed browser, Mozilla-supported automation path, persistent profile reuse via `-profile`, smallest stack on top of a browser that already launches on this host. | `navigator.webdriver` is exposed; don’t share the profile with daily browsing while automation runs. | **Established** citeturn41view1turn41view3turn40view0 |
| **Chromium fallback with separate automation profile** | Good when a target is Chromium-first; Playwright persistent context is strong when you own the profile from day one. | Enterprise browser policies may interfere; default Chrome profile automation is explicitly discouraged by Playwright and Chrome. | **Established** citeturn38view0turn21view3turn42view0 |
| **Puppeteer Firefox/Chrome** | Technically good, persistent profiles available. | Extra Node moving parts. | **Established** citeturn26search1turn27search0 |
| **Direct Marionette / raw Remote Agent / connect-existing** | Powerful. | More fragile, more knobs, worse repairability. | **Established** citeturn41view0turn19search3 |
| **Portable Firefox in user space** | Fine on ordinary machines. | On managed Windows it is more likely to run into AppLocker/path/publisher policy concerns than a signed installed Firefox in `Program Files`. This is an inference from Microsoft AppLocker guidance and Playwright’s AppLocker field report. | **Emerging** citeturn30search2turn31view0turn21view0 |

The profile strategy matters more than people think. Mozilla explicitly documents that Firefox uses **dedicated profiles per installation**, and its Profile Manager lets you create and switch profiles cleanly. In practice, the stable pattern is to keep **one automation-only profile** for Google/ChatGPT/X, log into it once manually, and then never use the owner’s daily Firefox profile for headless jobs. That avoids profile-lock contention and reduces the blast radius if a site session needs a fresh login every few weeks. Confidence: **established**. citeturn40view0turn40view1turn22search4

I would also keep the anti-detect posture conservative. Use a real installed browser, a real persistent profile, ordinary window sizes, and no malware-class evasion such as process injection or signature bypassing. The standards-based automation stacks already expose `navigator.webdriver`; trying to “stealth patch” hardened corporate Windows endpoints usually increases EDR and ToS risk faster than it decreases bot risk. Confidence: **established** for the webdriver signal; the operational recommendation is engineering judgment. citeturn18search2turn19search14

## Concrete Windows and Python runbook

The runbook below is the one I would actually hand to a non-technical owner.

### Create the dedicated Firefox profile and log in once

Mozilla documents the Windows Profile Manager entry point as `firefox.exe -P`, and the Profile Manager / `about:profiles` UI shows the **Root Directory** of each profile. Use that instead of guessing hidden profile-folder names. Confidence: **established**. citeturn40view0

```powershell
# Open Firefox Profile Manager for the installed Firefox
& "C:\Program Files\Mozilla Firefox\firefox.exe" -P
```

In the UI, create a profile named **AutoFF**. Then launch that profile, open `about:profiles`, and copy the **Root Directory** path. Log into the needed services manually in that profile, complete any 2FA, and close Firefox cleanly. Confidence: **established**. citeturn40view0

### Install the minimal Python stack

Modern Selenium ships with Selenium Manager and can manage driver discovery automatically, so the smallest first attempt is just Python plus Selenium. If Selenium Manager is blocked by proxy/policy, pin a manual `geckodriver.exe` from Mozilla’s release page instead. Confidence: **established**. citeturn25search0turn25search12turn24view0turn41view1

```powershell
py -3 -m venv .venv
.venv\Scripts\Activate.ps1
python -m pip install -U pip selenium
```

### Use this Python script for headless reuse of the logged-in profile

Mozilla’s `firefoxOptions` supports `-profile` for an existing profile and `-headless` as a normal Firefox argument. Confidence: **established**. citeturn41view3

```python
from pathlib import Path
import json
from selenium import webdriver
from selenium.webdriver.firefox.options import Options
from selenium.webdriver.support.ui import WebDriverWait

FIREFOX_BIN = r"C:\Program Files\Mozilla Firefox\firefox.exe"
PROFILE_DIR = r"C:\REPLACE_WITH_THE_ROOT_DIRECTORY_FROM_about_profiles"
TARGET_URL = "https://chatgpt.com/"

OUT = Path("out")
OUT.mkdir(exist_ok=True)

opts = Options()
opts.binary_location = FIREFOX_BIN
opts.add_argument("-profile")
opts.add_argument(PROFILE_DIR)
opts.add_argument("-headless")

# Optional but useful if later you want BiDi features through Selenium
opts.set_capability("webSocketUrl", True)

# Smallest-stack path: let Selenium Manager locate/provision geckodriver
driver = webdriver.Firefox(options=opts)

try:
    driver.get(TARGET_URL)
    WebDriverWait(driver, 30).until(
        lambda d: d.execute_script("return document.readyState") == "complete"
    )

    result = {
        "url": driver.current_url,
        "title": driver.title,
        "cookie_count": len(driver.get_cookies()),
    }

    (OUT / "result.json").write_text(
        json.dumps(result, indent=2, ensure_ascii=False),
        encoding="utf-8",
    )
    (OUT / "page.html").write_text(driver.page_source, encoding="utf-8")
    driver.save_screenshot(str(OUT / "page.png"))

finally:
    driver.quit()
```

Run it like this:

```powershell
python .\run_ff.py
```

### If Selenium Manager cannot fetch geckodriver

Mozilla’s geckodriver release page is the canonical download point. The latest visible release in the current upstream page is **0.36.0**. Confidence: **established**. citeturn24view0

After placing `geckodriver.exe` somewhere stable such as `C:\Tools\geckodriver\geckodriver.exe`, replace driver startup with this:

```python
from selenium.webdriver.firefox.service import Service

service = Service(
    executable_path=r"C:\Tools\geckodriver\geckodriver.exe",
    log_output="geckodriver.log",
)

driver = webdriver.Firefox(service=service, options=opts)
```

### Operational rules that keep this stable

Keep **only one process owner** for that profile: do not leave the same Firefox profile open in normal Firefox while the headless job uses it. Playwright documents the same general truth for persistent browser directories, and Mozilla’s support docs around profile locks explain the failure mode when a profile is still in use or left locked. Confidence: **established**. citeturn38view0turn22search4

Expect occasional re-logins. That is normal for major identity providers. The reason the dedicated automation profile still wins is not “infinite session life”; it is that re-auth happens in **one controlled place** rather than in a shared daily profile that you also browse with. Confidence: **engineering judgment**, consistent with browser profile behavior documented by Mozilla. citeturn40view0turn40view1

## Final recommendation and command checklist

**Main path:** **Installed Firefox + Selenium/geckodriver + dedicated profile**.  
**Fallback:** **Playwright Chromium or branded Chrome/Edge with a separate automation profile**, never the default real profile.  
**Do not use as standard path:** Playwright Firefox on this machine until `sxstrace` + manifest extraction + policy logs prove a fixable cause. Confidence: **established** for the main recommendation; **emerging** for whether Playwright Firefox can be salvaged on this host at all. citeturn41view1turn41view3turn38view0turn28search4turn28search5

Use these commands, in this order:

```powershell
# Diagnose the broken Playwright Firefox bundle
sxstrace Trace -logfile:$env:TEMP\pwff.etl
# reproduce the failing "%LOCALAPPDATA%\ms-playwright\firefox-1532\firefox\firefox.exe" --version
sxstrace Parse -logfile:$env:TEMP\pwff.etl -outfile:$env:TEMP\pwff.txt

Get-WinEvent -FilterHashtable @{LogName='Application'; ProviderName='SideBySide'; StartTime=(Get-Date).AddMinutes(-30)} |
  Sort-Object TimeCreated -Descending |
  Select-Object -First 20 TimeCreated,Id,Message | Format-List

Get-WinEvent -LogName 'Microsoft-Windows-CodeIntegrity/Operational' -MaxEvents 200 |
  ? {$_.Message -match 'ms-playwright|firefox|mozglue'} |
  fl TimeCreated,Id,Message

Get-WinEvent -LogName 'Microsoft-Windows-AppLocker/EXE and DLL' -MaxEvents 200 |
  ? {$_.Message -match 'ms-playwright|firefox|mozglue'} |
  fl TimeCreated,Id,Message
```

```powershell
# Create the separate Firefox profile
& "C:\Program Files\Mozilla Firefox\firefox.exe" -P
# In Firefox, open about:profiles, copy the Root Directory path for AutoFF
```

```powershell
# Minimal Python setup
py -3 -m venv .venv
.venv\Scripts\Activate.ps1
python -m pip install -U pip selenium
python .\run_ff.py
```

```powershell
# If admin is available and policy / component-store repair is required
Get-AppLockerPolicy -Effective -Xml > "$env:TEMP\applocker-effective.xml"
DISM /Online /Cleanup-Image /RestoreHealth
sfc /scannow
```

**Bottom line:** the “AK-47” answer for this machine is **stock Firefox that already starts + Selenium/geckodriver + dedicated automation profile**. Everything else is downstream of that. citeturn41view1turn41view3turn40view0