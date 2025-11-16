# NIKATU-decompiled
vb.mi/nikatu.exe


flowchart:

A[Start FUN_0054c1d0] --> B[Setup VB error handling]
B --> C[Clear output Variant param_3 (VariantClear/VT_EMPTY)]
C --> D[Read Variant argument as mode (CInt/Coerce)]

D --> E{mode == 0?}
E -- Yes --> F[Mode 0 block]
E -- No --> H{mode == 1?}

%% MODE 0  (GetWindowsDirectoryA)
F --> F1[Load S (BSTR) from this+0x34]
F1 --> F2[Prepare ANSI buffer from S via VB runtime]
F2 --> F3[Call KERNEL32!GetWindowsDirectoryA via thunk_GetWindowsDirectoryA]
F3 --> F4[Capture LastError into VB Err.LastDllError]
F4 --> F5[Truncate S: S = Left$(S, returned_length)]
F5 --> G[Store S (updated) back into this+0x34]

%% MODE 1  (GetSystemDirectoryA)
H -- Yes --> I[Mode 1 block]
H -- No --> G

I --> I1[Load S (BSTR) from this+0x34]
I1 --> I2[Prepare ANSI buffer from S via VB runtime]
I2 --> I3[Call KERNEL32!GetSystemDirectoryA via thunk_GetSystemDirectoryA]
I3 --> I4[Capture LastError into VB Err.LastDllError]
I4 --> I5[Truncate S: S = Left$(S, returned_length)]
I5 --> G[Store S (updated) back into this+0x34]

%% COMMON POST-PROCESSING (normalize path)
G --> J[Set local S = current this+0x34]
J --> K[Compute T = Right$(S, 1) if Len(S) > 0]
K --> L{T == '\' (DAT_00409afc)?}
L -- Yes --> M[If Len(S) > 0 then S = Left$(S, Len(S) - 1)]
L -- No --> N[Skip trailing backslash trim]

M --> O[Copy S into output Variant param_3 (__vbaVarMove)]
N --> O

O --> P[Cleanup temporaries / locals (strings, variants)]
P --> Q[Return]
