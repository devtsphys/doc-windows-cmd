# Windows CMD Scripting Reference Card

## Basic Syntax & Structure

### Script Header

```cmd
@echo off
REM Script description
setlocal enabledelayedexpansion
```

### Comments

```cmd
REM This is a comment
:: This is also a comment (faster)
```

## Variables

### Setting Variables

```cmd
set VAR=value
set /a NUM=5+3          REM Arithmetic
set /p INPUT=Enter:     REM User input
```

### Using Variables

```cmd
echo %VAR%              REM Standard expansion
echo !VAR!              REM Delayed expansion (requires setlocal enabledelayedexpansion)
```

### Environment Variables

```cmd
%CD%                    REM Current directory
%DATE%                  REM Current date
%TIME%                  REM Current time
%RANDOM%                REM Random number (0-32767)
%ERRORLEVEL%            REM Last command exit code
%COMPUTERNAME%          REM Computer name
%USERNAME%              REM Current user
%USERPROFILE%           REM User profile path
%PROGRAMFILES%          REM Program Files directory
%TEMP% / %TMP%          REM Temporary directory
```

### Parameter Variables

```cmd
%0                      REM Script name
%1-%9                   REM Arguments 1-9
%*                      REM All arguments
shift                   REM Shift arguments left
```

## Control Structures

### IF Statements

```cmd
REM String comparison
if "%VAR%"=="value" (
    echo Match
) else (
    echo No match
)

REM Numeric comparison
if %NUM% EQU 5 echo Equal
if %NUM% NEQ 5 echo Not equal
if %NUM% LSS 5 echo Less than
if %NUM% LEQ 5 echo Less or equal
if %NUM% GTR 5 echo Greater than
if %NUM% GEQ 5 echo Greater or equal

REM File/Directory checks
if exist file.txt echo File exists
if not exist file.txt echo File missing
if exist "C:\Folder\" echo Directory exists

REM Other conditions
if defined VAR echo Variable is set
if errorlevel 1 echo Error occurred
if /i "%VAR%"=="VALUE" echo Case-insensitive match
```

### FOR Loops

#### Basic FOR Loop

```cmd
for %%i in (1 2 3 4 5) do echo %%i
for %%f in (*.txt) do echo %%f
```

#### FOR /L - Numeric Loop

```cmd
for /l %%i in (1,1,10) do echo %%i
REM Syntax: (start,step,end)
```

#### FOR /F - File/String Processing

```cmd
REM Read lines from file
for /f %%i in (file.txt) do echo %%i

REM Parse delimited data
for /f "tokens=1,2 delims=," %%a in (data.csv) do (
    echo First: %%a Second: %%b
)

REM Skip lines and use specific tokens
for /f "skip=1 tokens=1-3 delims=," %%a in (file.csv) do (
    echo %%a %%b %%c
)

REM Process command output
for /f %%i in ('dir /b *.txt') do echo %%i
```

#### FOR /D - Directory Loop

```cmd
for /d %%d in (*) do echo %%d
```

#### FOR /R - Recursive Loop

```cmd
for /r "C:\Path" %%f in (*.txt) do echo %%f
```

### GOTO and Labels

```cmd
goto :label

:label
echo At label
goto :eof

:eof
REM End of file
```

### CALL - Subroutines

```cmd
call :subroutine arg1 arg2
goto :eof

:subroutine
echo Arg1: %1
echo Arg2: %2
exit /b 0
```

## String Operations

### Substrings

```cmd
set STR=HelloWorld
echo %STR:~0,5%         REM Hello (start at 0, length 5)
echo %STR:~5%           REM World (from position 5 to end)
echo %STR:~-5%          REM World (last 5 characters)
```

### Find and Replace

```cmd
set STR=Hello World
set NEW=%STR:World=Universe%
echo %NEW%              REM Hello Universe

set PATH_STR=C:\Path\To\File
set UNIX=%PATH_STR:\=/%
echo %UNIX%             REM C:/Path/To/File
```

### String Length

```cmd
set STR=Hello
set LEN=0
:loop
if defined STR (
    set STR=%STR:~1%
    set /a LEN+=1
    goto :loop
)
echo Length: %LEN%
```

## Arithmetic Operations

### Basic Math

```cmd
set /a RESULT=5+3       REM Addition: 8
set /a RESULT=5-3       REM Subtraction: 2
set /a RESULT=5*3       REM Multiplication: 15
set /a RESULT=5/3       REM Division: 1 (integer)
set /a RESULT=5%%3      REM Modulo: 2
set /a RESULT=(5+3)*2   REM Parentheses: 16
```

### Bitwise Operations

```cmd
set /a RESULT=5^3       REM XOR
set /a RESULT=5^&3      REM AND
set /a RESULT=5^|3      REM OR
set /a RESULT=~5        REM NOT
set /a RESULT=5^<^<2    REM Left shift
set /a RESULT=5^>^>1    REM Right shift
```

## File Operations

### Reading Files

```cmd
REM Read entire file
for /f "delims=" %%i in (file.txt) do echo %%i

REM Read line by line
set LINE=0
for /f "delims=" %%i in (file.txt) do (
    set /a LINE+=1
    echo Line !LINE!: %%i
)
```

### Writing Files

```cmd
echo Content > file.txt         REM Overwrite
echo More >> file.txt           REM Append
(
    echo Line 1
    echo Line 2
    echo Line 3
) > output.txt
```

### File Attributes

```cmd
attrib +r file.txt              REM Set read-only
attrib -h file.txt              REM Remove hidden
attrib +s +h folder             REM System and hidden
```

## Error Handling

### Check ERRORLEVEL

```cmd
command
if %ERRORLEVEL% neq 0 (
    echo Command failed with code %ERRORLEVEL%
    exit /b %ERRORLEVEL%
)
```

### Conditional Execution

```cmd
command && echo Success         REM Execute if previous succeeded
command || echo Failed          REM Execute if previous failed
command1 && command2 && command3
```

### Error Redirection

```cmd
command 2>nul                   REM Suppress errors
command >output.txt 2>&1        REM Redirect stdout and stderr
command 2>errors.txt            REM Redirect only errors
```

## Essential Commands Table

|Command   |Description            |Example                           |
|----------|-----------------------|----------------------------------|
|`cd`      |Change directory       |`cd C:\Windows`                   |
|`dir`     |List directory contents|`dir /s /b *.txt`                 |
|`copy`    |Copy files             |`copy file.txt backup.txt`        |
|`xcopy`   |Extended copy          |`xcopy /s /e source dest`         |
|`move`    |Move files             |`move file.txt C:\Temp`           |
|`del`     |Delete files           |`del /f /q *.tmp`                 |
|`rd`      |Remove directory       |`rd /s /q folder`                 |
|`mkdir`   |Create directory       |`mkdir newfolder`                 |
|`type`    |Display file contents  |`type file.txt`                   |
|`find`    |Search in files        |`find "text" file.txt`            |
|`findstr` |Advanced search        |`findstr /i /n "pattern" *.txt`   |
|`more`    |Display page by page   |`type file.txt | more`            |
|`sort`    |Sort text              |`sort file.txt`                   |
|`tasklist`|List processes         |`tasklist /fi "status eq running"`|
|`taskkill`|Kill process           |`taskkill /im notepad.exe /f`     |
|`start`   |Start program          |`start "" notepad.exe`            |
|`timeout` |Pause execution        |`timeout /t 5 /nobreak`           |
|`choice`  |User prompt            |`choice /c yn /m "Continue?"`     |
|`ping`    |Network test           |`ping -n 1 google.com`            |
|`ipconfig`|Network config         |`ipconfig /all`                   |
|`net`     |Network commands       |`net user username password /add` |
|`sc`      |Service control        |`sc query wuauserv`               |
|`reg`     |Registry operations    |`reg query HKLM\Software`         |
|`wmic`    |WMI commands           |`wmic cpu get name`               |

## Advanced Techniques

### Menu System

```cmd
:menu
cls
echo ==================
echo 1. Option One
echo 2. Option Two
echo 3. Exit
echo ==================
choice /c 123 /n /m "Select: "
if errorlevel 3 goto :eof
if errorlevel 2 goto :option2
if errorlevel 1 goto :option1

:option1
echo You selected option 1
pause
goto :menu

:option2
echo You selected option 2
pause
goto :menu
```

### Progress Indicator

```cmd
@echo off
setlocal enabledelayedexpansion
set /a count=0
for /l %%i in (1,1,50) do (
    set /a count+=2
    set /a dots=%%i/5
    set "bar="
    for /l %%j in (1,1,!dots!) do set "bar=!bar!█"
    <nul set /p "=Progress: [!bar!] !count!%%"
    timeout /t 1 /nobreak >nul
    echo.
)
```

### Function Library Pattern

```cmd
@echo off
if "%1"=="" goto :main
goto :%1 2>nul || echo Function %1 not found && exit /b 1

:main
call :greet "World"
call :add 5 3
exit /b

:greet
echo Hello %~1!
exit /b

:add
set /a result=%1+%2
echo Result: %result%
exit /b
```

### Array Simulation

```cmd
@echo off
setlocal enabledelayedexpansion

REM Set array elements
set arr[0]=Apple
set arr[1]=Banana
set arr[2]=Cherry

REM Access elements
echo First: !arr[0]!

REM Loop through array
for /l %%i in (0,1,2) do (
    echo Item %%i: !arr[%%i]!
)
```

### Date/Time Formatting

```cmd
@echo off
REM Get current date components
for /f "tokens=1-4 delims=/ " %%a in ('date /t') do (
    set dow=%%a
    set month=%%b
    set day=%%c
    set year=%%d
)

REM Get current time
for /f "tokens=1-3 delims=:. " %%a in ('echo %time%') do (
    set hour=%%a
    set min=%%b
    set sec=%%c
)

echo Date: %year%-%month%-%day%
echo Time: %hour%:%min%:%sec%
```

### Logging Function

```cmd
:log
echo [%date% %time%] %* >> script.log
echo [%date% %time%] %*
exit /b
```

### Command Line Argument Parsing

```cmd
@echo off
:parse
if "%~1"=="" goto :endparse
if /i "%~1"=="-h" goto :help
if /i "%~1"=="--verbose" set VERBOSE=1
if /i "%~1"=="-f" (
    set FILE=%~2
    shift
)
shift
goto :parse

:endparse
if defined VERBOSE echo Verbose mode enabled
if defined FILE echo File: %FILE%
exit /b

:help
echo Usage: script.cmd [-h] [--verbose] [-f filename]
exit /b
```

## Special Characters & Escaping

|Character|Escape|Usage                 |
|---------|------|----------------------|
|`^`      |`^^`  |Caret (escape char)   |
|`&`      |`^&`  |Command separator     |
|`        |`     |`^                    |
|`<`      |`^<`  |Input redirect        |
|`>`      |`^>`  |Output redirect       |
|`%`      |`%%`  |In batch files        |
|`!`      |`^^!` |With delayed expansion|
|`"`      |`\"`  |Quote                 |

## Best Practices

1. **Always use `@echo off`** at the start to clean output
1. **Use `setlocal`** to prevent variable pollution
1. **Quote paths** with spaces: `"C:\Program Files\"`
1. **Use delayed expansion** for variables in loops: `!VAR!`
1. **Check ERRORLEVEL** after critical operations
1. **Add REM comments** to explain complex logic
1. **Use `exit /b`** for subroutines, `exit` for script
1. **Test with `echo`** before executing destructive commands
1. **Use `/b` flag** in loops: `for %%i` not `for %i`
1. **Validate input** before processing

## Common Patterns

### Check if Admin

```cmd
net session >nul 2>&1
if %errorlevel% neq 0 (
    echo Requires administrator privileges
    exit /b 1
)
```

### Backup Files

```cmd
set BACKUP_DIR=backup_%date:~-4,4%%date:~-10,2%%date:~-7,2%
mkdir %BACKUP_DIR%
xcopy /s /e /y source\* %BACKUP_DIR%\
```

### Find and Process Files

```cmd
for /r "C:\Logs" %%f in (*.log) do (
    if %%~zf gtr 10485760 (
        echo Large file: %%f (%%~zf bytes)
        move "%%f" "C:\Archive\"
    )
)
```

-----

**Note**: Replace `%%` with `%` when running commands directly in CMD (not in batch files).