# NIKATU-decompiled
vb.mi/nikatu.exe


flowchart:

  A[Start FUN_0054c1d0] --> B[Setup VB error handling]
  B --> C[Clear output (*param_3 = 0)]
  C --> D[Read Variant argument as mode]
  
  D --> E{mode == 0?}
  E -- Yes --> F[Mode 0 block]
  E -- No --> H{mode == 1?}

  
  F --> F1[Load S from this+0x34]
  F1 --> F2[Convert S to ANSI]
  F2 --> F3[Call FUN_00409854<br/>(lazy-resolve KERNEL32 API A)]
  F3 --> F4[Set system error, convert back to Unicode]
  F4 --> F5[Truncate S via Left$(S, N)]
  F5 --> G[Store S back into this+0x34]


  H -- Yes --> I[Mode 1 block]
  H -- No --> G
  I --> I1[Load S from this+0x34]
  I1 --> I2[Convert S to ANSI]
  I2 --> I3[Call FUN_004098a0<br/>(lazy-resolve KERNEL32 API B)]
  I3 --> I4[Set system error, convert back to Unicode]
  I4 --> I5[Truncate S via Left$(S, N)]
  I5 --> G


  G --> J[Set S = current this+0x34]
  J --> K[T = Right$(S, 1)]
  K --> L{T == marker const<br/>(DAT_00409afc)?}
  L -- Yes --> M[If Len(S) > 0:<br/>S = Left$(S, Len(S) - 1)]
  L -- No --> N[Skip trailing-char trim]


  M --> O[Copy S into output Variant (param_3)]
  N --> O
  O --> P[Cleanup temp vars/strings]
  P --> Q[Return]
