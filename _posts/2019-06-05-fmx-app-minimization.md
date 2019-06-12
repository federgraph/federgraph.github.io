---
layout: post
title:  "FMX Minimization"
tags: Delphi
description: "How to properly minimize an FMX app via the taskbar icon."
---

# Fix for RSP-17322

An update regarding my FMX minimize bug fix for Delphi Rio Community.
In short, you need to add a patched `FMX.Platform.Win` to your project.
The workaround is the same as for Tokyo.
The two screenshots show you how.

[RSP-17322](https://quality.embarcadero.com/browse/RSP-17322){: .start-btn}

## Fix Part 1

*[fix-for-rio-part-1.png](images/fix/fix-for-rio-part-1.png)*<br>
![screenshot for fix part 1](images/fix/fix-for-rio-part-1.png)

```pascal
in FMX.Platform.Win 

WndProc

The 'optimization' is in the LForm <> nil branch (FormHandle)

          WM_WINDOWPOSCHANGED:
            begin
              Placement.Length := SizeOf(TWindowPlacement);
              GetWindowPlacement(hwnd, Placement);
              if (Application.MainForm <> nil) and (LForm = Application.MainForm)
                and (Placement.showCmd = SW_SHOWMINIMIZED) then
              begin
                //call MinimizeApp only once when minimiz box clicked
                if ( (PWindowPos(lParam)^.flags and SWP_HIDEWINDOW) <> SWP_HIDEWINDOW) //added
                and (PWindowPos(lParam)^.hWndInsertAfter = 0) //added
                then
                begin
                  //call only once after mouse click on minimize box.
                  PlatformWin.MinimizeApp;
                end;
                Result := DefWindowProc(hwnd, uMsg, wParam, lParam);
              end
              else
              begin
                //unchanged
              end;
```

## Fix part 2

*[fix-for-rio-part-2.png](images/fix/fix-for-rio-part-2.png)*<br>
![screenshot for fix part 2](images/fix/fix-for-rio-part-2.png)

```pascal
in FMX.Platform.Win 

WndProc

the fix code is in the LForm = nil branch (ApplicationHandle)

        WM_SYSCOMMAND: //new code for claim 3)
        begin
          case wParam of
            SC_MINIMIZE:
            begin
              Winapi.Windows.ShowWindow(FormToHWND(Application.MainForm), SW_MINIMIZE);
            end;
          end;
          Result := DefWindowProc(hwnd, uMsg, wParam, lParam);
        end;

        WM_ACTIVATE: //new code for claim 2)
        begin
          //this makes the MainForm have focus after restoring
          case LoWord(wParam) of
            WA_ACTIVE:
            begin
              if (HiWord(wParam) = 0) and (lParam = 0) then
              begin
                PlatformWin.RestoreApp;
              end;
            end;
          end;
        end;

        WM_ACTIVATEAPP: //changed code for claim 1)
        begin
          Result := DefWindowProc(hwnd, uMsg, wParam, lParam);
          //added lParam test needed for minimization via taskbar button
          if BOOL(wParam) and (lParam <> 0) then
            PlatformWin.RestoreApp;
        end;
```

## From original G+ post

I think I figured it out and here is my unofficial
update to `FMX.Platform.Win` ready for beta testing in your real app.

Improvement claims:
1. click on task bar button will now minimize the app
2. restore from minimized will now focus the main form
3. restore from minimized will now restore docked state

Steps:
- create new FMX app
- add local copy of FMX.Platform.Win
- apply fix in LForm=nil branch of WndProc

Include this in your testing:
1. dock MainForm to the edge of the Windows 10 desktop
1. minimize via click on taskbar icon
1. restore

## RSP numbers

RSP-numbers can be confusing

Snapshot from 27.11.2018:

```
21809 - open (3 votes)
18102 - open (0 votes)
17322 - reported (18 votes)
17033 - resolved/Duplicate (3 votes)
17285 - resolved/Duplicate (3 votes)
```
