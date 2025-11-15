# NIKATU-decompiled
vb.mi/nikatu.exe


flowchart:

  A[Start FUN_0054c1d0] --> B[Setup VB error handling]
  B --> C[Clear output (*param_3 = 0)]
  C --> D[Read Variant argument as mode]

  D --> E{mode == 0?}
  E -- Yes --> F[Mode 0 block]
  E -- No --> H{mode == 1?}

  %% MODE 0
  F --> F1[Load S from this+0x34]
  F1 --> F2[Convert S to ANSI]
  F2 --> F3[Call GetWindowsDirectoryA via FUN_00409854]
  F3 --> F4[Set system error, convert back to Unicode]
  F4 --> F5[Truncate S via Left(S, N) using returned length]
  F5 --> G[Store S back into this+0x34]

  %% MODE 1
  H -- Yes --> I[Mode 1 block]
  H -- No --> G
  I --> I1[Load S from this+0x34]
  I1 --> I2[Convert S to ANSI]
  I2 --> I3[Call KERNEL32 API via FUN_004098a0 (mode 1)]
  I3 --> I4[Set system error, convert back to Unicode]
  I4 --> I5[Truncate S via Left(S, N) using returned length]
  I5 --> G

  %% COMMON POST-PROCESSING
  G --> J[Set S = current this+0x34]
  J --> K[T = Right(S, 1)]
  K --> L{T == marker const? (DAT_00409afc)}
  L -- Yes --> M[If Len(S) > 0: S = Left(S, Len(S) - 1)]
  L -- No --> N[Skip trailing-char trim]

  M --> O[Copy S into output Variant (param_3)]
  N --> O
  O --> P[Cleanup temp vars/strings]
  P --> Q[Return]
