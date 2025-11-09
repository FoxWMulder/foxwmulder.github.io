---
title: 12345
share: true
---
sdfdg

#delphi/functions 

URL: [ShellExecute in Delphi](https://delphiprogrammingdiary.blogspot.com/2014/07/shellexecute-in-delphi.html)
Local copy: 

```ad-desc
collapse: none

**ShellApi** unit

**Syntax of Windows API function**

HINSTANCE ShellExecute(  
  _In_opt_        HWND hwnd,  
  _In_opt_        LPCTSTR lpOperation,
  _In_              LPCTSTR lpFile,
  _In_opt_        LPCTSTR lpParameters,
  _In_opt_        LPCTSTR lpDirectory,
  _In_INT         nShowCmd
);

**Parameters Details**

_In_opt_  HWND hwnd,

A handle to the parent window used for displaying a UI or error messages. Ex - handle  


_In_opt_  LPCTSTR lpOperation,
Operations to perform.

*edit* = Launches an editor and opens the document for editing

*explore* = Explores a folder 

*find* = Initiates a search beginning in the directory 

*open* = Opens an item, file, folder, link

*print* = Prints the file

_In_      LPCTSTR lpFile,
FileName, link URL to open and modify. EX - PChar(filename)
 
_In_opt_  LPCTSTR lpParameters
the parameters to be passed to the application delimited by space. Ex - '-c -i -v'

_In_opt_  LPCTSTR lpDirectory,
the default (working) directory for the action.   

_In_      INT nShowCmd
how an application is to be displayed when it is opened.

**SW_HIDE** = Hides the window and activates another window
**SW_MAXIMIZE**  = Maximizes the specified window
**SW_MINIMIZE** = Minimizes the specified window and activates the next top-level window in the z-order
**SW_RESTORE**  = Activates and displays the window.
**SW_SHOW**  = Activates the window and displays it in its current size and position.
**SW_SHOWDEFAULT**  = Sets the show state based on the SW_ flag specified in the STARTUPINFO
**SW_SHOWMAXIMIZED**  = Activates the window and displays it as a maximized window.
**SW_SHOWMINIMIZED**  = Activates the window and displays it as a minimized window.
**SW_SHOWMINNOACTIVE**  = Displays the window as a minimized window. The active window remains active
**SW_SHOWNA**  = Displays the window in its current state. The active window remains active.
**SW_SHOWNOACTIVATE**  = Displays a window in its most recent size and position. The active window remains active.
**SW_SHOWNORMAL**  = Activates and displays a window. If the window is minimized or maximized, Windows restores it to its original size and position.

Коды ошибок:

0 = The operating system is out of memory or resources
2 = The specified file was not found
3 = The specified path was not found
5 = Windows 95 only: The operating system denied access to the specified file
8 = Windows 95 only: There was not enough memory to complete the operation
10 = Wrong Windows version
11 = The .EXE file is invalid (non-Win32 .EXE or error in .EXE image)
12 = Application was designed for a different operating system
13 = Application was designed for MS-DOS 4.0
15 = Attempt to load a real-mode program
16 = Attempt to load a second instance of an application with non-readonly data segments
19 = Attempt to load a compressed application file
20 = Dynamic-link library (DLL) file failure
26 = A sharing violation occurred
27 = The filename association is incomplete or invalid
28 = The DDE transaction could not be completed because the request timed out
29 = The DDE transaction failed
30 = The DDE transaction could not be completed because other DDE transactions were being processed
31 = There is no application associated with the given filename extension
32 = Windows 95 only: The specified dynamic-link library was not found

```

```ad-code
collapse: none
title: Код

```pascal
//Start an application:
ShellExecute(Handle, 'open', PChar('c:\test\app.exe'), nil, nil, SW_SHOW); 

//Start NotePad and load a file (the system "knows" the location of NotePad.exe, therefore we don't have to specify the full path):
ShellExecute(Handle, 'open', PChar('notepad'), PChar('c:\test\readme.txt'), nil, SW_SHOW);

//Print a document:
ShellExecute(Handle, 'print', PChar('c:\test\test.doc'), nil, nil, SW_SHOW);  

//Note: probably you will see the window of Word open very briefly, but it is closed automatically.

//Open an HTML page, local or remote:
ShellExecute(Handle, 'open', PChar('http://www.google.com/'), nil, nil, SW_SHOW);

//You can do the previuos trick with any type of registered data-file, e.g. open a Text file:
ShellExecute(Handle, 'open', PChar('c:\test\readme.txt'), nil, nil, SW_SHOW);

//HTML Help File:
ShellExecute(Handle, 'open', PChar('c:\windows\help\calc.chm'), nil, nil, SW_SHOW);

//Explore a folder with Windows Explorer:
ShellExecute(Handle, 'explore', PChar('c:\windows)', nil, nil, SW_SHOW);

//Run a DOS command and return immediately:
ShellExecute(Handle, 'open', PChar('command.com'), PChar('/c copy file1.txt file2.txt'), nil, SW_SHOW);

//Run a DOS command and keep the DOS-window open ("stay in DOS"):
ShellExecute(Handle, 'open', PChar('command.com'), PChar('/k dir'), nil, SW_SHOW);

//Run an executable and show it:
filename := 'c:\program.exe';
ShellExecute(handle,'open',PChar(filename), '','',SW_SHOWNORMAL);

//Run an executable and minimize it:
filename := 'c:\program.exe';
ShellExecute(handle,'open',PChar(filename), '','',SW_MINIMIZE);

//Run an executable and maximize it:
filename := 'c:\program.exe';
ShellExecute(handle,'open',PChar(filename), '','',SW_MAXIMIZE);

//Run an executable and hide it:
filename := 'c:\program.exe';
ShellExecute(handle,'open',PChar(filename), '','',SW_HIDE);

//Run an executable with parameters:
filename := 'c:\program.exe';
parameters := '-c -i -v';
ShellExecute(handle,'open',PChar(filename), PChar(parameters), '', SW_SHOWNORMAL);

//Send a mail with attachment
ShellExecute(Self.Handle,
             nil,
             'mailto:' +
             'jiten.g.s001@gmail.com' +
             '?Subject=Test Message Subject' +
             '&Body=Test Message Body' +
             '&Attach="c:\Mail\attachment.txt"',
             nil,
             nil,
             SW_NORMAL);

```

```ad-result



```

```ad-remark



```

```ad-simcom



```

```ad-code
collapse: none
title: Как получить результат выполнения ShellExecute

URL: [Using ShellExecute's return value](https://www.tweaking4all.com/forum/delphi-lazarus-free-pascal/delphi-using-shellexecutes-return-value/)

```pascal
procedure TForm1.Button1Click(Sender: TObject);
var
 errorcode: integer;
begin
 errorcode := ShellExecute(0, 'open', pchar('c:test.txt'), nil, nil, SW_NORMAL);
 case errorcode of
  2:showmessage('file not found');
  3:showmessage('path not found');
  5:showmessage('access denied');
  8:showmessage('not enough memory');
  32:showmessage('dynamic-link library not found');
  26:showmessage('sharing violation');
  27:showmessage('filename association incomplete or invalid');
  28:showmessage('DDE request timed out');
  29:showmessage('DDE transaction failed');
  30:showmessage('DDE busy');
  31:showmessage('no application associated with the given filename extension');
 end;
 
{
 SE_ERR_FNF              = 2;
 SE_ERR_PNF              = 3;
 SE_ERR_ACCESSDENIED     = 5;
 SE_ERR_OOM              = 8;
 SE_ERR_DLLNOTFOUND      = 32;
 SE_ERR_SHARE            = 26;
 SE_ERR_ASSOCINCOMPLETE  = 27;
 SE_ERR_DDETIMEOUT       = 28;
 SE_ERR_DDEFAIL          = 29;
 SE_ERR_DDEBUSY          = 30;
 SE_ERR_NOASSOC          = 31;
 }
end;
```